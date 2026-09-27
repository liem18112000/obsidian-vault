---
title: "Embed Excalidraw in repo markdown: render to SVG; the renderer does not auto-wrap bound text"
created: 2026-09-23
type: howto
status: seedling
source: "session 2026-09-23"
tags: [excalidraw, svg, markdown, diagramming, gotcha]
---

# Embed Excalidraw in repo markdown: render to SVG; the renderer does not auto-wrap bound text

To put an Excalidraw diagram into a repo markdown doc (GitHub / GitBook), export it to **SVG** and embed with a normal image tag — SVG is portable, sharp at any zoom, ~10x smaller than PNG, and renders as an image everywhere.

Workflow with the `excalidraw-diagram` skill:
1. Hand-author the `.excalidraw` JSON (keep it as the editable source next to the doc, e.g. `docs/.../assets/`).
2. Render + view (mandatory loop): `cd ~/.claude/skills/excalidraw-diagram/references && uv run python render_excalidraw.py file.excalidraw` → Read the PNG → fix.
3. Export: add `--format svg` (or `both`). Multiple files in one invocation share the chromium launch.
4. Embed: `![alt](assets/name.svg)`.

**Gotcha discovered:** this renderer does NOT auto-wrap or auto-reposition *bound* text (text with `containerId`). Long strings overflow the box horizontally, and a wrong `y` on the text element is honored literally (text lands at that y, not centered in its container). So for every boxed label: set the text `y` to the box center yourself, size `width` to the box, and pre-wrap long strings with explicit `\n`. Do not rely on containerId to wrap/center — verify in the render.

Mermaid, by contrast, renders inline from ```mermaid fences (no export needed). Validate mermaid separately with `mermaid.parse()` under jsdom — see [[Validate mermaid diagrams headlessly with mermaid.parse under jsdom]].
