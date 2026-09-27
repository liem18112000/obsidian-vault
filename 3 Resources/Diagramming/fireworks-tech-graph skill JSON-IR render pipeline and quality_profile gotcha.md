---
title: "fireworks-tech-graph skill: JSON-IR render pipeline and quality_profile gotcha"
created: 2026-09-09
type: howto
status: seedling
source: "session 2026-09-09 SCRUM-92"
tags: [diagrams, svg, skill, claude-code]
---

# fireworks-tech-graph skill: JSON-IR render pipeline and quality_profile gotcha

fireworks-tech-graph is a portable Agent Skill (installs to `~/.claude/skills/`) that renders technical architecture/UML/agent diagrams to SVG locally — no API key. Pipeline per diagram:
`python3 scripts/fireworks.py validate <mode> in.json` → `render <mode> in.json out.svg --report r.json` → `check out.svg`.

Input is a JSON IR: `containers` (layer bands), `nodes` (kind = rect | double_rect | cylinder; x/y/width/height, label, sublabel, fill, stroke, flat), `arrows` (source/target + source_port/target_port = left|right|top|bottom, flow = read|control, label). Node fill/stroke are explicit hex, so you can recolor to any palette regardless of the chosen `style` (1–12).

**quality_profile gotcha:** `showcase` enforces tight composition budgets (max 2 bends/edge, 8 total, route-stretch 1.35, 20px container gutters) that hand-placed diagonal edges routinely fail with "no collision-free label position" / "COMPOSITION_QUALITY". Set `quality_profile: "standard"` (12 bends/edge, no gutter minimum) for hand-authored layouts. Also: keep arrow segments long enough for their label, and keep the footer within `height` or the geometry check fails with `canvas_clip: g#footer exceeds viewBox`.

PNG/GIF export needs CairoSVG/rsvg-convert (+ Node for GIF); SVG render needs only Python. From: https://github.com/yizhiyanhua-ai/fireworks-tech-graph

## Related

- [[Embed generated SVG in an artifact via <img> data-URI to isolate its styles]]
