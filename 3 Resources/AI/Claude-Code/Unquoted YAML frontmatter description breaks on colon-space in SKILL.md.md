---
ai_hash: e51eeae5bc40967f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-10
entities:
- SKILL.md
- YAML frontmatter
- description
- colon-space
- unquoted YAML scalar
- mapping values are not allowed in this context
- Claude Code's local skill loader
- plugin/marketplace loader
- skill
- plugin repo
- single-quote
- double quotes
- trigger phrases
- yaml.safe_load
- luz-skills-plugin
- broken files
- Luz plugin repos how skills and hooks are packaged for distribution
source: session 2026-06-10
status: seedling
tags:
- yaml
- gotcha
- claude-code
- skill
- frontmatter
title: Unquoted YAML frontmatter description breaks on colon-space in SKILL.md
type: lesson
---

# Unquoted YAML frontmatter description breaks on colon-space in SKILL.md

A `SKILL.md` frontmatter `description:` written as an unquoted (plain) YAML scalar breaks with `mapping values are not allowed in this context` the moment the text contains a colon followed by a space (e.g. `preview-first: the script prints...`). YAML reads the inner `: ` as a nested mapping inside a plain scalar, which is illegal.

The trap is asymmetric: **Claude Code's local skill loader tolerates it** (skills under `~/.claude/skills/` keep working), but the **plugin/marketplace loader parses strictly** and rejects the skill — so the bug only surfaces after the skill is published to a plugin repo.

Fix: single-quote the whole description (`description: '...'`), doubling any internal apostrophes (`''`). Single quotes are safer than double here because skill descriptions are full of `"trigger phrases"`.

Prevention: after editing any SKILL.md frontmatter, validate with a quick `yaml.safe_load` sweep over `skills/**/SKILL.md` — a 2026-06-10 sweep found 5 broken files in luz-skills-plugin plus 4 more local-only skills, all from the same `: ` pattern.

Applies to [[Luz plugin repos how skills and hooks are packaged for distribution]].

## Related

- [[Luz plugin repos how skills and hooks are packaged for distribution]]

%% ai-graph-start %%

**Related notes:**
- [[luz-skills-plugin packages skills by category directory listed in plugin.json]]
- [[Colon-space in an unquoted GitHub Actions run value breaks the workflow YAML]]
- [[Luz plugin repos how skills and hooks are packaged for distribution]]
- [[Mermaid node labels with parentheses or special chars must be wrapped in double quotes]]
- [[Claude Code Bash tool collapses backslashes even inside quoted heredocs]]

**Relations:**
- SKILL.md — *contains* — YAML frontmatter
- YAML frontmatter — *includes field* — description
- description — *written as* — unquoted YAML scalar
- unquoted YAML scalar — *breaks with* — colon-space
- colon-space — *causes error* — mapping values are not allowed in this context
- Claude Code's local skill loader — *tolerates* — colon-space
- plugin/marketplace loader — *parses strictly* — skill
- plugin/marketplace loader — *rejects* — skill
- skill — *published to* — plugin repo
- single-quote — *fixes issue in* — description
- single-quote — *is safer than* — double quotes
- skill descriptions — *contain* — trigger phrases
- yaml.safe_load — *validates* — SKILL.md frontmatter
- yaml.safe_load — *found* — broken files
- broken files — *located in* — luz-skills-plugin
- broken files — *caused by* — colon-space
- SKILL.md — *applies to* — Luz plugin repos how skills and hooks are packaged for distribution
- Luz plugin repos how skills and hooks are packaged for distribution — *is related to* — Luz plugin repos how skills and hooks are packaged for distribution

%% ai-graph-end %%