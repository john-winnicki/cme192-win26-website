# Codex Task: Fix Jekyll theme asset URLs so they work with `baseurl` in BOTH dev + prod

## Context / Problem

When running:

- `bundle exec jekyll serve` (using `_config.yml` `baseurl: /cme192-win26`)

Jekyll serves the site at:

- `http://127.0.0.1:4000/cme192-win26/`

…but the theme templates currently emit asset URLs like:

- `/doks-theme/assets/css/style.css`

That path is missing the `baseurl` prefix, so the browser requests:

- `http://127.0.0.1:4000/doks-theme/assets/css/style.css`  (404)

Result: pages look like “raw HTML” / unstyled.

It *only* looks correct when serving at `/` (e.g., `--baseurl ""`).

## Goal

Make all theme asset URLs baseurl-aware using Jekyll’s built-in `relative_url` filter, so BOTH of these work:

- `bundle exec jekyll serve` (baseurl from `_config.yml`, likely `/cme192-win26`)
- `bundle exec jekyll serve --baseurl ""` (serve at `/`)

## Constraints

- **DO NOT edit anything under `_site/`** (generated output).
- Only edit source templates under `doks-theme/`.
- Keep paths pointing to the existing `/doks-theme/assets/...` structure.

---

## Edits to Apply

### 1) `doks-theme/_includes/site-head.html`

**Find** the stylesheet line (currently uses `jekyll.environment` and `site.baseurl`):

```html
<link rel="stylesheet" href="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/css/style.css">
````

**Replace with**:

```liquid
<link rel="stylesheet" href="{{ '/doks-theme/assets/css/style.css' | relative_url }}">
```

---

### 2) `doks-theme/_includes/site-header.html`

**Find** the logo image `src` line:

```liquid
<img src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/images/layout/logo.png">
```

**Replace with**:

```liquid
<img src="{{ '/doks-theme/assets/images/layout/logo.png' | relative_url }}">
```

---

### 3) `doks-theme/_includes/site-footer.html`

**Footer logo**

**Find**:

```liquid
<img src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/images/layout/logo-footer.png">
```

**Replace with**:

```liquid
<img src="{{ '/doks-theme/assets/images/layout/logo-footer.png' | relative_url }}">
```

**Scripts**

For each `<script ... src="...">` that looks like:

```liquid
<script src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/js/vendor/jquery.min.js"></script>
```

Replace the `src` value to use `relative_url`, e.g.:

```liquid
<script src="{{ '/doks-theme/assets/js/vendor/jquery.min.js' | relative_url }}"></script>
```

Do the same for all the others listed in grep output:

* `/doks-theme/assets/js/vendor/bootstrap/affix.min.js`
* `/doks-theme/assets/js/vendor/bootstrap/scrollspy.min.js`
* `/doks-theme/assets/js/vendor/matchHeight.min.js`
* `/doks-theme/assets/js/scripts.min.js`

Example final block:

```liquid
<script src="{{ '/doks-theme/assets/js/vendor/jquery.min.js' | relative_url }}"></script>
<script type="text/javascript" src="{{ '/doks-theme/assets/js/vendor/bootstrap/affix.min.js' | relative_url }}"></script>
<script type="text/javascript" src="{{ '/doks-theme/assets/js/vendor/bootstrap/scrollspy.min.js' | relative_url }}"></script>
<script type="text/javascript" src="{{ '/doks-theme/assets/js/vendor/matchHeight.min.js' | relative_url }}"></script>
<script type="text/javascript" src="{{ '/doks-theme/assets/js/scripts.min.js' | relative_url }}"></script>
```

---

### 4) `doks-theme/_includes/image.html`

**Find**:

```liquid
<tr><td><img src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/images/{{ include.image }}" style="width: 100%" /></td></tr>
```

**Replace with** (prevents accidental double slashes if `include.image` starts with `/`):

```liquid
<tr><td><img src="{{ include.image | remove_first: '/' | prepend: '/doks-theme/assets/images/' | relative_url }}" style="width: 100%" /></td></tr>
```

---

### 5) `doks-theme/_includes/instructor.html`

**Find**:

```liquid
<img class="headshot" src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/images/headshot/{{ include.image }}" style="text-align:center;">
```

**Replace with**:

```liquid
<img class="headshot" src="{{ include.image | remove_first: '/' | prepend: '/doks-theme/assets/images/headshot/' | relative_url }}" style="text-align:center;">
```

---

### 6) `doks-theme/_includes/staff.html`

There are two occurrences.

**Find** lines like:

```liquid
<img class="headshot" src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/images/headshot/{{ people.image }}" style="text-align:center;">
```

**Replace with**:

```liquid
<img class="headshot" src="{{ people.image | remove_first: '/' | prepend: '/doks-theme/assets/images/headshot/' | relative_url }}" style="text-align:center;">
```

Apply to both occurrences.

---

### 7) `doks-theme/_layouts/default.html`

**Find**:

```liquid
<img class="headshot" src="{% if jekyll.environment == 'production' %}{{ site.baseurl }}{% endif %}/doks-theme/assets/images/headshot/{{ people.image }}" style="text-align:center;">
```

**Replace with**:

```liquid
<img class="headshot" src="{{ people.image | remove_first: '/' | prepend: '/doks-theme/assets/images/headshot/' | relative_url }}" style="text-align:center;">
```

---

## Cleanup / Consistency Checks

After edits, ensure there are **no remaining** instances of the old conditional baseurl snippet inside `doks-theme/`:

```bash
grep -RIn "jekyll.environment == 'production'" doks-theme
grep -RIn "site.baseurl" doks-theme
```

(There should be no matches for these patterns in the theme includes/layouts after the change, unless used for non-asset purposes.)

---

## Verification Steps (Must Pass)

1. Clean + serve normally (uses `_config.yml` `baseurl`)

```bash
bundle exec jekyll clean
bundle exec jekyll serve
```

Expected:

* Server address shows `http://127.0.0.1:4000/cme192-win26/` (if baseurl is set)
* **No error** like:

  * `ERROR '/doks-theme/assets/css/style.css' not found.`

2. Confirm generated HTML has the baseurl prefix:

```bash
grep -n "doks-theme/assets/css/style.css" _site/index.html
```

Expected output contains:

* `/cme192-win26/doks-theme/assets/css/style.css`  (when baseurl is `/cme192-win26`)

3. Also verify serve at root works:

```bash
bundle exec jekyll serve --baseurl ""
```

Then `grep` should show:

* `/doks-theme/assets/css/style.css`

---

## Deliverables

* Commit the template changes above.
* Ensure local `bundle exec jekyll serve` loads CSS without needing `--baseurl ""`.

```

Save that as something like: `codex_fix_baseurl_assets.md` in the repo root and point Codex at it.
```

