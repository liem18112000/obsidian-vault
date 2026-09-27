---
ai_hash: a6154cd9d580c68f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities:
- deployment-view-uat.png
- deployment-view-uat.svg
- resvg-js
- leo-customer360/deployments/
- UAT deployment diagram
- deployment-view-uat.excalidraw
- Excalidraw
- Node
- ImageMagick
- browser
- Windows fonts
- Consolas
- monospace
- Segoe UI
- system-ui
- Resvg
- fs
- npm
- Portainer CSRF origin-invalid behind a reverse proxy - expose it directly
source: session 2026-08-19
status: seedling
tags:
- customer360
- excalidraw
- svg
- png
- resvg
- docs
- diagram
title: Re-exporting the deployment-view PNG from its SVG with resvg-js
type: howto
---

# Re-exporting the deployment-view PNG from its SVG with resvg-js

`leo-customer360/deployments/` keeps the UAT deployment diagram as THREE parallel files:
`deployment-view-uat.excalidraw` (editable JSON), `deployment-view-uat.svg`
(hand-authored vector, the real source of the image), and `deployment-view-uat.png`
(a **2x raster of the SVG**, 2800x2000 from a 1400x1000 viewBox). The PNG is derived from
the SVG, NOT from the excalidraw — keep all three in sync when the design changes.

**Re-export the PNG headlessly** (no browser, no ImageMagick — none installed on this box).
The SVG uses only standard Windows fonts (`Consolas, monospace` + `Segoe UI, system-ui`),
so `@resvg/resvg-js` (prebuilt Node binary, no native build) renders it faithfully:

```js
const {Resvg}=require('@resvg/resvg-js');
const fs=require('fs');
const svg=fs.readFileSync('deployment-view-uat.svg');
const img=new Resvg(svg,{fitTo:{mode:'width',value:2800},font:{loadSystemFonts:true}}).render();
fs.writeFileSync('deployment-view-uat.png', img.asPng());
```
`npm i @resvg/resvg-js@2` in a scratch dir first. The SVG already paints its own white bg
(`<rect width="100%" height="100%" fill="#ffffff"/>`), so no `background` option needed.

**Gotcha — the excalidraw JSON drifts.** A past "refresh" appended new label elements with
the SAME ids as the old ones instead of replacing them, leaving duplicate ids + a stale
`:9443/:19999 (SSO)` label. Fix = dedupe elements keeping the FIRST occurrence per id
(the first block held the correct current labels). The SVG/PNG were fine; only the
excalidraw needed cleaning.

Related: [[Portainer CSRF origin-invalid behind a reverse proxy - expose it directly]]

%% ai-graph-start %%

**Related notes:**
- [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]
- [[Editing Excalidraw diagrams with committed SVG exports needs both files updated]]
- [[Render Excalidraw-style hand-drawn PNGs headlessly with rough.js in the Playwright browser]]
- [[Render .excalidraw to PNG headlessly with excalidraw-brute-export-cli]]
- [[Excalidraw fontFamily codes + .excalidraw.png can drift out of sync]]

**Relations:**
- deployment-view-uat.png — *re-exported from* — deployment-view-uat.svg
- deployment-view-uat.png — *exported using* — resvg-js
- leo-customer360/deployments/ — *keeps* — UAT deployment diagram
- UAT deployment diagram — *represented as* — deployment-view-uat.excalidraw
- UAT deployment diagram — *represented as* — deployment-view-uat.svg
- UAT deployment diagram — *represented as* — deployment-view-uat.png
- deployment-view-uat.png — *derived from* — deployment-view-uat.svg
- deployment-view-uat.png — *is a* — 2x raster
- 2x raster — *of* — deployment-view-uat.svg
- resvg-js — *is a* — prebuilt Node binary
- deployment-view-uat.svg — *uses* — Windows fonts
- Windows fonts — *includes* — Consolas
- Windows fonts — *includes* — monospace
- Windows fonts — *includes* — Segoe UI
- Windows fonts — *includes* — system-ui
- Resvg — *is part of* — resvg-js
- fs — *is a* — Node module
- npm — *installs* — resvg-js
- deployment-view-uat.excalidraw — *is* — editable JSON
- deployment-view-uat.excalidraw — *can* — drift
- deployment-view-uat.excalidraw — *had* — duplicate ids
- deployment-view-uat.excalidraw — *needed* — cleaning
- deployment-view-uat.svg — *is* — hand-authored vector
- deployment-view-uat.svg — *is the* — source of image
- deployment-view-uat.png — *has dimensions* — 2800x2000
- deployment-view-uat.svg — *has viewBox* — 1400x1000
- resvg-js — *renders* — deployment-view-uat.svg
- Portainer CSRF origin-invalid behind a reverse proxy - expose it directly — *is related to* — this note
- resvg-js — *replaces* — browser
- resvg-js — *replaces* — ImageMagick
- deployment-view-uat.excalidraw — *is a type of* — Excalidraw

%% ai-graph-end %%