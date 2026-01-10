# CME192 Win26 Website — Local Setup Notes (macOS Tahoe / Apple Silicon)

This document summarizes **all fixes** we needed to get the site running locally on a modern macOS (Apple Silicon) machine, and provides a **single copy/paste script** to reproduce the setup from a fresh `git clone`.

---

## What broke and why (high level)

1. **macOS system Ruby is not usable**
   - The preinstalled Ruby is **old** and installing gems into `/Library/Ruby/...` fails without elevated permissions.
   - Even with `sudo`, modern Bundler/Jekyll won’t run on that Ruby.

2. **The repo’s dependency stack is “old GitHub Pages / Jekyll”**
   - This repo uses the `github-pages` gem stack (Jekyll 3.x era).
   - That ecosystem works, but it requires a **modern, user-installed Ruby** and a couple compatibility tweaks.

3. **Lockfile pinned an ancient Bundler**
   - Bundler will try to use the lockfile’s recorded Bundler version.
   - If that version is too old for your Ruby, you’ll see crashes when Bundler tries to auto-install/downgrade itself.

4. **Native gem build issues (`ffi`)**
   - On Apple Silicon + modern macOS, old `ffi` builds can fail while compiling native extensions.
   - Installing **Homebrew `libffi` + `pkg-config`** and telling Bundler to use system `libffi` makes this reliable.

5. **Ruby 3.4 “bundled stdlib gems”**
   - With Ruby 3.4, some libraries that used to be “stdlib” behave like **bundled gems** when using Bundler.
   - We hit runtime errors like:
     - `cannot load such file -- base64`
     - `cannot load such file -- bigdecimal`
   - Fix: explicitly add these gems to the bundle (Gemfile).

6. **Jekyll accidentally built files inside your gems**
   - If you install gems into `vendor/bundle` inside the repo, Jekyll can mistakenly crawl gem directories that contain `_posts/…` templates.
   - We saw:
     - `Invalid date '<%= Time.now.strftime(...) %>'` from a file inside `vendor/bundle/.../jekyll.../site_template/_posts/...`
   - Fix options:
     - **Recommended:** install gems outside the repo
     - Or: exclude `vendor/bundle` in `_config.yml`

---

## Recommended “make future clones painless” repo changes

If you want future clones to “just work” without patching every time, commit these changes to your fork:

### 1) Add Ruby 3.4 bundled gems to your `Gemfile`

Add these lines to `Gemfile` (if not already present):

```ruby
# Ruby 3.4+: some stdlib pieces are bundled gems under Bundler
gem "base64"
gem "bigdecimal"
gem "logger"    # optional; silences warning and future-proofs Ruby 3.5+
```

Then:

```bash
bundle install
git add Gemfile Gemfile.lock
git commit -m "Fix Ruby 3.4 bundled stdlib gems (base64/bigdecimal/logger)"
```

### 2) If you install gems into `vendor/bundle`, exclude it in `_config.yml`

Add or extend:

```yaml
exclude:
  - vendor
  - vendor/bundle
  - .bundle
  - Gemfile
  - Gemfile.lock
  - node_modules
```

Then:

```bash
git add _config.yml
git commit -m "Exclude bundler directories from Jekyll build"
```

> If you follow the script below, it installs gems outside the repo, so this exclusion is not strictly required.

---

## One copy/paste script (fresh machine → running site)

### What it does
- Ensures Xcode Command Line Tools are installed (required for native gems).
- Installs Homebrew (if missing).
- Installs `chruby` + `ruby-install`.
- Installs Ruby **3.4.1** under `~/.rubies/` (no system Ruby usage).
- Installs Bundler.
- Clones the repo.
- Applies the **minimum** compatibility patches (Gemfile for bundled stdlib gems; lockfile Bundler version).
- Installs gems **outside the repo** (so Jekyll won’t scan them).
- Runs the site (`./run.sh` if present, otherwise `bundle exec jekyll serve`).

### Copy/paste (bash)

