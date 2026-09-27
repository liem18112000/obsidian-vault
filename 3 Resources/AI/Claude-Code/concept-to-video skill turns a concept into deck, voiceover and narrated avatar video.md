---
ai_hash: 1ecddfb625aa62b4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-19
entities:
- concept-to-video skill
- concept
- deck
- voiceover
- narrated avatar video
- ~/.claude/skills/concept-to-video/
- resources
- excalidraw diagrams
- pptx deck
- per-slide voiceover
- EN
- Southern-VI
- staged-reveal videos
- narrated HD video
- Google TTS
- audio-reactive anime-mascot presenter overlay
- hook-present
- obsidian-present
- telegram-present
- reference outputs
- ~/.claude/docs/{hook,obsidian,telegram}-present
- SKILL.md
- 8-step workflow
- setup.sh
- node
- pptxgenjs
- python PIL
- fitz
- imageio-ffmpeg
- python-pptx
- LibreOffice
- uv
- excalidraw renderer
- gcloud TTS
- avatar images
- Windows fonts
- templates/
- make-narrated-video.py
- add-avatar.py
- deck.config.json
- make-video.py
- _frames.js
- gen-diagram template
- build-deck template
- references/
- pipeline.md
- diagram-and-deck-rules.md
- voiceover-and-tts.md
- Per-deck flow
- docs/<topic>-present/
- build directory
- diagrams directory
- assets directory
- voiceover-vi/en.md
- '`DECK=<dir> LANG_CODE=VI python make-narrated-video.py`'
- '`add-avatar.py`'
- Make one diagram generator double as a reveal-video frame source with STAGE() markers
- Google Cloud TTS from Windows fetch the token in bash, pass via env to Python
- Audio-reactive anime mascot overlay for narrated videos (ffmpeg)
- STAGE() markers
- Windows
- bash
- Python
- ffmpeg
source: session 2026-06-19
status: seedling
tags:
- skill
- claude-code
- video
- slides
- tts
- reference
title: concept-to-video skill turns a concept into deck, voiceover and narrated avatar
  video
type: reference
---

# concept-to-video skill turns a concept into deck, voiceover and narrated avatar video

Reusable skill at `~/.claude/skills/concept-to-video/` that runs the whole chain from a concept + resources: excalidraw diagrams -> pptx deck -> per-slide voiceover (EN + humorous Southern-VI) -> staged-reveal videos -> narrated HD video (Google TTS) -> audio-reactive anime-mascot presenter overlay. It generalizes the three hand-built decks under `~/.claude/docs/{hook,obsidian,telegram}-present`, which stay as the reference outputs.

Structure:
- `SKILL.md` - the 8-step workflow.
- `setup.sh` - one-time pre-step; checks/fixes node+pptxgenjs, python PIL/fitz/imageio-ffmpeg/python-pptx, LibreOffice, uv + excalidraw renderer, gcloud TTS (with a LIVE synth test), avatar images, and the four Windows fonts.
- `templates/` - config-driven `make-narrated-video.py` + `add-avatar.py` (both driven by a per-deck `deck.config.json`), generic `make-video.py` + `_frames.js`, and copy-and-adapt `gen-diagram` + `build-deck` templates.
- `references/` - `pipeline.md`, `diagram-and-deck-rules.md`, `voiceover-and-tts.md` (the gotchas).

Per-deck flow: run `setup.sh` once; create `docs/<topic>-present/` with `build,diagrams,assets`; adapt the gen + build-deck templates; write `voiceover-vi/en.md` as `[Slide N]` blocks; write `deck.config.json`; then `DECK=<dir> LANG_CODE=VI python make-narrated-video.py` followed by `add-avatar.py`.

## Related

- [[Make one diagram generator double as a reveal-video frame source with STAGE() markers]]
- [[Google Cloud TTS from Windows fetch the token in bash, pass via env to Python]]
- [[Audio-reactive anime mascot overlay for narrated videos (ffmpeg)]]

%% ai-graph-start %%

**Related notes:**
- [[Audio-reactive anime mascot overlay for narrated videos (ffmpeg)]]
- [[Assemble a narrated slide video pptx to png + per-slide Google TTS + ffmpeg -shortest segments + concat]]
- [[Narration-synced highlight region-based dimemphasize excalidraw variants + timed xfade]]
- [[concept-to-video avatar overlay sits bottom-right — keep callouts clear]]
- [[Make an MP4 from staged Excalidraw reveal frames (corner-pin canvas + PIL blend + imageio-ffmpeg)]]

