# ASCII Asset Generator

Turn an image into ASCII art in the browser. Pick a character ramp, set the resolution, push the
lighting around until the characters land where you want them, then copy the text or download a PNG.

**Try it:** [20prateeksingh.github.io/ascii-tool](https://20prateeksingh.github.io/ascii-tool/) ·
also hosted at [prateeksingh.in/tools/ascii-generator](https://www.prateeksingh.in/tools/ascii-generator/)

The image never leaves your machine. It is read with `FileReader`, drawn to an offscreen `<canvas>`
and read back with `getImageData` — there is no upload, no server and no account. Open `index.html`
straight off the filesystem and it works, offline included.

## What it does

- **Nine ramps, or write your own.** Ten-step ASCII, currency symbols, binary, shaded blocks,
  quadrant blocks, squares, circles, triangles. Lightest character first, densest last.
- **Resolution from 20 to 400 columns.** Rows follow the image's aspect ratio, corrected for the
  width-to-height ratio of a monospace cell so the output keeps the input's shape.
- **Brightness, contrast and saturation.** Applied to the canvas context *before* the pixels are
  sampled, so they change what the ramp actually sees rather than tinting the result afterwards.
- **Colour, background and weight.** All three are part of the artifact, so all three end up in the
  exported PNG.
- **Copy as text, or download a PNG.** The PNG is re-drawn at a fixed 24px cell rather than being a
  screenshot of the preview, so a 300-column piece exports large enough to use.

## Two things it works out for you

**Which way round the ramp goes.** A dense glyph is ink. Whether ink means "the dark part of the
photo" or "the bright part" depends entirely on whether the ink is darker or lighter than what it
sits on — get it backwards and you have a photographic negative. So the polarity is derived from the
luminance of your colour against the luminance of the background, and the rail tells you which way it
currently reads. With no background, it assumes dark ink will end up on something light.

**Contrast you would not be able to see.** Choosing a light background while the ink is near-white
exports a blank. On a background change only — never while you are dragging the colour picker — ink
with almost no contrast against the new background is moved to the far end of it.

## Running it

There is no build step and there are no dependencies.

```bash
git clone https://github.com/20prateeksingh/ascii-tool.git
cd ascii-tool
open index.html
```

Any static host will serve it. A local server is only needed if your browser blocks something over
`file://`:

```bash
python3 -m http.server 8000
```

## Layout

| File | What is in it |
| --- | --- |
| `index.html` | Markup, and the whole app in one inline `<script>` |
| `index.css` | Every style. The design tokens are borrowed — see below |

**The design language is not this repo's own.** The `:root` block in `index.css` is lifted verbatim
from the [Design Context Kit's page](https://www.prateeksingh.in/tools/design-context-kit/), so the
two tools read as one family. It is kept as a straight copy on purpose: a token this app alone wants
goes in the second block, never in the borrowed one, which keeps a future re-copy a cheap diff.

`index.css` is byte-identical to the copy in the `prateeksingh.in` repo. The two `index.html` files
differ only in `<head>`: the hosted copy carries canonical and Open Graph tags plus that site's
first-party analytics, this one carries Google Analytics for the GitHub Pages deployment.

## Notes on the implementation

- **No fonts are fetched.** An earlier version pulled Inter and Source Code Pro from Google Fonts —
  two blocking third-party round-trips to land somewhere the system stack already was. `system-ui`
  and `ui-monospace` resolve to SF Pro and SF Mono on macOS, Segoe UI and Consolas on Windows.
- **Monospace is load-bearing.** The output is a grid of characters whose shape only survives if
  every glyph is one width.
- **`FONT_ASPECT` is a constant, not a measurement.** The entire grid is scaled by this one number,
  so fixing it at 0.6 keeps the output's proportions stable across machines even where the resolved
  monospace font differs slightly.
