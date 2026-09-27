---
ai_hash: ebc76ba24586ce69
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-23
entities: []
source: session 2026-08-23
status: seedling
tags:
- git-bash
- msys
- windows
- gh-cli
- gotcha
title: Git Bash mangles gh api leading-slash paths to C:/... — set MSYS_NO_PATHCONV=1
type: gotcha
---

# Git Bash mangles gh api leading-slash paths to C:/... — set MSYS_NO_PATHCONV=1

On Windows **Git Bash / MSYS**, `gh api` calls with a leading-slash path get mangled by MSYS path conversion — the arg `/repos/OWNER/REPO/...` is rewritten to a filesystem path like `C:/Program Files/Git/repos/...`, and gh fails: `invalid API endpoint: "C:/Program Files/Git/repos/...". Your shell might be rewriting URL paths as filesystem paths.`

It's intermittent — a bare `gh api "/repos/..."` may work, but the SAME call inside command substitution `x=$(gh api "/repos/...")` triggers the rewrite.

**Fixes (any):**
- `export MSYS_NO_PATHCONV=1` (or prefix the command) — disables the conversion for that call/session. Most reliable.
- Omit the leading slash: `gh api "repos/OWNER/REPO/..."` (gh's own error message suggests this).
- `MSYS2_ARG_CONV_EXCL='*'` also works.

Same class of bug hits any tool taking URL-ish `/path` args under Git Bash (curl to unix sockets, docker, kubectl exec paths). When a Windows tool complains an API path became `C:/...`, reach for MSYS_NO_PATHCONV=1.

Source: session 2026-08-23, tracing GitHub Actions runs with gh api on Windows.

%% ai-graph-start %%

**Related notes:**
- [[Git Bash mangles Unix path args to kubectl exec — disable with MSYS_NO_PATHCONV]]
- [[Git Bash mangles absolute POSIX paths meant for a remote kubectl exec target]]
- [[Windows Python resolves a leading-slash path to C-colon-tmp, not Git Bash tmp]]
- [[Git Bash mktemp paths are unreadable by Windows python; pipe via stdin instead of a temp-file path]]
- [[MSYS c paths passed to native Windows node become Cc (ENOENT)]]

%% ai-graph-end %%