```bash
#!/usr/bin/env bash
set -euo pipefail

# ---- config you may edit ----
REPO_URL="https://github.com/john-winnicki/cme192-win26-website.git"
REPO_DIR="cme192-win26-website"
RUBY_VERSION="3.4.1"
# Install bundle outside the repo to avoid Jekyll scanning vendor/bundle
BUNDLE_PATH="$HOME/.bundle/gems/$REPO_DIR"
# -----------------------------

echo "==> Checking Xcode Command Line Tools..."
if ! xcode-select -p >/dev/null 2>&1; then
  echo "Xcode Command Line Tools not found."
  echo "Running: xcode-select --install"
  echo "Complete the GUI install, then re-run this script."
  xcode-select --install || true
  exit 1
fi

echo "==> Checking Homebrew..."
if ! command -v brew >/dev/null 2>&1; then
  echo "Homebrew not found. Installing Homebrew..."
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

  # Try to add brew to PATH for this shell session (Apple Silicon default)
  if [[ -x /opt/homebrew/bin/brew ]]; then
    eval "$(/opt/homebrew/bin/brew shellenv)"
  elif [[ -x /usr/local/bin/brew ]]; then
    eval "$(/usr/local/bin/brew shellenv)"
  fi
fi

# Ensure brew is on PATH now
if ! command -v brew >/dev/null 2>&1; then
  echo "ERROR: brew is still not on PATH."
  echo "Open a new terminal or add brew to your PATH, then re-run this script."
  exit 1
fi

echo "==> Installing build deps + Ruby tooling..."
brew update
brew install chruby ruby-install pkg-config libffi

# Load chruby into this shell session
CHRUBY_SH="$(brew --prefix)/opt/chruby/share/chruby/chruby.sh"
AUTO_SH="$(brew --prefix)/opt/chruby/share/chruby/auto.sh"
# shellcheck disable=SC1090
source "$CHRUBY_SH"
# shellcheck disable=SC1090
source "$AUTO_SH"

echo "==> Installing Ruby $RUBY_VERSION (if missing)..."
if [[ ! -d "$HOME/.rubies/ruby-$RUBY_VERSION" ]]; then
  ruby-install ruby "$RUBY_VERSION"
fi

echo "==> Activating Ruby $RUBY_VERSION..."
chruby "ruby-$RUBY_VERSION"
ruby -v
which ruby

echo "==> Installing Bundler..."
gem install bundler

echo "==> Cloning repo (or updating if it already exists)..."
if [[ -d "$REPO_DIR/.git" ]]; then
  git -C "$REPO_DIR" pull --ff-only
else
  git clone "$REPO_URL" "$REPO_DIR"
fi

cd "$REPO_DIR"

echo "==> Ensuring Gemfile contains Ruby 3.4 bundled stdlib gems..."
touch Gemfile
if ! grep -qE '^\s*gem\s+["'\'']base64["'\'']' Gemfile; then
  cat >> Gemfile <<'EOF'

# Ruby 3.4+: some stdlib pieces are bundled gems under Bundler
gem "base64"
EOF
fi
if ! grep -qE '^\s*gem\s+["'\'']bigdecimal["'\'']' Gemfile; then
  cat >> Gemfile <<'EOF'
gem "bigdecimal"
EOF
fi
# logger isn't required today, but it removes warnings and helps with Ruby 3.5+
if ! grep -qE '^\s*gem\s+["'\'']logger["'\'']' Gemfile; then
  cat >> Gemfile <<'EOF'
gem "logger"
EOF
fi

echo "==> Patching lockfile Bundler version (prevents Bundler downgrade crash)..."
if [[ -f Gemfile.lock ]]; then
  # Get installed bundler version like "4.0.3"
  BUNDLER_VER="$(bundle -v | awk '{print $3}')"
  # Replace the version line after "BUNDLED WITH"
  perl -0777 -i -pe "s/(BUNDLED WITH\\n\\s*)\\d+(?:\\.\\d+)*\\n/\\\$1$BUNDLER_VER\\n/s" Gemfile.lock || true
fi

echo "==> Configuring Bundler install path outside the repo..."
bundle config set --local path "$BUNDLE_PATH"

echo "==> Making ffi builds reliable on Apple Silicon..."
export PKG_CONFIG_PATH="$(brew --prefix libffi)/lib/pkgconfig"
bundle config set --local build.ffi "--enable-system-libffi --with-ffi-dir=$(brew --prefix libffi)"

echo "==> Installing gems..."
bundle install

echo "==> Cleaning old build artifacts..."
rm -rf _site .jekyll-cache .sass-cache

echo "==> Starting the site..."
if [[ -x "./run.sh" ]]; then
  ./run.sh
else
  bundle exec jekyll serve
fi

echo ""
echo "==> If Jekyll started successfully, open: http://127.0.0.1:4000"
echo "==> Tip: To use this Ruby in future shells, add to ~/.zshrc:"
echo "    source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh"
echo "    source $(brew --prefix)/opt/chruby/share/chruby/auto.sh"
echo "    chruby ruby-$RUBY_VERSION"
```

---

## Troubleshooting quick reference

### “Could not locate Gemfile”
You ran `bundle …` from the wrong directory. `cd` into the repo root (where `Gemfile` is).

### Bundler says it’s using a lockfile generated with a very old Bundler
Patch the `BUNDLED WITH` section at the bottom of `Gemfile.lock` to match your installed Bundler, or remove the lockfile and regenerate it.

### `ffi` build errors
Install `pkg-config` + `libffi` and set:

```bash
export PKG_CONFIG_PATH="$(brew --prefix libffi)/lib/pkgconfig"
bundle config set --local build.ffi "--enable-system-libffi --with-ffi-dir=$(brew --prefix libffi)"
```

### Jekyll complains about files inside `vendor/bundle/.../site_template/_posts/...`
Don’t install gems into `vendor/bundle`, or add `vendor/bundle` to `_config.yml` `exclude:`.

---

## Notes on Ruby 3.3.4 (optional)

GitHub Pages often documents Ruby 3.3.x. If you try installing Ruby 3.3.4 and `ruby-install` fails with a macro error involving `RUBY_FUNCTION_NAME_STRING`, the workaround is to re-run with:

```bash
ruby-install ruby 3.3.4 -- rb_cv_function_name_string=__func__
```

(We didn’t need this once Ruby 3.4.1 + the bundled-gem fixes were in place.)
