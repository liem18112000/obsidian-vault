---
ai_hash: abbdccf21b639bf1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26
status: seedling
tags:
- excalidraw
- rendering
- cli
- gotcha
- claude-code
title: render_excalidraw.py output path needs -o flag, not positional arg
type: gotcha
---

# render_excalidraw.py output path needs -o flag, not positional arg

The excalidraw-to-PNG renderer at `~/.claude/skills/excalidraw-diagram/references/render_excalidraw.py` takes the **output path via the `-o`/`--output` flag**, never as a second positional argument. Every positional arg is parsed as an *input* `.excalidraw` file (the script uses `nargs="+"` for inputs).

**The gotcha:** if you pass the output PNG positionally (`render_excalidraw.py in.excalidraw out.png`), the script treats `out.png` as another input and tries to `read_text(encoding="utf-8")` an existing PNG, failing with:

`UnicodeDecodeError: 'utf-8' codec can't decode byte 0x89 in position 0`

`0x89` is the first byte of the PNG magic number — a reliable tell that a binary image is being read as text.

**Correct invocation** (run from the renderer's uv dir):

```bash
uv run --quiet python render_excalidraw.py input.excalidraw -o output.png -s 2 -w 2400
```

`-s` = device scale (default 2), `-w` = max viewport width (default 1920). Format is inferred from the `-o` extension (or `-f png|svg|both`).

See [[Excalidraw renderer runs from its uv venv references dir]] for the venv/chromium setup.

## Related

- [[Excalidraw renderer runs from its uv venv references dir]]

%% ai-graph-start %%

**Related notes:**
- [[excalidraw-diagram renderer must run under uv run, not plain python]]
- [[Render .excalidraw to PNGSVG offline with render_excalidraw.py]]
- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]
- [[Excalidraw file stores clean UTF-8 glyphs; console mojibake is a false alarm]]
- [[Embed Excalidraw in repo markdown render to SVG; the renderer does not auto-wrap bound text]]

%% ai-graph-end %%