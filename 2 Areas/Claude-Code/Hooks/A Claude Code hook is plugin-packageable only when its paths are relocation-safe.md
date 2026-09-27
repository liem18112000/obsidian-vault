---
ai_hash: febf29ec6d54e12a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-11
entities:
- Claude Code hook
- plugin-packageable
- relocation-safe paths
- marketplace plugin
- ~/.claude/hooks
- plugins/<name>/hooks/
- CLAUDE_PLUGIN_ROOT
- hooks.json
- Stateless reminder/gate hooks
- block-coauthor
- zero-trust-web-reminder
- obsidian-skill-reminder
- reusable-gate
- handoff
- ABSOLUTE canonical dir
- $env:USERPROFILE\.claude\hooks\state
- ~/.claude/projects/...
- simplify-gate
- state dir
- HERE (script directory)
- companion skill
- ~/.claude/hooks/<x>/
- manager skill
- config/markers
- telegram bridge
- telegram-notify.sh
- telegram-approval.sh
- telegram-input-poller.sh
- CFG
- telegram-notify.config
- kill-switch markers
- callback/input maps
- pid
- telegram-hook-installation
- telegram-toggle
- docs-only
- plugin
- live bot token
- chat id
- case 3 hook
- .config.example
- runtime state
- '*.jsonl maps'
- '*.log'
- '*.offset'
- '*.pid'
- session json
- __pycache__
- plugin-safe
- Luz plugin repos
- skills
- luz-env-config-reminder hook
- overlay propagation
- new env reads
- luz repos
source: session 2026-09-11 luz-hooks-plugin sync
status: seedling
tags:
- claude-code
- hooks
- plugins
- packaging
- gotcha
- telegram
title: A Claude Code hook is plugin-packageable only when its paths are relocation-safe
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Relocating a hardcoded-path hook integration self-locate or patch every reference site]]
- [[luz-hooks-plugin packages each hook as its own plugin registered in marketplace.json]]
- [[Luz plugin repos how skills and hooks are packaged for distribution]]
- [[Installer skill templates drift from the live scripts they install]]
- [[PSScriptRoot-relative state breaks when a hook moves to a subfolder]]

**Relations:**
- Claude Code hook — *is_plugin_packageable_if_paths_are* — relocation-safe
- Claude Code hook — *can_be_moved_from* — ~/.claude/hooks
- Claude Code hook — *can_be_moved_to* — marketplace plugin
- marketplace plugin — *has_location_pattern* — plugins/<name>/hooks/
- Claude Code hook — *requires_path_resolution_to_be* — relocation-safe
- Stateless reminder/gate hooks — *references_scripts_via* — CLAUDE_PLUGIN_ROOT
- Stateless reminder/gate hooks — *uses_config_file* — hooks.json
- Stateless reminder/gate hooks — *portability_status* — ports cleanly
- block-coauthor — *is_example_of* — Stateless reminder/gate hooks
- zero-trust-web-reminder — *is_example_of* — Stateless reminder/gate hooks
- obsidian-skill-reminder — *is_example_of* — Stateless reminder/gate hooks
- reusable-gate — *is_example_of* — Stateless reminder/gate hooks
- handoff — *is_example_of* — Stateless reminder/gate hooks
- Claude Code hook — *persists_state_to* — ABSOLUTE canonical dir
- ABSOLUTE canonical dir — *example_path* — $env:USERPROFILE\.claude\hooks\state
- ABSOLUTE canonical dir — *example_path* — ~/.claude/projects/...
- Claude Code hook — *portability_status_when_persisting_to_absolute_dir* — ports fine
- reusable-gate — *shares_file_with* — simplify-gate
- reusable-gate — *shares_file_in* — state dir
- handoff — *writes_state_to* — ~/.claude/projects/*/handoff
- Claude Code hook — *resolves_config_from* — HERE (script directory)
- Claude Code hook — *is_provisioned_by* — companion skill
- companion skill — *writes_files_to* — ~/.claude/hooks/<x>/
- Claude Code hook — *portability_status_if_HERE_and_skill_managed* — does NOT port
- Packaging hook — *decouples* — Claude Code hook from manager skill
- Claude Code hook — *cannot_find_config_at* — CLAUDE_PLUGIN_ROOT
- manager skill — *creates* — config/markers
- telegram bridge — *is_example_of* — case 3 hook
- telegram-notify.sh — *is_part_of* — telegram bridge
- telegram-approval.sh — *is_part_of* — telegram bridge
- telegram-input-poller.sh — *is_part_of* — telegram bridge
- telegram-notify.sh — *computes* — HERE (script directory)
- telegram-approval.sh — *computes* — HERE (script directory)
- telegram-input-poller.sh — *computes* — HERE (script directory)
- telegram-notify.sh — *uses_config_variable* — CFG
- CFG — *points_to* — telegram-notify.config
- telegram bridge — *reads_from_HERE* — kill-switch markers
- telegram bridge — *reads_from_HERE* — callback/input maps
- telegram bridge — *reads_from_HERE* — pid
- telegram-notify.config — *written_by* — telegram-hook-installation
- telegram-notify.config — *written_by* — telegram-toggle
- telegram-notify.config — *written_into* — ~/.claude/hooks/telegram/
- telegram bridge — *must_stay_in_place* — true
- telegram bridge — *is_categorized_as* — docs-only
- telegram bridge — *is_not_a* — plugin
- telegram-notify.config — *holds* — live bot token
- telegram-notify.config — *holds* — chat id
- live bot token — *should_not_be* — committed
- chat id — *should_not_be* — committed
- case 3 hook — *if_packaged_should_ship* — sanitized .config.example
- case 3 hook — *if_packaged_should_exclude* — runtime state
- runtime state — *includes* — *.jsonl maps
- runtime state — *includes* — *.log
- runtime state — *includes* — *.offset
- runtime state — *includes* — *.pid
- runtime state — *includes* — session json
- runtime state — *includes* — __pycache__
- plugin-safe — *implies_paths_are* — CLAUDE_PLUGIN_ROOT-relative
- plugin-safe — *implies_paths_are* — absolute canonical ~/.claude location
- plugin-safe — *does_not_imply_paths_are* — HERE-relative when separate skill owns files
- Luz plugin repos — *describes_packaging_of* — skills
- Luz plugin repos — *describes_packaging_of* — Claude Code hook
- Luz plugin repos — *is_related_to* — Claude Code hook
- luz-env-config-reminder hook — *nudges* — overlay propagation
- overlay propagation — *is_for* — new env reads
- new env reads — *occurs_in* — luz repos
- luz-env-config-reminder hook — *is_related_to* — Claude Code hook

%% ai-graph-end %%