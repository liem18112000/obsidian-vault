---
ai_hash: 188bfb6d26b31a66
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-16
entities: []
source: session 2026-08-16 claude-watermark docs
status: seedling
tags:
- excalidraw
- diagrams
- gotcha
- rendering
title: Excalidraw offline renderer does not auto-wrap bound container text
type: lesson
---

# Excalidraw offline renderer does not auto-wrap bound container text

When generating `.excalidraw` JSON for **offline** rendering via the `excalidraw-diagram` skill's `render_excalidraw.py` (headless Chromium + esm.sh module), **bound container text does NOT auto-wrap**. A text element with `containerId` pointing to a rectangle will, if a single line is longer than the box, overflow straight past the box edge instead of wrapping — unlike interactive Excalidraw on the web, which re-wraps bound text to the container width.

**Why it matters:** it's silent in the JSON and only visible in the rendered PNG/SVG. It's the most common cause of text spilling out of a box in these diagrams.

**Fix:** pre-wrap the text yourself by inserting `\n` line breaks sized to the box width. Safe line length ≈ `floor(boxWidthPx / (fontSize × 0.6))` characters for `fontFamily: 3` (the code font). Then make the box tall enough for N lines: height ≥ `N × fontSize × lineHeight + 2×padding`.

**What DOES work:** multi-line strings that already contain explicit `\n` render fine and center vertically/horizontally as expected — only the *auto-wrap of a long single line* is missing. So the discipline is simply: never rely on wrapping; always hand-wrap long strings.

Discovered building the Claude text-watermarking diagrams (export at `C:\Users\dvtliem\.claude\docs\claude-watermark`): a one-line `CLAIM SIGNATURE …` label overflowed its box until split with `\n`.

## Related
[[Excalidraw diagram skill]]

## Related

- [[Excalidraw diagram skill]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw container-bound text does not auto-wrap in the render script]]
- [[Excalidraw text does not auto-wrap or auto-center]]
- [[Excalidraw standalone text does not auto-wrap to element width]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]

%% ai-graph-end %%