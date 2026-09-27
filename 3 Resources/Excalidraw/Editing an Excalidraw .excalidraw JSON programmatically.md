---
ai_hash: 7d972408c45cd87b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26 (leo-customer360 deploy diagram)
status: seedling
tags:
- excalidraw
- json
- diagram
- gotcha
title: Editing an Excalidraw .excalidraw JSON programmatically
type: howto
---

# Editing an Excalidraw .excalidraw JSON programmatically

To extend an existing Excalidraw `.excalidraw` file (JSON) in code without breaking it:

- **Deep-clone an existing element of the same type** and mutate id/x/y/text — this inherits the full field schema (seed, versionNonce, roundness, fillStyle, fontFamily, lineHeight, ...) which Excalidraw needs; hand-writing a partial element often fails to load. Give each new element a fresh `seed`/`versionNonce`.
- **A text label inside a box needs the link declared on BOTH ends:** the text sets `containerId=<rectId>`, AND the rect's `boundElements` must include `{type:"text", id:<textId>}`. Set only one and the label floats / isn't contained.
- **Arrows bind the same way, reciprocally:** the arrow sets `startBinding/endBinding = {elementId, focus, gap}`, and BOTH endpoint elements list `{type:"arrow", id:<arrowId>}` in their `boundElements`. Arrow geometry is `x,y` at the start point + `points=[[0,0],[dx,dy]]`; with bindings set, Excalidraw re-routes when a box is moved.
- Validate before saving: JSON parses, no duplicate ids, every binding/containerId resolves to an existing id.

**Rendering gotcha:** a README usually embeds the exported `.png`/`.svg`, not the `.excalidraw`. You can edit the JSON source in code, but you CANNOT regenerate the PNG/SVG without the Excalidraw app (roughjs hand-drawn rendering uses random seeds) — the user must re-export. Surfaced adding a service box to a VNG deployment diagram.

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]
- [[Editing Excalidraw diagrams with committed SVG exports needs both files updated]]
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]
- [[Excalidraw fontFamily codes + .excalidraw.png can drift out of sync]]
- [[Convert Excalidraw to draw.io by reading exported PNGs instead of the JSON]]

%% ai-graph-end %%