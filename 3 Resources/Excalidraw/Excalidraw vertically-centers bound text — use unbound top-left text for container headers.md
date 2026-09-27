---
ai_hash: 38a25161143f204c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26
status: seedling
tags:
- excalidraw
- diagrams
- gotcha
- json
title: Excalidraw vertically-centers bound text — use unbound top-left text for container
  headers
type: lesson
---

# Excalidraw vertically-centers bound text — use unbound top-left text for container headers

In the Excalidraw JSON model, **text bound to a container** (a text element with `containerId` set, listed in the rectangle's `boundElements`) is always **vertically centered** inside that shape regardless of `verticalAlign`. That's fine for leaf boxes (centered label) but wrong for **container/group headers** (VPC/subnet/host boxes) whose title should sit at the **top-left**.

Fix: render container headers as **standalone, unbound** text elements (`containerId: null`) positioned at `x+12, y+10`; keep bound centered text only for leaf boxes. Otherwise the header floats in the middle of a large container when the file is opened for editing (even if a self-rendered SVG/PNG export looks correct).

Also: `roughness: 0` makes excalidraw draw clean straight lines, so a plain hand-rendered SVG closely matches the excalidraw look — useful for keeping an editable source and its exported image consistent.

Related: [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]

## Related

- [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]
- [[Editing an Excalidraw .excalidraw JSON programmatically]]
- [[Excalidraw text does not auto-wrap or auto-center]]
- [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]
- [[Excalidraw container-bound text does not auto-wrap in the render script]]

%% ai-graph-end %%