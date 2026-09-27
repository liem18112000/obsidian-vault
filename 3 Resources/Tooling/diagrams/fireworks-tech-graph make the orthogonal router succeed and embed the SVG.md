---
ai_hash: edbc157b4b3a763e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: PROD investigation 2026-09-09
status: seedling
tags:
- fireworks-tech-graph
- svg
- diagrams
- skill
- gotcha
title: 'fireworks-tech-graph: make the orthogonal router succeed and embed the SVG'
type: howto
---

# fireworks-tech-graph: make the orthogonal router succeed and embed the SVG

Practical gotchas for the fireworks-tech-graph skill (SVG tech diagrams from JSON via `fireworks.py render architecture in.json out.svg`).

**"no collision-free orthogonal route satisfies the current constraints"** — the strict router rejects the layout. Causes + fixes, in order of impact:
- Container band **titles** sit in an arrow that crosses that band -> remove the `containers` array (they are decorative) or their labels. This was the biggest one.
- **Long arrow labels** reserve big obstacle boxes -> shorten `label` to a few words; put detail in the surrounding doc, not the arrow.
- Forced `source_port`/`target_port` over-constrain -> omit ports and let the router choose; only add ports (or `corridor_x`/`corridor_y`, or explicit `route_points`) when a specific edge is wrong.
- Keep clean lanes: a node directly under another node blocks the upper nodes vertical drop to a third node; route the crossing edge out a side port through an empty gap.

**"COMPOSITION_QUALITY: EDGE_MICRO_SEGMENT / EDGE_BEND_BUDGET / EDGE_ROUTE_STRETCH"** — layout routed but failed the strict gate that `quality_profile:"showcase"` turns on. Remove `quality_profile` (default profile) to relax it; `check` still enforces collisions/composition/geometry/markers/xml.

**Embedding the SVG in an HTML artifact:** Style 3 Blueprint emits a self-contained dark-navy SVG (bg #082f49, cyan strokes), no external refs -> CSP-safe to inline. Strip the root `width=`/`height=` attrs and set CSS `svg{width:100%;height:auto}` for responsive scaling; it is a fixed-palette panel (not theme-aware), so it looks the same in light+dark.

**No eyeball:** `export-png` needs CairoSVG or rsvg-convert installed; without them you cannot rasterize to inspect. Rely on `fireworks.py check` (validates identity, markers, collisions, composition, geometry) + the render report`s `typography.complete_text`, and explicitly mark the visual check as skipped.

## Related
[[Deep-link a Google Cloud Logging query into the console via URL]]

## Related

- [[Deep-link a Google Cloud Logging query into the console via URL]]

## Style 12 Ops Pulse — extra contract rules (learned)

Ops Pulse (`style:12`, `semantic_profile:"ops-pulse"`, `diagram_type:"observability"`) is a strict semantic view. Beyond the generic gotchas above:

- **Every signal's `window` must EXACTLY equal the top-level `observation_window`** (it is a *fixed* window). Different per-signal windows fail validation — put peak-time nuance in the value/subtitle, not the window.
- **Each `ops_role:"service"` needs exactly latency+traffic+errors+saturation, and every signal needs a non-empty `unit`** — `unit:""` fails (`SEMANTIC_REQUIRED`). Use `"-"` for a signal that has no natural unit. Values may be non-numeric strings (`"OOM"`, `"n/a"`, `">=90"`).
- **A trace waterfall is MANDATORY** (`OPS_TRACE_REQUIRED`) — you cannot make a service-health-only Ops Pulse. Spans need positive duration, one root, and valid parent coverage (child start≥parent start, child end≤parent end). So two Ops Pulse diagrams of the same request duplicate the band+trace — prefer one.
- `critical_path` is an ordered list of **business edge ids** (contiguous, no telemetry edges); telemetry edges use a different `flow` token (e.g. `async`) and `dashed:true`.
- `legend_locked:true` can throw "locked legend intersects diagram content or a mandatory route" — unlock it or give it its own vertical band (raise canvas height, move legend/footer down).
- It fits incident reports well because the four golden signals map to real evidence: latency=app request time, traffic=req/min, errors=timeout count, saturation=OOM.

%% ai-graph-start %%

**Related notes:**
- [[fireworks-tech-graph skill JSON-IR render pipeline and quality_profile gotcha]]
- [[Animate fireworks SVGs via injected CSS keyframes; Style-12 Ops Pulse constraints]]
- [[Embed a fireworks-tech-graph SVG in an HTML artifact and animate it with CSS via its data-flowid hooks]]
- [[Embed generated SVG in an artifact via img data-URI to isolate its styles]]

%% ai-graph-end %%