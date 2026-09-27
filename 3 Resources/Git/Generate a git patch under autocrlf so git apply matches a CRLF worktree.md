---
ai_hash: 9e04e4bcc6c7760e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 deployments/proxy, 2026-08
status: seedling
tags:
- git
- windows
- crlf
- patch
- gotcha
title: Generate a git patch under autocrlf so git apply matches a CRLF worktree
type: lesson
---

# Generate a git patch under autocrlf so git apply matches a CRLF worktree

On Windows with `core.autocrlf=true` (worktree = CRLF, index/blobs = LF), a patch you intend to ship in the repo and re-apply later must be generated with the DEFAULT git settings, NOT `-c core.autocrlf=false`. `git diff -c core.autocrlf=false` emits LF context lines; `git apply` onto a CRLF worktree then fails with "patch does not apply" (trailing `\r` mismatch). Generating with plain `git diff` produces a patch that plain `git apply` re-applies cleanly (git normalizes per autocrlf).

**How to apply:** generate `git diff -- <files> > x.patch`, revert the worktree, then verify with the exact command the consumer will run — `git apply --check x.patch`. If it still balks, `git apply --ignore-whitespace` (treats the trailing `\r` as whitespace) and `git apply --3way` are the fallbacks.

Related: [[Apostrophe inside bash ${varmessage} breaks the parser|Apostrophe inside bash ${var:?message} breaks the parser]]

%% ai-graph-start %%

**Related notes:**
- [[PowerShell here-string @'...'@ silently corrupts git commit messages in the Bash tool]]
- [[A PR off a stale local main shows a huge misleading diff and CONFLICTING; use two-dot diff vs origin-main to find the real delta]]
- [[Split intermixed single-file changes into two commits via backup and intermediate edit]]
- [[Windows Git Bash mangles non-ASCII to cp1252 breaking UTF-8]]
- [[CRLF line endings on a shell script shebang cause Docker exit 127 (env bash^M not found)]]

%% ai-graph-end %%