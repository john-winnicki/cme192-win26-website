# Deploying the CME192 Jekyll Website to Stanford AFS

This guide covers the exact workflow we used to fix the CSS/baseurl problem and deploy the site to:

**https://stanford.edu/class/cme192/**

---

## 0) One-time setup check (important)

### A. Make sure you run Jekyll through Bundler
Always use `bundle exec`:

✅ Correct:
```bash
bundle exec jekyll build
bundle exec jekyll serve
```

❌ Not recommended (often fails with “command not found” or version mismatch):
```bash
jekyll build
jekyll serve
```

### B. Confirm `_config.yml` matches the Stanford URL
Open `_config.yml` and ensure these match your real public path:

```yaml
url: "https://stanford.edu"
baseurl: "/class/cme192"
```

If these are wrong, links/CSS may break after deployment.

> Note: We fixed the theme to use Jekyll’s `relative_url`, so asset paths work correctly with `baseurl`.

---

## 1) Preview the site locally

### Option A (recommended): preview using the same baseurl as production
Run:

```bash
bundle exec jekyll serve
```

Then open the URL **printed by Jekyll**. With `baseurl: /class/cme192`, it will typically be:

- **http://127.0.0.1:4000/class/cme192/**

### Option B: preview at root (sometimes more convenient)
Run:

```bash
bundle exec jekyll serve --baseurl ""
```

Then open:

- **http://127.0.0.1:4000/**

Both options should work now that asset URLs are baseurl-safe.

---

## 2) Build the production site

From the repo root:

```bash
bundle exec jekyll clean
JEKYLL_ENV=production bundle exec jekyll build
```

This generates the final static website into:

- `./_site/`

**Only `_site/` gets uploaded.**  
Do **not** manually edit files inside `_site/` (they are regenerated every build).

---

## 3) Upload the site to Stanford AFS

Upload the **contents** of `_site/` to the class web directory:

```bash
rsync -av --delete _site/ winnicki@cardinal2:/afs/ir/class/cme192/WWW/
```

Key details:
- The trailing slash in `_site/` matters: it copies the contents, not the folder itself.
- `--delete` removes old files on the server that no longer exist locally (prevents stale pages).

---

## 4) Verify the deployment

Open:

**https://stanford.edu/class/cme192/**

If you don’t see changes right away, hard refresh:
- macOS Chrome: **Cmd + Shift + R**
- Safari: **Cmd + Option + R**

---

## 5) Quick sanity checks if styling breaks

If the page looks like “raw HTML” (unstyled), it usually means CSS didn’t load.

### Check CSS exists locally (after build)
```bash
ls _site/doks-theme/assets/css/style.css
```

### Check CSS exists on AFS (after upload)
```bash
ssh winnicki@cardinal2
ls /afs/ir/class/cme192/WWW/doks-theme/assets/css/style.css
```

### Check for 404s in the browser
Open DevTools → Network and look for missing assets.  
With `baseurl: /class/cme192`, CSS/JS/image URLs should start with:

- `/class/cme192/...`

---

## TL;DR (copy/paste)

```bash
bundle exec jekyll clean
JEKYLL_ENV=production bundle exec jekyll build
rsync -av --delete _site/ winnicki@cardinal2:/afs/ir/class/cme192/WWW/
```
