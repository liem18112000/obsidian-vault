---
ai_hash: e353d19afd403f56
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: session 2026-09-05
status: seedling
tags:
- excalidraw
- diagrams
- gotcha
- text-layout
title: Excalidraw standalone text does not auto-wrap to element width
type: lesson
---

# Excalidraw standalone text does not auto-wrap to element width

Excalidraw **free-floating / standalone** text elements do **not** auto-wrap to their `width` property. A single unbroken string renders on one long line no matter what `width` you set, so it can silently overrun its box or collide with nearby arrows/elements.

**Fix:**
- Insert manual `\n` line breaks in `text` (and `originalText`) yourself — safe line length in monospace `fontFamily:3` is roughly `floor(boxWidthPx / (fontSize * 0.6))` chars.
- Size the box to the wrapped line count: height >= `lines * fontSize * lineHeight + 2*padding`.
- Center **deterministically** — the renderer also ignores `verticalAlign:middle` on standalone text; compute `y = boxY + (boxH - textH)/2` and use `textAlign:center` for the horizontal axis.

**Concrete instance:** an ~80-char loop caption in a one-pager rendered ~528px wide on a single line and left only ~6px clearance from two vertical loop arrows. Splitting it into two `\n`-separated lines (~304px wide) restored clean spacing.

**Takeaway:** always pre-wrap long strings and re-render to verify — never trust `width` to wrap for you.

## Related
[[Excalidraw diagram creation workflow]]

## Related

- [[Excalidraw diagram creation workflow]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw container-bound text does not auto-wrap in the render script]]
- [[Excalidraw text does not auto-wrap or auto-center]]
- [[Excalidraw offline renderer does not auto-wrap bound container text]]
- [[Excalidraw vertically-centers bound text — use unbound top-left text for container headers]]
- [[Excalidraw render script does not auto-position containerId-bound text — set explicit x,y]]

%% ai-graph-end %%