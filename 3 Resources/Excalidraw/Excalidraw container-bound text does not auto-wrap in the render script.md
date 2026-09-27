---
ai_hash: bad8555bdd370eaa
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17 agent-framework-skeleton diagram
status: seedling
tags:
- excalidraw
- diagrams
- gotcha
- claude-skills
title: Excalidraw container-bound text does not auto-wrap in the render script
type: lesson
---

# Excalidraw container-bound text does not auto-wrap in the render script

Excalidraw text elements bound to a container (via `containerId` + the container's `boundElements`) are **not** re-wrapped to the container width by the excalidraw-diagram skill's headless render script. Whatever single line you put in `text`/`originalText` is drawn at its full width and simply overflows past the box edge if it is too long.

**Why it bites:** it looks like normal Excalidraw (where bound text wraps live in the editor), so you assume wrapping and ship a label that clips. Saw it with `AGENT\n(orchestration core)` in a 220px box — "core)" poked out the right side.

**Fix / rule of thumb:**
- Keep in-box labels short, or insert `\n` yourself to hand-wrap.
- Budget ~`floor(innerWidthPx / (fontSize * 0.6))` chars per line for fontFamily 3 (monospace). A 220px box (~188px inner) at fontSize 20 holds ~15 chars/line.
- When unsure, make the box taller and split across lines rather than shrinking the font.

Same failure mode the skill warns about for standalone text — it just also applies to bound text under this renderer.

## Related

- [[excalidraw-diagram skill]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw standalone text does not auto-wrap to element width]]
- [[Excalidraw offline renderer does not auto-wrap bound container text]]
- [[Excalidraw text does not auto-wrap or auto-center]]
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]

%% ai-graph-end %%