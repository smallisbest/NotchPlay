# NotchPlay — Landing Page

Static site for NotchPlay. Deploy to any static host (GitHub Pages, Netlify, Cloudflare Pages, S3, etc.).

## Files

```
web/
├── index.html          ← Bilingual (en default, ko auto-detected)
├── sample_ko.zip       ← symlink → ../sample_ko/sample_ko.zip
└── README.md
```

`index.html` references `sample_ko.zip` as a same-directory relative path, so the zip must be **colocated** with the HTML wherever the site is deployed.

## Local preview

```bash
cd web
python3 -m http.server 8080
# → http://localhost:8080
```

Python's `http.server` follows the symlink automatically.

## Deployment

### GitHub Pages
Symlinks are preserved on GitHub — just push `web/` as the Pages source.

### Netlify / Cloudflare Pages
Symlinks may be stripped. Replace the symlink with a real copy before publishing:

```bash
cd web
rm sample_ko.zip
cp ../sample_ko/sample_ko.zip sample_ko.zip
```

### S3 / generic static host
Upload `index.html` and the resolved `sample_ko.zip` (not the symlink) to the same bucket/prefix.

## Language handling

- Default language: **English** (`<html lang="en">`).
- Korean browsers (`navigator.language` starts with `ko`) see Korean on first load.
- User's choice (via the toggle in the nav) is persisted in `localStorage` under `notchplay.lang`.
- CSS uses `html[lang="en"] [data-lang="ko"] { display: none }` and vice versa to swap content without reloading.

## Updating sample_ko.zip

Whenever you regenerate the sample pack:

```bash
cd ../sample_ko
python3 build_sample.py
python3 build_sample_zip.py
```

The symlink in `web/` picks up the new file automatically — no manual copy.
