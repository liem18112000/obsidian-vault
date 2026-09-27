---
ai_hash: d25999c6116a0b76
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-28
entities: []
source: session 2026-08-28, test-agent /test trigger
status: seedling
tags:
- claude-code
- slash-commands
- hooks
- claude-md
title: Natural-language triggers in Claude Code are CLAUDE.md rules, not hooks
type: lesson
---

# Natural-language triggers in Claude Code are CLAUDE.md rules, not hooks

To make "when I type <phrase>, do <workflow>" work in Claude Code, you have two idiomatic mechanisms — and hooks are NOT one of them for content matching:

- **Slash command** — a markdown file at `.claude/commands/<name>.md` with YAML frontmatter (`description`, `argument-hint`) and a prompt body using `$ARGUMENTS`. The user invokes it explicitly (`/name args`). Tracked in the repo if not gitignored, so it's shared with the team.
- **CLAUDE.md rule** — a plain-language instruction in a project `CLAUDE.md` ("when the user says X, do Y…"). This is what recognizes a *natural phrase* typed in a normal message (no slash). Loaded into context at session start.

**Hooks do NOT match message content for this.** Hooks fire on harness/tool EVENTS (PreToolUse, PostToolUse, UserPromptSubmit, etc.) and run a shell command — UserPromptSubmit can inject context on every prompt, but you don't use it to pattern-match "test <JIRA>" and branch behavior. For a language trigger, use a CLAUDE.md rule (optionally pointing at a slash command so both `/test X` and plain "test X" run the same workflow).

Gotchas: a new `.claude/commands/*` file and a new/edited project `CLAUDE.md` take effect after a Claude Code reload/restart. `.claude/settings.local.json` is gitignored, so `git status` may show `.claude/` collapsed as untracked — add the command file explicitly to track just it.

%% ai-graph-start %%

**Related notes:**
- [[Ship a workflow trigger from an MCP server (no client setup) via server instructions + prompts]]
- [[Claude Code hooks cannot run slash commands or clear-compact; they only inject additionalContext]]
- [[Claude Code hooks event model]]
- [[Claude Code hooks fire for any spawned claude process, not just interactive sessions]]
- [[Command-scanning git-commit hooks miss flag-separated forms and -F message files]]

%% ai-graph-end %%