---
ai_hash: 40bc51f5dc63381e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities: []
source: session 2026-08-25
status: seedling
tags:
- excalidraw
- diagrams
- json
- gotcha
title: 'Excalidraw JSON generator ghost-text: filtering a node''s rectangle by id
  leaves its text elements behind'
type: lesson
---

# Excalidraw JSON generator ghost-text: filtering a node's rectangle by id leaves its text elements behind

When a script builds Excalidraw `.excalidraw` JSON, a single logical "node" is usually **multiple elements**: a `rectangle` element **plus one or more separate `text` elements** (title, description) that are positioned over it (or bound via `containerId`).

**The trap:** deleting/repositioning a node by filtering the elements array on the rectangle's id —
```js
elements = elements.filter(e => e.id !== "s3_ag");
```
— removes only the rectangle. The associated text elements have their own `txt...` ids and **survive**, rendering as faint "ghost" text stuck at the old coordinates.

**Fix:** place each node exactly once at its final coordinates. If you must remove a node after the fact, also remove its text elements (track their ids, or give the text a `containerId`/`groupId` you can filter on). Simplest is to never create it in the wrong place to begin with.

## Related

- [[claude.ai share links can be org-restricted and require login]]

%% ai-graph-start %%

**Related notes:**
- [[Editing an Excalidraw .excalidraw JSON programmatically]]
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]
- [[Excalidraw offline renderer does not auto-wrap bound container text]]
- [[Excalidraw text does not auto-wrap or auto-center]]

%% ai-graph-end %%