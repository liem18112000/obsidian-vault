---
title: "Vinnstack · Prompt Governance &amp; Code Grounding"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49515102402/Vinnstack+Prompt+Governance+amp+Code+Grounding
space: "TK"
topic: ai_ml
relevance: 0.731
depth: 2.51
updated: 2026-06-18
attachments: 0
tags:
  - confluence
  - ai-ml
  - space/tk
---

# Vinnstack · Prompt Governance &amp; Code Grounding

> [!info] Imported from Confluence
> Space **TK** · updated 2026-06-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49515102402/Vinnstack+Prompt+Governance+amp+Code+Grounding)
> Relevance 0.731 · topic `ai_ml`

<div hasbody="true" macro-id="20133707-546c-406e-991d-71fc0c140d29" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

How Vinnstack makes the agent follow rules — like "check the knowledge graph first, then the repo." **The headline answer:** it is **not** a `CLAUDE.md` file. Vinnstack injects its rules as an appended system prompt on every spawned run.

</div>

</div>

## What it does

- Gives every agent run the same behavioural rules — capabilities, guardrails, memory habits, output formatting, and the code-grounding procedure — without the operator configuring anything.

- Forces a consistent answer to code questions: **query the local code graph first, fall back to the real source, never leave a claim unverified.**

- Hard-blocks dangerous actions (e.g. any Bitbucket write) at the tool level, not just by asking nicely.

- Carries durable facts about you and your projects into each session so the agent isn't starting cold.

## How it works (technical)

### The mechanism — not a CLAUDE.md

All governance is the `CHAT_SYSTEM_RULE` string in `lib/ultracodeRunner.ts`, appended to every spawned session by `permissionArgs()` via the CLI's `--append-system-prompt` flag. Because it's injected programmatically per run, it:

- applies **regardless of working directory** (a repo `CLAUDE.md` only applies inside that repo);

- **can't be overridden** by a checked-out repo's own `CLAUDE.md`;

- is **versioned in the Vinnstack code**, so every operator gets identical governance.

The curated long-term memory (`<vault>/Agentic OS/Memory.md`, read by `readMemory()`, capped at 8000 chars) is folded into the same appended prompt as established background.

### Code-related prompts: graph first → source fallback

For any code-structure question (callers/callees, imports, cross-file flow, change blast-radius, where a symbol lives), the rule mandates:

1.  **Query the knowledge graph first** — local Graphify `graph.json` via `query` / `explain` / `path`. 100% local, token-free, works behind the egress firewall. Preferred over grepping the whole repo.

2.  **If the graph can't confirm it** (repo not built, node not found, ambiguous) — **verify against source directly**: Grep/Read the read-only clone under `<graphify>\repos\<repo>` if present, else fetch files via the Bitbucket read tools (`bb_get` / `bb_clone`).

3.  **Never leave it unverified** — the agent must not answer "I haven't checked yet"; it verifies and states which method it used.

This is reinforced by the `query-code-graph` Vinnstack skill, which restates the same procedure as a numbered SKILL.md.

### The enforcement stack

<div>

|  |  |
|----|----|
| Mechanism | Role |
| `--append-system-prompt` (`CHAT_SYSTEM_RULE`) | **Primary.** Injected every run; carries capabilities, the graph-first procedure, skill-consultation rules, memory rules, output rules, and the Bitbucket read-only guardrail. |
| Skills (SKILL.md) | The detailed named procedures the rule tells the agent to read and follow. |
| Tool permissions | `--permission-mode dontAsk` (headless `-p` can't prompt) + an explicit allow-list + deny-list, with precedence **deny → ask → allow** (deny always wins). `bypassPermissions` is deliberately avoided because it would skip the deny-list. |
| Curated memory | `Memory.md` folded into the prompt each run for cross-session continuity. |

</div>

<div hasbody="true" macro-id="f1b4b06b-ac51-491b-8874-2f0ae0ca090e" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

**Honest framing for readers:** the system prompt and skills *instruct* the model strongly — that's a model-following-instructions guarantee. The *hard* guarantees are the tool permissions: e.g. Bitbucket writes and `git push` are actually blocked by the deny-list, not merely discouraged.

</div>

</div>

<div hasbody="true" macro-id="b95f4263-ae57-4660-8d95-a748ae68f78c" macro-name="tip">

<span class="aui-icon aui-icon-small aui-iconfont-approve confluence-information-macro-icon"> </span>

<div>

**Direct answer:** "graph first, then repo" is enforced by the **appended system prompt + the** **`query-code-graph`** **skill + tool permissions** — not by a `CLAUDE.md`.

</div>

</div>

## Key files

- `lib/ultracodeRunner.ts` — `CHAT_SYSTEM_RULE`, `permissionArgs()` (allow/deny lists, `--add-dir`, `--append-system-prompt`), `readMemory()`.

- `vinnstack-skills/query-code-graph/SKILL.md` — the graph-first / source-fallback procedure.
