---
ai_hash: 12480c40c28ecb72
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-24
entities: []
source: session 2026-09-24
status: seedling
tags:
- excalidraw
- git
- recovery
- gotcha
- diagrams
title: A clobbered 0-element .excalidraw stub can get committed — recover the rich
  version from git history
type: lesson
---

# A clobbered 0-element .excalidraw stub can get committed — recover the rich version from git history

GOTCHA CONFIRMED + escalated: an .excalidraw open in an editor can be auto-saved as a GUTTED 206-byte, 0-element stub — and here that stub got COMMITTED (test-agent-v2/docs/full-flow.excalidraw at commit ddab2d9), so both HEAD and the working copy were empty while the rich full-flow.png stayed orphaned. The real 120-element source survived ONLY in git history (commit 42e7cc1, 124KB). RECOVERY: `git log --format=%h -- <path>` then check each rev size/element-count, and `git show <rev>:<path> > <path>` to restore the last rich version (NOT HEAD, which was the stub). ALWAYS before editing an .excalidraw: check `wc -c` + element count; a ~200-byte / 0-element file is a clobbered stub, not the diagram. Updated the 3 overview diagrams (agents-overview-flow, deployment-architecture, full-flow) to add the 4th/5th agent test-executor-agent-v2 (EXEC / Pillar 2) via Python load-edit-save scripts + the excalidraw-diagram render loop (PYTHONIOENCODING=utf-8). See [[Excalidraw file clobbered by open editor]] and [[Excalidraw diagram editing technique]].

%% ai-graph-start %%

**Related notes:**
- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]
- [[An open editor can clobber a mid-session programmatic file edit]]
- [[Excalidraw fontFamily codes + .excalidraw.png can drift out of sync]]
- [[Editing Excalidraw diagrams with committed SVG exports needs both files updated]]
- [[Editing an Excalidraw .excalidraw JSON programmatically]]

%% ai-graph-end %%