---
title: "A Claude Code hook is plugin-packageable only when its paths are relocation-safe"
created: 2026-09-11
type: lesson
status: seedling
source: "session 2026-09-11 luz-hooks-plugin sync"
tags: [claude-code, hooks, plugins, packaging, gotcha, telegram]
---

# A Claude Code hook is plugin-packageable only when its paths are relocation-safe

A Claude Code hook can be moved from ~/.claude/hooks into a distributable **marketplace plugin** (`plugins/<name>/hooks/`) only if it resolves every path in a **relocation-safe** way. Three cases decide it:

1. **Stateless reminder/gate hooks** that reference their own scripts through `${CLAUDE_PLUGIN_ROOT}` in `hooks.json` port cleanly. Examples packaged this way: `block-coauthor`, `zero-trust-web-reminder`, `obsidian-skill-reminder`, `reusable-gate`, the `handoff` trio.
2. **Hooks that persist state to an ABSOLUTE canonical dir** — e.g. `$env:USERPROFILE\.claude\hooks\state` or `~/.claude/projects/...` — also port fine, because the state location does not depend on where the script lives. `reusable-gate` shares a per-event claim file with `simplify-gate` in that state dir; `handoff` writes under `~/.claude/projects/*/handoff`.
3. **A hook that resolves its config/markers/state from `$HERE` (its own script dir)** *and* is provisioned by a companion skill that writes those files into a fixed `~/.claude/hooks/<x>/` does **NOT** port. Packaging it decouples it from its manager skill, so at the new `${CLAUDE_PLUGIN_ROOT}` location it can no longer find the config/markers the skill created.

**Worked example (case 3): the telegram bridge.** `telegram-notify.sh` / `telegram-approval.sh` / `telegram-input-poller.sh` all compute `HERE="$(cd "$(dirname "$0")" && pwd)"` then `CFG="$HERE/telegram-notify.config"`, and read the kill-switch markers, callback/input maps, and pid from `$HERE` too. The config is written by the `telegram-hook-installation` / `telegram-toggle` skills into `~/.claude/hooks/telegram/`. So it must stay in place; the repo correctly keeps it **docs-only** rather than as a plugin.

**Secret gotcha:** `telegram-notify.config` holds a **live bot token + chat id**. Never commit it. If a case-3 subsystem is ever packaged, ship a sanitized `.config.example` and exclude all runtime state (`*.jsonl` maps, `*.log`, `*.offset`, `*.pid`, session json, `__pycache__`).

Rule of thumb: *plugin-safe = paths are either `${CLAUDE_PLUGIN_ROOT}`-relative or an absolute canonical `~/.claude` location; NOT `$HERE`-relative when a separate skill owns those files.*

## Related

- [[Luz plugin repos how skills and hooks are packaged for distribution]]
- [[luz-env-config-reminder hook nudges overlay propagation for new env reads in luz repos]]
