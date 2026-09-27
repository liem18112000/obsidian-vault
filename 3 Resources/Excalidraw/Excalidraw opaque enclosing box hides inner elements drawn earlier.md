---
ai_hash: 95d83fc9950eeb19
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities: []
source: session 2026-09-23 exec-overview diagram
status: seedling
tags:
- excalidraw
- diagrams
- gotcha
title: Excalidraw opaque enclosing box hides inner elements drawn earlier
type: lesson
---

# Excalidraw opaque enclosing box hides inner elements drawn earlier

An Excalidraw `rectangle` used as a grouping / sandbox / trust-boundary wrapper around inner boxes will **paint over and hide those inner boxes** if it has a solid `backgroundColor` (e.g. `#ffffff`) AND appears later in the `elements` array — Excalidraw paints in array order, no z-index. The fix: give any enclosing/annotation box `backgroundColor: "transparent"` (keep the dashed stroke for the visible boundary), or draw it before the elements it surrounds.

Symptom seen once: a dashed red 'sandbox' box drawn after the RUN step rendered as an empty dashed box — the RUN box was underneath, fully covered by the wrapper's white fill.

Applies whenever you programmatically build .excalidraw JSON and append a container after its contents (common in a Python builder). See [[Excalidraw diagram editing technique]] and the caveman/diagram convention in test-agent-v2/docs/reports/.

## Related

- [[Excalidraw diagram editing technique]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]
- [[Excalidraw container-bound text does not auto-wrap in the render script]]
- [[Editing an Excalidraw .excalidraw JSON programmatically]]
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]
- [[Excalidraw arrow x is the first point, not the bounding-box corner]]

%% ai-graph-end %%