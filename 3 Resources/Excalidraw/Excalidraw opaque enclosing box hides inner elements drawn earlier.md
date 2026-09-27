---
title: "Excalidraw opaque enclosing box hides inner elements drawn earlier"
created: 2026-09-23
type: lesson
status: seedling
source: "session 2026-09-23 exec-overview diagram"
tags: [excalidraw, diagrams, gotcha]
---

# Excalidraw opaque enclosing box hides inner elements drawn earlier

An Excalidraw `rectangle` used as a grouping / sandbox / trust-boundary wrapper around inner boxes will **paint over and hide those inner boxes** if it has a solid `backgroundColor` (e.g. `#ffffff`) AND appears later in the `elements` array — Excalidraw paints in array order, no z-index. The fix: give any enclosing/annotation box `backgroundColor: "transparent"` (keep the dashed stroke for the visible boundary), or draw it before the elements it surrounds.

Symptom seen once: a dashed red 'sandbox' box drawn after the RUN step rendered as an empty dashed box — the RUN box was underneath, fully covered by the wrapper's white fill.

Applies whenever you programmatically build .excalidraw JSON and append a container after its contents (common in a Python builder). See [[Excalidraw diagram editing technique]] and the caveman/diagram convention in test-agent-v2/docs/reports/.

## Related

- [[Excalidraw diagram editing technique]]
