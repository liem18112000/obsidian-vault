---
ai_hash: 303979624cc3932e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-05
entities:
- Command-scanning git-commit hooks
- flag-separated forms
- -F message files
- git commit
- PreToolUse hook
- git commit command string
- git -C <path> commit
- git -c key=val commit
- git commit -F file
- commit-msg hook
- final message
- disk
- ~/.claude/hooks/git/block-coauthor.ps1
- 7-case harness
- JSON payloads
- Block AI commit attribution by anchoring on the email, not the trailer label
source: session 2026-06-05
status: seedling
tags:
- claude-code
- hooks
- git
- regex
- gotcha
title: Command-scanning git-commit hooks miss flag-separated forms and -F message
  files
type: lesson
---

# Command-scanning git-commit hooks miss flag-separated forms and -F message files

A PreToolUse hook that blocks commits by regex-scanning the `git commit` command string has two structural gaps beyond whatever pattern it checks.

1. **Flag-separated invocations.** `\bgit\s+commit\b` only matches when `commit` immediately follows `git`, so `git -C <path> commit` or `git -c key=val commit` sail through. Match `\bgit\b[^|;&]*\bcommit\b` instead — the `[^|;&]*` allows intervening flags but stops at command separators, so `git log | grep x; svn commit` does not false-positive.

2. **Message files are invisible.** `git commit -F file` / `--file` puts the message on disk, not in the command string — no command-scanning hook can see it. Catching that requires a real git `commit-msg` hook in the repo, which inspects the final message regardless of how it was supplied.

Accept the small false-positive cost: a legit message that merely *mentions* the blocked phrase (e.g. docs about the policy) will be denied — usually the right trade for an enforcement hook.

Verified 2026-06-05 against `~/.claude/hooks/git/block-coauthor.ps1` with a 7-case harness piping JSON payloads into the script.

## Related

- [[Block AI commit attribution by anchoring on the email, not the trailer label]]

%% ai-graph-start %%

**Related notes:**
- [[Block AI commit attribution by anchoring on the email, not the trailer label]]
- [[Regex allowlists of model names go stale when vendors ship new names]]
- [[PowerShell here-string @'...'@ silently corrupts git commit messages in the Bash tool]]
- [[Pre-staged files silently merge selective commit batches - check the index first]]
- [[Cooperating PostToolUse hooks via a shared per-event SHA1 claim file]]

**Relations:**
- Command-scanning git-commit hooks — *miss* — flag-separated forms
- Command-scanning git-commit hooks — *miss* — -F message files
- PreToolUse hook — *is a type of* — Command-scanning git-commit hooks
- PreToolUse hook — *scans* — git commit command string
- git commit command string — *includes* — flag-separated forms
- git commit -F file — *does not put message in* — git commit command string
- git -C <path> commit — *is an example of* — flag-separated forms
- git -c key=val commit — *is an example of* — flag-separated forms
- git commit -F file — *is an example of* — -F message files
- git commit -F file — *puts message on* — disk
- commit-msg hook — *inspects* — final message
- commit-msg hook — *can see messages from* — -F message files
- ~/.claude/hooks/git/block-coauthor.ps1 — *is a* — git-commit hook
- ~/.claude/hooks/git/block-coauthor.ps1 — *verified against* — 7-case harness
- 7-case harness — *pipes* — JSON payloads
- JSON payloads — *into* — ~/.claude/hooks/git/block-coauthor.ps1
- Block AI commit attribution by anchoring on the email, not the trailer label — *is related to* — Command-scanning git-commit hooks

%% ai-graph-end %%