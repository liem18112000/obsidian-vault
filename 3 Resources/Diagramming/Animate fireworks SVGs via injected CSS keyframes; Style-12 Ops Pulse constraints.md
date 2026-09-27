---
ai_hash: 37d08bef0a771631
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: session 2026-09-09 SCRUM-92
status: seedling
tags:
- diagrams
- svg
- animation
- fireworks
- claude-code
- gotcha
title: Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints
type: lesson
---

# Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints

Making a fireworks-tech-graph diagram "animate like the skills GIF" does NOT require the GIF route when the target is a web page. The `animate` command needs ffmpeg + puppeteer-core (+ a Chromium download) — often absent. Instead, render the static SVG, then inject a `<style>` with `@keyframes` right after the opening `<svg>` tag and target the stable data-attributes the renderer emits: `path[data-graph-role="edge"]` (animate `stroke-dashoffset` with a dasharray for a flowing pulse) and `path[data-graph-role="decoration"][id$="-critical-glow"]` (opacity pulse). Embed the result as an `<img>` data-URI so the SVG-internal CSS is isolated AND still plays (img-embedded SVGs run their own declarative CSS/SMIL). Always add a `@media (prefers-reduced-motion:reduce)` reset.

Style 12 (Ops Pulse) authoring constraints learned the hard way (semantic_profile:"ops-pulse", diagram_type:"observability", mode stays "architecture"): ops_service cards must be >=180x108; each `trace_span` child must be fully time-AND-x contained by its `parent_span` (reparent to the root span if it starts where the parent ends); and a `legend_locked:true` legend easily throws "locked legend intersects diagram content" — set it false to auto-place. Use quality_profile "standard" not "showcase" for hand-placed layouts. From SCRUM-92 (2026-09-09).

## Related

- [[fireworks-tech-graph skill JSON-IR render pipeline and quality_profile gotcha|fireworks-tech-graph skill: JSON-IR render pipeline and quality_profile gotcha]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles|Embed generated SVG in an artifact via <img> data-URI to isolate its styles]]

%% ai-graph-start %%

**Related notes:**
- [[Embed a fireworks-tech-graph SVG in an HTML artifact and animate it with CSS via its data-flowid hooks]]
- [[fireworks-tech-graph skill JSON-IR render pipeline and quality_profile gotcha]]
- [[fireworks-tech-graph make the orthogonal router succeed and embed the SVG]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]
- [[Re-exporting the deployment-view PNG from its SVG with resvg-js]]

%% ai-graph-end %%