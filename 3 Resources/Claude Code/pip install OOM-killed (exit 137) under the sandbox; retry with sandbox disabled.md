---
ai_hash: 93ad94af20a60935
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20, customer360-api
status: seedling
tags:
- claude-code
- sandbox
- oom
- exit-137
- gotcha
title: pip install OOM-killed (exit 137) under the sandbox; retry with sandbox disabled
type: lesson
---

# pip install OOM-killed (exit 137) under the sandbox; retry with sandbox disabled

When a background agent runs `pip install` inside the Claude Code sandbox, a large/compiling dependency set can be **OOM-killed** — the process dies with **exit code 137** (128 + SIGKILL 9), not a normal pip error. This looks like a dependency-resolution failure but is really the sandbox memory cap.

**Fix:** re-run the same `pip install` with the sandbox disabled (the memory ceiling is what kills it, not the packages). Observed installing the `customer360-api` requirements; the retry succeeded unchanged.

General rule: an **exit 137** from any build/install step ≈ out-of-memory (killed), so raise the memory limit or drop the sandbox rather than editing the command.

%% ai-graph-start %%

**Related notes:**
- _(none above threshold)_

%% ai-graph-end %%