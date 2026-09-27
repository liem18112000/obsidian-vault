---
ai_hash: 0870d73ab998c4af
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20, cd-process.excalidraw
status: seedling
tags:
- excalidraw
- diagrams
- gotcha
- rendering
title: Excalidraw render script does not auto-position containerId-bound text — set
  explicit x,y
type: lesson
---

# Excalidraw render script does not auto-position containerId-bound text — set explicit x,y

When generating `.excalidraw` JSON to be rasterized by the excalidraw-diagram skill's `render_excalidraw.py`, do **not** rely on `containerId` binding to place a label inside its shape. A text element bound via `containerId` (with the container listing it in `boundElements`) is only re-positioned/centered by the interactive Excalidraw app's layout pass; the headless render script draws the text at its own literal `x,y`. If you set the text to `x:0,y:0` expecting the container to move it, it renders at the **canvas top-left** instead of inside the shape.

**Fix:** position box labels as **free-floating** text with explicit `x,y`, `textAlign:"center"`, and `width` = the box width so centering math lands it in the shape. Multi-line: start `y = boxY + (boxH - lines*fontSize*lineHeight)/2`. This matched what already-working titles did. Discovered building `deployments/cd-process.excalidraw` for leo-customer360 — the two trigger ellipses came out empty with their labels stacked at (0,0). Related: [[Verify a GitHub Action's Node runtime and inputs via gh api on action.yml at a tag]] is unrelated; this pairs with general Excalidraw authoring.

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]
- [[Excalidraw container-bound text does not auto-wrap in the render script]]
- [[Excalidraw text does not auto-wrap or auto-center]]
- [[Editing an Excalidraw .excalidraw JSON programmatically]]
- [[Excalidraw offline renderer does not auto-wrap bound container text]]

%% ai-graph-end %%