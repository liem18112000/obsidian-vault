---
ai_hash: a2c2e887cdb25e21
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-18
entities: []
source: session 2026-09-18 full-flow.excalidraw edit
status: seedling
tags:
- excalidraw
- encoding
- utf-8
- gotcha
- diagrams
title: Excalidraw file stores clean UTF-8 glyphs; console mojibake is a false alarm
type: lesson
---

# Excalidraw file stores clean UTF-8 glyphs; console mojibake is a false alarm

An `.excalidraw` file (e.g. `test-agent-v2/docs/full-flow.excalidraw`) stores text as **clean Unicode codepoints** — `·` is U+00B7, `▸` is U+25B8, `→` is U+2192. If a quick `PYTHONIOENCODING=utf-8 python -c "print(...)"` shows them as `Â·` / `â€¢` / `â†’`, that is the **console codepage misrendering the output**, NOT mojibake in the file.

**The trap:** seeing `Â·` in terminal echo, concluding the file is double-encoded, and "matching" it by writing `s.encode("utf-8").decode("cp1252")` — which would actually corrupt the glyphs.

**Verify the real bytes before "fixing":**
```python
import json
d = json.load(open("full-flow.excalidraw", encoding="utf-8"))
t = next(e for e in d["elements"] if e["id"]=="tev_call_h")["text"]
print(repr(t), [hex(ord(c)) for c in t if ord(c)>127])
# -> clean "·" == 0xb7, not "Â·"
```
So: **write plain Unicode directly** and `json.dump(..., ensure_ascii=False, encoding="utf-8")`. The excalidraw renderer reads UTF-8 fine.

Second gotcha from the same edit: this file mixes **custom short ids** (`tev_call`, `ttm_box`) with long auto-generated ids (`oj_3vkQD-ddjzWHI_NicI`). A sentinel check that truncates ids (`id[:16]`) will falsely fail — compare full ids.

## Related

- [[Excalidraw diagram editing technique]]
- [[Excalidraw builder for new diagrams]]

%% ai-graph-start %%

**Related notes:**
- [[Python emoji in Excalidraw JSON use single code point, not surrogate pair]]
- [[render_excalidraw.py output path needs -o flag, not positional arg]]
- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]
- [[Windows Python stdout is cp1252 and crashes on emoji; reconfigure to UTF-8]]
- [[Excalidraw fontFamily codes + .excalidraw.png can drift out of sync]]

%% ai-graph-end %%