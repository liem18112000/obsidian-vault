---
ai_hash: 783b903fe63a22b5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-28
entities:
- Claude Code
- Fast mode
- Opus
- HEADLESS `claude -p` runs
- '`--settings` flag'
- '`--fast` flag'
- '`CLAUDE_FAST_MODE`'
- '`ANTHROPIC_FAST_MODE`'
- '`/fast` command'
- Claude Code subscription
- Pro subscription
- Max subscription
- Team subscription
- Enterprise subscription
- Console API access
- Usage credits
- Org level fast mode
- '`claude -p` command'
- '`claude-opus-4-8` model'
- '`@anthropic-ai/claude-code` binary'
- Vinnstack
- '`lib/ai/claudeRun.ts`'
- '`code.claude.com/docs/en/fast-mode.md`'
- '`headless.md`'
- Config read into a module-level const applies only on next process launch
source: claude-code-guide agent, session 2026-07-28
status: seedling
tags:
- claude-code
- fast-mode
- headless
- cli
- vinnstack
title: Enable Claude Code fast mode in headless -p runs via --settings fastMode
type: howto
---

# Enable Claude Code fast mode in headless -p runs via --settings fastMode

Claude Code "Fast mode" (Opus with faster OUTPUT — not a smaller model; toggled interactively with `/fast`) CAN be enabled for HEADLESS `claude -p` runs — but only via the `--settings` flag, not a dedicated flag or env var.

- CLI flag: there is NO `--fast` flag. Enable it with `--settings {"fastMode": true}`.
- Env var: NONE (no `CLAUDE_FAST_MODE` / `ANTHROPIC_FAST_MODE`). `--settings` is the only lever.
- In non-interactive `-p` mode, `/fast` only works if the session was launched with `--settings {"fastMode": true}` already; you cannot toggle it on mid-run.
- Prerequisites are account/subscription-level: an active Claude Code subscription (Pro/Max/Team/Enterprise) or Console API access, usage credits enabled, and (Team/Enterprise) fast mode enabled at the org level. Then you opt in per headless session.

Example: `claude -p --model claude-opus-4-8 --settings {"fastMode": true} --permission-mode dontAsk --allowedTools ... --append-system-prompt "..."`.

For an app that spawns the bundled @anthropic-ai/claude-code binary headlessly (e.g. Vinnstack, in lib/ai/claudeRun.ts), add `--settings {"fastMode": true}` to the spawn args — ideally gated behind a config toggle. Docs: code.claude.com/docs/en/fast-mode.md and headless.md.

Related: [[Config read into a module-level const applies only on next process launch]] (same project, Vinnstack, drives Claude via headless `claude -p`).

## Related

- [[Config read into a module-level const applies only on next process launch]]

%% ai-graph-start %%

**Related notes:**
- [[Vinnstack per-request claude CLI spawn has a ~12s cold-start floor, model-independent]]
- [[Claude Code hooks fire for any spawned claude process, not just interactive sessions]]
- [[--bare forces API-key-only auth in Claude Code]]
- [[Claude Code headless auth setup-token prints a 1-year token, inject via CLAUDE_CODE_OAUTH_TOKEN]]
- [[vinnstack spawns the local claude CLI for subscription-authenticated automation]]

**Relations:**
- Fast mode — *is_a_feature_of* — Claude Code
- Fast mode — *uses* — Opus
- Fast mode — *can_be_enabled_for* — HEADLESS `claude -p` runs
- Fast mode — *is_enabled_via* — `--settings` flag
- `--settings` flag — *requires_value* — {"fastMode": true}
- HEADLESS `claude -p` runs — *do_not_support* — `--fast` flag
- HEADLESS `claude -p` runs — *do_not_support* — `CLAUDE_FAST_MODE`
- HEADLESS `claude -p` runs — *do_not_support* — `ANTHROPIC_FAST_MODE`
- `/fast` command — *works_if_session_launched_with* — `--settings` flag
- Fast mode — *requires* — Claude Code subscription
- Claude Code subscription — *includes* — Pro subscription
- Claude Code subscription — *includes* — Max subscription
- Claude Code subscription — *includes* — Team subscription
- Claude Code subscription — *includes* — Enterprise subscription
- Fast mode — *requires* — Console API access
- Fast mode — *requires* — Usage credits
- Fast mode — *requires* — Org level fast mode
- `claude -p` command — *can_use* — `claude-opus-4-8` model
- Vinnstack — *spawns* — `@anthropic-ai/claude-code` binary
- Vinnstack — *uses* — `lib/ai/claudeRun.ts`
- Fast mode — *has_documentation_at* — `code.claude.com/docs/en/fast-mode.md`
- HEADLESS `claude -p` runs — *has_documentation_at* — `headless.md`
- Config read into a module-level const applies only on next process launch — *is_related_to* — Vinnstack
- Config read into a module-level const applies only on next process launch — *is_related_to* — HEADLESS `claude -p` runs

%% ai-graph-end %%