---
ai_hash: e7c65224f96c1a88
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: session 2026-09-09
status: seedling
tags:
- fireworks-tech-graph
- svg
- css-animation
- artifacts
- diagrams
title: Embed a fireworks-tech-graph SVG in an HTML artifact and animate it with CSS
  via its data-flow/id hooks
type: howto
---

# Embed a fireworks-tech-graph SVG in an HTML artifact and animate it with CSS via its data-flow/id hooks

The fireworks-tech-graph skill renders a semantic SVG whose edges carry stable hooks: `id="<edge-id>"`, `data-graph-role="edge"`, `data-flow="control|feedback"`, and `data-node-id` on nodes. Inline that SVG into an HTML artifact and animate it with plain CSS targeting those hooks — no library, self-contained, theme-safe:

```css
.fw-figure svg [data-graph-role="edge"]{stroke-dasharray:9 7}
@keyframes fw-march{to{stroke-dashoffset:-16}}
.fw-figure svg [data-flow="control"]{animation:fw-march 1s linear infinite}
.fw-figure svg [data-flow="feedback"]{animation:fw-march .8s linear infinite}
.fw-figure svg [data-node-id="leaked"]{animation:fw-pulse 1.5s ease-in-out infinite}
@media (prefers-reduced-motion:reduce){.fw-figure svg *{animation:none!important}}
```

Marching dashes (negative `stroke-dashoffset`) always move toward each edges target because they follow the paths own direction. CSS `stroke-dasharray` overrides the SVGs inline presentation attribute, so control (solid) and feedback (dashed) edges unify.

**Gotchas hit during render:** (1) `legend_locked:true` fails with "locked legend intersects content" — set it false to auto-place. (2) Composition gate `EDGE_MICRO_SEGMENT` / inflated `EDGE_BEND_BUDGET` came from `route_points` that were 0.5px off the port center — use integer-centered node geometry so routes are clean 2-bend paths. (3) "no collision-free label position" = nodes too close; widen canvas / gaps (~110px) for arrow labels. (4) GIF `animate` needs Puppeteer + ffmpeg (doctor shows gif:false without them) — CSS animation of the inline SVG is the offline alternative.

Wrap the light-styled SVG in a fixed white card so it reads in both artifact themes.

%% ai-graph-start %%

**Related notes:**
- [[Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints]]
- [[fireworks-tech-graph skill JSON-IR render pipeline and quality_profile gotcha]]
- [[fireworks-tech-graph make the orthogonal router succeed and embed the SVG]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]
- [[Inline SVG ignores theme unless shapes use CSS-variable classes, not hardcoded hex]]

%% ai-graph-end %%