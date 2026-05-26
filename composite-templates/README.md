# Composite templates — landing site `#customize` section

Three HTML pages that render the 4:5 composite images used by the
`#customize` section of `index.html`. Open each in a browser, drop
in any image assets that are still TODO placeholders, then take a
1200×1500 screenshot and save as `webp` next to `index.html`'s
expected paths.

## Why HTML and not Figma

- Reproducible — change the asset, re-screenshot, done
- No design tools needed; just a browser
- Same Tailwind colours + typography as the live site so the
  composite blends with the surrounding `#customize` card frames
- Easy to tweak composition by editing CSS instead of redoing the
  whole flat-lay

## Final filenames (where to save the screenshots)

```
docs/legal/site/images/customize/v2-themes-flat-lay.webp
docs/legal/site/images/customize/v2-decks-row.webp
docs/legal/site/images/customize/v2-tables-grid.webp
```

## Capture recipe (pick one)

### Option A — Browser DevTools (easiest, recommended)

1. Open the template HTML in Chrome
2. DevTools (⌘⌥I) → Device Toolbar (⌘⇧M)
3. Set custom dimensions: **1200 × 1500**
4. Set DPR to **2** (for crisp Retina output)
5. Three-dot menu in device toolbar → **Capture full size screenshot**
6. PNG comes out at 2400×3000 (DPR 2) — that's fine, downscale on
   `cwebp` step below

### Option B — Headless Chrome (scriptable)

```bash
chromium --headless --disable-gpu \
  --window-size=1200,1500 \
  --screenshot=/tmp/themes.png \
  --hide-scrollbars \
  file://$(pwd)/docs/legal/site/composite-templates/themes-flat-lay.html
```

### Option C — Playwright (cleanest, needs node)

```bash
npx playwright screenshot \
  --viewport-size=1200,1500 \
  --device-scale-factor=2 \
  docs/legal/site/composite-templates/themes-flat-lay.html /tmp/themes.png
```

### Convert PNG → WebP (q85)

```bash
cwebp -q 85 -resize 1200 1500 /tmp/themes.png \
  -o docs/legal/site/images/customize/v2-themes-flat-lay.webp
```

## Image placeholders

Each template loads asset images via relative paths under
`../../../../src/assets/book/` etc. — that path reaches the bundled
React Native source assets from the template directory. If any path
shows as a broken image in browser:

1. Check the bundled asset actually exists in the repo
   (`ls src/assets/book/`)
2. For non-bundled assets (Plus theme covers, non-default decks, non-
   baseline tables), either copy the asset locally into
   `docs/legal/site/images/customize/sources/` OR replace the `src`
   in the template with the Supabase Storage public URL of the
   `cosmetic-previews/` bucket equivalent.

The templates render placeholders (dashed boxes with the asset
filename) when an image fails to load — so missing assets are
obvious before you take the screenshot.