**Relations:**
- concept-to-video skill — *transforms concept into* — deck
- concept-to-video skill — *transforms concept into* — voiceover
- concept-to-video skill — *transforms concept into* — narrated avatar video
- concept-to-video skill — *is located at* — ~/.claude/skills/concept-to-video/
- concept-to-video skill — *runs chain from* — concept
- concept-to-video skill — *runs chain from* — resources
- concept-to-video skill — *produces* — excalidraw diagrams
- concept-to-video skill — *produces* — pptx deck
- concept-to-video skill — *produces* — per-slide voiceover
- concept-to-video skill — *produces* — staged-reveal videos
- concept-to-video skill — *produces* — narrated HD video
- concept-to-video skill — *produces* — audio-reactive anime-mascot presenter overlay
- per-slide voiceover — *supports language* — EN
- per-slide voiceover — *supports language* — Southern-VI
- narrated HD video — *uses* — Google TTS
- concept-to-video skill — *generalizes* — hook-present
- concept-to-video skill — *generalizes* — obsidian-present
- concept-to-video skill — *generalizes* — telegram-present
- hook-present — *is a type of* — reference outputs
- obsidian-present — *is a type of* — reference outputs
- telegram-present — *is a type of* — reference outputs
- reference outputs — *are located at* — ~/.claude/docs/{hook,obsidian,telegram}-present
- concept-to-video skill — *has component* — SKILL.md
- concept-to-video skill — *has component* — setup.sh
- concept-to-video skill — *has component* — templates/
- concept-to-video skill — *has component* — references/
- SKILL.md — *describes* — 8-step workflow
- setup.sh — *manages dependency* — node
- setup.sh — *manages dependency* — pptxgenjs
- setup.sh — *manages dependency* — python PIL
- setup.sh — *manages dependency* — fitz
- setup.sh — *manages dependency* — imageio-ffmpeg
- setup.sh — *manages dependency* — python-pptx
- setup.sh — *manages dependency* — LibreOffice
- setup.sh — *manages dependency* — uv
- setup.sh — *manages dependency* — excalidraw renderer
- setup.sh — *manages dependency* — gcloud TTS
- setup.sh — *manages dependency* — avatar images
- setup.sh — *manages dependency* — Windows fonts
- templates/ — *contains* — make-narrated-video.py
- templates/ — *contains* — add-avatar.py
- templates/ — *contains* — make-video.py
- templates/ — *contains* — _frames.js
- templates/ — *contains* — gen-diagram template
- templates/ — *contains* — build-deck template
- make-narrated-video.py — *is configured by* — deck.config.json
- add-avatar.py — *is configured by* — deck.config.json
- references/ — *contains* — pipeline.md
- references/ — *contains* — diagram-and-deck-rules.md
- references/ — *contains* — voiceover-and-tts.md
- Per-deck flow — *executes* — setup.sh
- Per-deck flow — *creates* — docs/<topic>-present/
- Per-deck flow — *adapts* — gen-diagram template
- Per-deck flow — *adapts* — build-deck template
- Per-deck flow — *writes* — voiceover-vi/en.md
- Per-deck flow — *writes* — deck.config.json
- Per-deck flow — *runs command* — `DECK=<dir> LANG_CODE=VI python make-narrated-video.py`
- Per-deck flow — *runs command* — `add-avatar.py`
- docs/<topic>-present/ — *contains* — build directory
- docs/<topic>-present/ — *contains* — diagrams directory
- docs/<topic>-present/ — *contains* — assets directory
- concept-to-video skill — *is related to* — Make one diagram generator double as a reveal-video frame source with STAGE() markers
- concept-to-video skill — *is related to* — Google Cloud TTS from Windows fetch the token in bash, pass via env to Python
- concept-to-video skill — *is related to* — Audio-reactive anime mascot overlay for narrated videos (ffmpeg)
- Make one diagram generator double as a reveal-video frame source with STAGE() markers — *uses* — STAGE() markers
- Google Cloud TTS from Windows fetch the token in bash, pass via env to Python — *involves* — Google TTS
- Google Cloud TTS from Windows fetch the token in bash, pass via env to Python — *involves* — Windows
- Google Cloud TTS from Windows fetch the token in bash, pass via env to Python — *involves* — bash
- Google Cloud TTS from Windows fetch the token in bash, pass via env to Python — *involves* — Python
- Audio-reactive anime mascot overlay for narrated videos (ffmpeg) — *uses* — ffmpeg

%% ai-graph-end %%