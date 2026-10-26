# ENT | Explore Work Order Items [Interaction]

A self-contained HTML interaction for Momentus Enterprise. Learners explore the
**Work order items** screen in two ways:

| Bar option | What the learner does |
| --- | --- |
| **Explore Columns** | Selects each column heading to see what it shows |
| **Explore Details** | Opens the slider and selects items to see their details |

The bar tracks progress ("0 of 2 viewed") and ticks each option once it is complete.
All items and quantities shown are illustrative.

> **Status:** PREVIEW build.
> **Internal content:** this file embeds captures of the Momentus product UI.
> Keep this repository **private** unless Momentus has approved public release.

## Run it

No build step, no dependencies, no network access needed.

```bash
open index.html            # macOS
# or serve it locally
python3 -m http.server 8000   # then visit http://localhost:8000
```

## How it is built

- **One file.** Fonts (Inter, icon font), CSS, JS and the UI capture are all inlined in `index.html` (~1.3 MB).
- **Fixed 1920 × 1176 canvas**: a 1920 × 1080 stage plus a 96 px selector bar. The whole canvas scales to fit the window (`fit()` in the outer script).
- **Two layers.** The outer page draws the "Choose what to explore" bar. The interaction itself runs inside an `<iframe id="stage">` loaded from an inline `srcdoc`.
- **`postMessage` contract** between the layers:

| Direction | Message | Purpose |
| --- | --- | --- |
| stage → bar | `{ simSelector: { items, current, visited, enabled } }` | Redraws the bar buttons and the "x of 2 viewed" count |
| stage → bar | `{ barHighlight: boolean }` | Toggles the red highlight ring on the bar |
| bar → stage | `{ selectExample: 0 \| 1 }` | Switches between Explore Columns and Explore Details |
| bar → stage | `{ simScale: number }` | Tells the stage the current scale factor |

## Embed it

Host it anywhere that serves static files (GitHub Pages, a CDN, your LMS's file hosting) and embed:

```html
<iframe src="https://YOUR-HOST/index.html"
        title="Explore Work Order Items"
        width="1920" height="1176"
        style="border:0;width:100%;aspect-ratio:1920/1176"></iframe>
```

## Editing

The product UI inside the stage comes from approved captures. Do not redraw or approximate it.
Edit copy and behavior in the stage script (the `D` config object holds the title, description,
learning points, column callouts and bar labels). Because the stage is stored as an escaped
string in `DOC`, prefer regenerating the file from its source over hand-editing where possible.

## GitHub Pages (optional)

A workflow in `.github/workflows/pages.yml` deploys the repo root on every push to `main`.
Enable it under **Settings → Pages → Source: GitHub Actions**.
Pages on a private repository requires a paid GitHub plan, and the published site is
public by default on most plans, so confirm visibility before enabling.
