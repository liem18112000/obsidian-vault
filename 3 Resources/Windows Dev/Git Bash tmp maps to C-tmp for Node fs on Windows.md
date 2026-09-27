---
ai_hash: 683ce4569fd65bcd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26
status: seedling
tags:
- windows
- git-bash
- nodejs
- gotcha
- paths
title: Git Bash /tmp maps to C-tmp for Node fs on Windows
type: lesson
---

# Git Bash /tmp maps to C-tmp for Node fs on Windows

When a Node script is launched from **Git Bash on Windows**, a Unix-style path like `/tmp/foo` passed to Node's `fs` resolves to `C:\tmp\foo` — **not** the Git Bash temp dir the shell redirect (`> /tmp/foo`) actually wrote to. So `bash -c 'cmd > /tmp/x'` then `node -e 'fs.readFileSync("/tmp/x")'` fails with ENOENT because the two `/tmp`s are different places.

Fix: write and read temp files via an **absolute Windows path** (e.g. the session scratchpad dir) rather than `/tmp`, and pass that path into the Node script.

Root cause: the redirect is interpreted by MSYS/Git Bash (which has its own `/tmp` mount), while Node uses the Windows path resolver that maps a leading `/` to the current drive root.

Related: [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]

## Related

- [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]

%% ai-graph-start %%

**Related notes:**
- [[MSYS c paths passed to native Windows node become Cc (ENOENT)]]
- [[Windows Python resolves a leading-slash path to C-colon-tmp, not Git Bash tmp]]
- [[Git Bash mktemp paths are unreadable by Windows python; pipe via stdin instead of a temp-file path]]
- [[Git Bash mangles Unix path args to kubectl exec — disable with MSYS_NO_PATHCONV]]
- [[Generate Excalidraw triplet from one layout model, rasterize with @resvgresvg-js]]

%% ai-graph-end %%