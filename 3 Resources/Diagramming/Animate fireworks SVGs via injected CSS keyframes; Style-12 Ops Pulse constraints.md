---
title: "Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints"
created: 2026-09-09
type: lesson
status: seedling
source: "session 2026-09-09 SCRUM-92"
tags: [diagrams, svg, animation, fireworks, claude-code, gotcha]
---

# Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints

Making a fireworks-tech-graph diagram "animate like the skills GIF" does NOT require the GIF route when the target is a web page. The `animate` command needs ffmpeg + puppeteer-core (+ a Chromium download) — often absent. Instead, render the static SVG, then inject a `<style>` with `@keyframes` right after the opening `<svg>` tag and target the stable data-attributes the renderer emits: `path[data-graph-role="edge"]` (animate `stroke-dashoffset` with a dasharray for a flowing pulse) and `path[data-graph-role="decoration"][id$="-critical-glow"]` (opacity pulse). Embed the result as an `<img>` data-URI so the SVG-internal CSS is isolated AND still plays (img-embedded SVGs run their own declarative CSS/SMIL). Always add a `@media (prefers-reduced-motion:reduce)` reset.

Style 12 (Ops Pulse) authoring constraints learned the hard way (semantic_profile:"ops-pulse", diagram_type:"observability", mode stays "architecture"): ops_service cards must be >=180x108; each `trace_span` child must be fully time-AND-x contained by its `parent_span` (reparent to the root span if it starts where the parent ends); and a `legend_locked:true` legend easily throws "locked legend intersects diagram content" — set it false to auto-place. Use quality_profile "standard" not "showcase" for hand-placed layouts. From SCRUM-92 (2026-09-09).

## Related

- [[fireworks-tech-graph skill: JSON-IR render pipeline and quality_profile gotcha]]
- [[Embed generated SVG in an artifact via <img> data-URI to isolate its styles]]
