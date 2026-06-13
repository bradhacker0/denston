# Denston Photo Gallery

A static, passcode-gated photo gallery hosted on GitHub Pages.

**Live site:** https://bradhacker0.github.io/denston/

Visitors browse thumbnails, select the photos they want (or **Select all**), then enter the
shared passcode to download their selection as a single ZIP.

## How it works

- `index.html` — the whole app (HTML + CSS + JS). Pulls [JSZip](https://stuk.github.io/jszip/) from a CDN to bundle downloads in the browser.
- `manifest.json` — list of photo filenames the page builds the grid from.
- `thumbs/` — ~600px gallery thumbnails (fast loading).
- `photos/` — full-resolution originals (the actual downloads).

## Changing the passcode

The passcode is a **light client-side gate only** — it deters casual visitors but the
full-resolution files are technically reachable by direct URL. To change it:

```sh
printf '%s' 'YOURNEWCODE' | shasum -a 256
```

Paste the resulting hex into the `PASSCODE_HASH` constant near the top of the `<script>` in `index.html`.

## Adding or removing photos

1. Put full-res JPGs in `photos/`.
2. Regenerate thumbnails: `for f in photos/*.jpg; do sips -Z 600 --out "thumbs/$(basename "$f")" "$f"; done`
3. Rebuild the manifest: `ls -1 photos/*.jpg | sed 's#photos/##' | sort` → update `manifest.json`.
4. Commit and push.
