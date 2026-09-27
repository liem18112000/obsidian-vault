---
ai_hash: e67b98e08f3e67d2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: session 2026-08-19
status: seedling
tags:
- excalidraw
- diagrams
- rendering
- tooling
title: Render .excalidraw to PNG/SVG offline with render_excalidraw.py
type: howto
---

# Render .excalidraw to PNG/SVG offline with render_excalidraw.py

The excalidraw-diagram skill ships a local headless-Chromium renderer, so you can turn a `.excalidraw` JSON file into a PNG or SVG entirely offline — no excalidraw.com round-trip. This is what lets a generated diagram be committed into a repo README as a static image.

```bash
cd ~/.claude/skills/excalidraw-diagram/references
uv run python render_excalidraw.py path/to/diagram.excalidraw --format both   # png | svg | both
```

- Output lands next to the input file (`diagram.png` / `diagram.svg`).
- A persistent Chromium profile in `references/.browser-cache/` makes repeat renders fast (the excalidraw ESM module is served from disk after the first run).
- Pass multiple `.excalidraw` files in one invocation to skip per-file Chromium cold-launch.
- First-time setup: `uv sync` then `uv run playwright install chromium`.

Workflow that uses it: generate the `.excalidraw` (by hand or a small deterministic generator), then run the render → Read the PNG → fix the JSON → re-render loop until clean, then embed the PNG in a README and keep the `.excalidraw`/`.svg` as editable sources.

Related: [[excalidraw-diagram skill]]

## Related

- [[excalidraw-diagram skill]]

%% ai-graph-start %%

**Related notes:**
- [[Render .excalidraw to PNG headlessly with excalidraw-brute-export-cli]]
- [[excalidraw-diagram renderer must run under uv run, not plain python]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]
- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]
- [[Render Excalidraw-style hand-drawn PNGs headlessly with rough.js in the Playwright browser]]

%% ai-graph-end %%