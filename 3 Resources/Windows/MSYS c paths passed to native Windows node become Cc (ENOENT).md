---
ai_hash: 5bd46e38de273977
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
entities: []
---

---
title: "MSYS /c/ paths passed to native Windows node become C:\c\ (ENOENT)"
created: 2026-08-25
type: lesson
status: seedling
source: "session 2026-08-25"
tags: [windows, nodejs, msys, git-bash, gotcha]
---

# MSYS /c/ paths passed to native Windows node become C:\c\ (ENOENT)

On Windows, native `node` does **not** understand MSYS/Git-Bash paths like `/c/Users/...`. Passing one to `node -e` (e.g. `require("fs").readFileSync("/c/Users/x")`) makes Node resolve it relative to the current drive root, turning it into `C:\c\Users\x` → `ENOENT`.

Bash builtins (`cd`, `cat`, `ls`) accept `/c/...` because MSYS translates them, but that translation does **not** apply to arguments handed to a native `.exe` such as `node`.

**Fix:** use forward-slash *Windows* paths (`C:/Users/...` — Node accepts forward slashes on Windows), or avoid the shell-quoting entirely by using the Write/Read tools for file I/O in scripts.

## Related

- [[Excalidraw JSON generator ghost-text filtering a node's rectangle by id leaves its text elements behind|Excalidraw JSON generator ghost-text: filtering a node's rectangle by id leaves its text elements behind]]

%% ai-graph-start %%

**Related notes:**
- [[Git Bash tmp maps to C-tmp for Node fs on Windows]]
- [[Git Bash mangles gh api leading-slash paths to C... — set MSYS_NO_PATHCONV=1]]
- [[Node.js process.env is case-insensitive on Windows]]
- [[Node spawn shellfalse on Windows won't run .cmd.ps1 wrappers (ENOENT)]]
- [[Git Bash mangles absolute POSIX paths meant for a remote kubectl exec target]]

%% ai-graph-end %%