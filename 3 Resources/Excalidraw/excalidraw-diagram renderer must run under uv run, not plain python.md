---
ai_hash: 2587c0c3e4456ef0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: session 2026-09-18
status: seedling
tags:
- excalidraw
- rendering
- playwright
- uv
- gotcha
title: excalidraw-diagram renderer must run under uv run, not plain python
type: lesson
---

# excalidraw-diagram renderer must run under uv run, not plain python

The `excalidraw-diagram` skill renderer (`references/render_excalidraw.py`) must be run through **`uv run python render_excalidraw.py <file> -o <out.png>`**, not plain `python`. Plain `python` fails with `ERROR: playwright not installed` because Playwright/Chromium live in the skill's own uv-managed `.venv` (set up via `uv sync && uv run playwright install chromium`), separate from any project venv.

Also set `PYTHONIOENCODING=utf-8` when the builder script prints Unicode (→, ◈, etc.) or Windows stdout crashes. Writing Unicode into the `.excalidraw` JSON itself is safe (json.dump escapes it); the crash is only on printing.

Batch-render is supported: pass multiple `.excalidraw` inputs. The docs-folder convention is a static `.excalidraw` + a rendered `.png` side by side.

%% ai-graph-start %%

**Related notes:**
- [[Render .excalidraw to PNGSVG offline with render_excalidraw.py]]
- [[render_excalidraw.py output path needs -o flag, not positional arg]]
- [[Render .excalidraw to PNG headlessly with excalidraw-brute-export-cli]]
- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]

%% ai-graph-end %%