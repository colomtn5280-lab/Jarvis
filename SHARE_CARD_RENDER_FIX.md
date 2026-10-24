# Share Card render fix — full-bleed 1080×1920

## Symptom
`DateMeUp-Cards-FullBleed.pdf`: every page is the correct 9:16 size and the image fills it, but inside each 1080×1920 PNG the card is only 360×640 in the top-left corner. The other ~89% is empty grey background. The card's rounded corners and drop shadow are also captured.

## Cause
The templates are authored at **360×640 CSS px** (the board size). The renderer opens a 1080×1920 viewport at `deviceScaleFactor: 1`, so the card paints at its natural 360×640 and the rest of the viewport is page background.

## Fix (pick one, A recommended)

### A. Render at 360×640 with 3× pixel density
```js
const page = await browser.newPage();
await page.setViewport({ width: 360, height: 640, deviceScaleFactor: 3 }); // => 1080×1920 PNG
await page.goto(cardUrl, { waitUntil: 'networkidle0' });
await page.evaluate(() => document.fonts.ready);
const el = await page.$('[data-share-card]');
const png = await el.screenshot({ type: 'png', omitBackground: false });
```
Playwright: `browser.newContext({ viewport: { width: 360, height: 640 }, deviceScaleFactor: 3 })`.

### B. Keep a 1080×1920 viewport and scale the card
```css
html, body { margin: 0; width: 1080px; height: 1920px; overflow: hidden; background: transparent; }
[data-share-card] { width: 360px; height: 640px; transform: scale(3); transform-origin: 0 0; }
```
Option B rasterises text at 1× and then scales it, so it can come out softer than A. Use A.

## Render-mode rules (both options)
Add `?render=1` (or a `.render` class on `<body>`). In render mode:
- Card root is exactly 360×640, `position: fixed; inset: 0`.
- `border-radius: 0`, `box-shadow: none`, no outer margin or padding.
- `html, body { margin: 0; background: <card background> }`. No grey page background.
- Hide all board chrome: template labels, mode tags, pickers, captions.
- Wait for `document.fonts.ready` (Sora, DM Sans, Caveat) and for all `<img>` to finish decoding before capturing.
- Capture the card element, not the full page.

## Acceptance check
For each of the 72 PNGs (36 templates × 2 modes):
- 1080×1920 px.
- Corner pixels (0,0), (1079,0), (0,1919), (1079,1919) match the card background, not #E8ECEF or white.
- No pixel column or row past x=360 / y=640 is uniformly page grey.

Quick check script (Node + sharp):
```js
const { data, info } = await sharp(file).raw().toBuffer({ resolveWithObject: true });
const px = (x, y) => data.slice((y * info.width + x) * info.channels, (y * info.width + x) * info.channels + 3);
console.assert(info.width === 1080 && info.height === 1920);
console.log(px(1079, 1919)); // must be the card colour
```

## PDF
Once the PNGs are right, the existing PDF step (612×1087.94 pt page, image at 0,0 full page) needs no change.
