---
ai_hash: f2e638cb4bfe662a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26 leo-customer360
status: seedling
tags:
- excalidraw
- svg
- png
- resvg
- diagrams
- nodejs
title: Generate Excalidraw triplet from one layout model, rasterize with @resvg/resvg-js
type: howto
---

# Generate Excalidraw triplet from one layout model, rasterize with @resvg/resvg-js

Build the diagram as data (nodes with x/y/w/h/fill/label, edges with from/to anchors + optional waypoints), then emit **both** an `.excalidraw` JSON file (editable source) and a hand-rendered `.svg` from that same layout model, and finally rasterize the SVG to PNG with **@resvg/resvg-js** — a native SVG→PNG binding that needs **no browser and no puppeteer** (`new Resvg(svg, {fitTo:{mode:'width',value:W*2}}).render().asPng()`).

Why: keeping one layout model as the source of truth means the editable `.excalidraw` and the exported image never drift. resvg installs cleanly on Windows via npm (2 packages, ~1 min) and is far lighter than a headless-Chromium export.

Used to regenerate the leo-customer360 `deployments/deployment-view-uat.{excalidraw,svg,png}` triplet (the repo keeps several such triplets referenced from README).

Related: [[Git Bash /tmp maps to C-tmp for Node fs on Windows]] · [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]

## Related

- [[Git Bash /tmp maps to C-tmp for Node fs on Windows]]
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]

%% ai-graph-start %%

**Related notes:**
- [[Re-exporting the deployment-view PNG from its SVG with resvg-js]]
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]
- [[Render .excalidraw to PNGSVG offline with render_excalidraw.py]]
- [[Render Excalidraw-style hand-drawn PNGs headlessly with rough.js in the Playwright browser]]
- [[Render .excalidraw to PNG headlessly with excalidraw-brute-export-cli]]

%% ai-graph-end %%