---
ai_hash: 7652cef450bc5948
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-07
entities: []
source: session 2026-09-07 — CQRS split diagram
status: seedling
tags:
- excalidraw
- diagramming
- gotcha
- arrow-routing
title: Excalidraw connector on a shape's center axis bisects a centered caption
type: lesson
---

# Excalidraw connector on a shape's center axis bisects a centered caption

A connector arrow routed straight along a shape's center axis will visually cut through any caption you center on that same axis — the arrowhead/shaft crosses the text at its middle. Excalidraw draws exactly the polyline you specify and free-floating text has **no collision outline**, so this is silent in the JSON and only shows up in the render.

**Why it happens:** a label centered under (or over) a box shares the box's center x. A connector dropping from that box's bottom-center (or rising to its top-center) runs down that exact x, so it bisects the centered label.

**Fixes (any one):**
- **Offset the caption off the arrow lane** — right-align it so its right edge stops a bit *before* the arrow's x, or left-align it to start *after* the arrow's x. The label then sits beside the connector, not on it.
- **Shorten the arrow** so it starts/ends in the clear band and the caption occupies the vacated space.
- Move the connector itself off-center.

**Discovered:** building the CQRS "system of record vs system of recall" diagram for the two-tier agent memory proposal (`test-agent/docs/cqrs-split-record-vs-recall.excalidraw`). The WRITE↓ and READ↑ connector arrows ran through the small `the C in CQRS — command` / `the Q in CQRS — query` captions right at the word "CQRS"; right-/left-aligning each caption off the arrow's x-line fixed it.

This is a concrete instance of the excalidraw-diagram skill's **Arrow Routing Discipline §3**: treat every free-floating text element as an obstacle for arrow segments — a tier/lane gap holds *either* arrows *or* text, never both on the same line.

## Related

- [[Arrow Routing Discipline]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw container-bound text does not auto-wrap in the render script]]
- [[Excalidraw standalone text does not auto-wrap to element width]]
- [[Excalidraw text does not auto-wrap or auto-center]]
- [[Excalidraw arrow x is the first point, not the bounding-box corner]]
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]

%% ai-graph-end %%