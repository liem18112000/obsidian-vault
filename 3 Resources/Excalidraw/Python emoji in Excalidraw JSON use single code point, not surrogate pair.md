---
ai_hash: d5f9465ecabf1ef9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20
status: seedling
tags:
- excalidraw
- python
- json
- gotcha
- diagrams
title: 'Python emoji in Excalidraw JSON: use single code point, not surrogate pair'
type: lesson
---

# Python emoji in Excalidraw JSON: use single code point, not surrogate pair

When generating Excalidraw `.excalidraw` JSON from Python, do **not** write an emoji as a surrogate-pair escape like `"\ud83d\ude80"` in a string literal. Python treats those as two lone surrogates, and `json.dump(..., ensure_ascii=False)` then raises `UnicodeEncodeError: surrogates not allowed`. Use the **single code point** instead: `"\U0001F680"` (or paste the literal 🚀). This bit me while embedding a JSON sample inside a diagram.

Two related Excalidraw rendering rules (same session):
- **Standalone text does not auto-wrap** to its element `width` — long strings overflow the canvas/box. Pre-insert `\n` yourself and size the box to the wrapped line count.
- **Arrowheads render *inside* the target box** if the arrow's last point sits exactly on the box edge. Land the last point ~8px short of the edge (or set `endBinding.gap`) so the triangle shows in the gap.

## Related

- [[blockbuzz architecture a Nostr-relay hive mind for humans and AI agents|block/buzz architecture: a Nostr-relay hive mind for humans and AI agents]]

%% ai-graph-start %%

**Related notes:**
- [[Excalidraw file stores clean UTF-8 glyphs; console mojibake is a false alarm]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]
- [[Editing an Excalidraw .excalidraw JSON programmatically]]
- [[Excalidraw offline renderer does not auto-wrap bound container text]]
- [[render_excalidraw.py output path needs -o flag, not positional arg]]

%% ai-graph-end %%