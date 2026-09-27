---
ai_hash: 74bcfcbdf3c06ba8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22 JEV J4
status: seedling
tags:
- editor
- autosave
- gotcha
- file-io
- agents
title: An open editor can clobber a mid-session programmatic file edit
type: lesson
---

# An open editor can clobber a mid-session programmatic file edit

A file left **open in an editor** can silently auto-save its stale in-memory buffer *over* an edit made programmatically (by a script, a tool, or an agent) while the session is running. The result is a **half-applied file**: some of your changes survive, others are reverted to the editor buffer, and the merge point can land mid-token — I hit a `print("` split across two lines, an unterminated-string `SyntaxError`.

**Guard after any scripted/programmatic write:**
1. Immediately verify the on-disk file **parses** — for Python, `python -c "import ast; ast.parse(open(f,encoding=\"utf-8\").read())"`.
2. Assert **sentinel edits** are present (grep for a unique string you just added) — catches a partial clobber that still parses.
3. If corrupted, **rewrite the whole file from a known-good version** (git HEAD or a fresh full render) rather than trying to patch the mangled state — patching a half-reverted file compounds the damage.
4. Tell the user to close the editor before continuing, or it clobbers again on the next write.

This generalizes the earlier `.excalidraw` version of the lesson — it is not specific to Excalidraw; any file type and any editor with autosave can do it.

Related: [[Piping a Python CLI through tail block-buffers stdout, looking like a hang]].

## Related

- [[Piping a Python CLI through tail block-buffers stdout, looking like a hang]]

%% ai-graph-start %%

**Related notes:**
- [[Piping a Python CLI through tail block-buffers stdout, looking like a hang]]
- [[A clobbered 0-element .excalidraw stub can get committed — recover the rich version from git history]]
- [[Never edit a shell script while it is executing]]
- [[A concurrent session's git stash can silently revert your in-progress edits]]
- [[JetBrains Excalidraw plugin rewrites the .excalidraw source field on save]]

%% ai-graph-end %%