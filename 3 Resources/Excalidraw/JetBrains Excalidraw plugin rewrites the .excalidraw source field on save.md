---
ai_hash: 4e468258b6fd4667
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28
status: seedling
tags:
- excalidraw
- tooling
- gotcha
- diagrams
- jetbrains
title: JetBrains Excalidraw plugin rewrites the .excalidraw source field on save
type: lesson
---

# JetBrains Excalidraw plugin rewrites the .excalidraw source field on save

When you save a `.excalidraw` file from the **JetBrains Excalidraw plugin**, it silently reformats the JSON on write — notably rewriting the top-level `"source"` field (e.g. from `https://excalidraw.com` to `https://excalidraw-jetbrains-plugin`) and re-indenting the whole file. This shows up as an unexpected "file was modified by a linter" diff right after you write it programmatically. It is harmless: the element content and the render are unaffected, so do not fight it or revert it.

**Rendering `.excalidraw` to an image:** use the `excalidraw-diagram` skill's renderer:
```
cd ~/.claude/skills/excalidraw-diagram/references
uv run python render_excalidraw.py path/to/file.excalidraw            # -> PNG next to the file
uv run python render_excalidraw.py file.excalidraw --format svg       # sharper, ~10x smaller
```
Then Read the produced PNG to visually validate. The script keeps a persistent Chromium profile under `references/.browser-cache/` so repeat renders are faster; pass multiple files in one invocation to skip per-file cold-launch.

**Gotcha:** standalone Excalidraw text does NOT auto-wrap to its element width and is not reliably vertically centered — pre-wrap long strings with explicit `\n` and size boxes to the wrapped line count, or text spills past the box edge.

## Related

- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

%% ai-graph-start %%

**Related notes:**
- [[Render .excalidraw to PNGSVG offline with render_excalidraw.py]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]
- [[Render .excalidraw to PNG headlessly with excalidraw-brute-export-cli]]
- [[excalidraw-diagram renderer must run under uv run, not plain python]]
- [[Excalidraw offline renderer does not auto-wrap bound container text]]

%% ai-graph-end %%