---
title: "excalidraw-diagram renderer must run under uv run, not plain python"
created: 2026-09-18
type: lesson
status: seedling
source: "session 2026-09-18"
tags: [excalidraw, rendering, playwright, uv, gotcha]
---

# excalidraw-diagram renderer must run under uv run, not plain python

The `excalidraw-diagram` skill renderer (`references/render_excalidraw.py`) must be run through **`uv run python render_excalidraw.py <file> -o <out.png>`**, not plain `python`. Plain `python` fails with `ERROR: playwright not installed` because Playwright/Chromium live in the skill's own uv-managed `.venv` (set up via `uv sync && uv run playwright install chromium`), separate from any project venv.

Also set `PYTHONIOENCODING=utf-8` when the builder script prints Unicode (→, ◈, etc.) or Windows stdout crashes. Writing Unicode into the `.excalidraw` JSON itself is safe (json.dump escapes it); the crash is only on printing.

Batch-render is supported: pass multiple `.excalidraw` inputs. The docs-folder convention is a static `.excalidraw` + a rendered `.png` side by side.
