---
title: "Wrap the agent CLI rather than reimplementing the agent loop"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Vinnstack Agentic OS (TK)"
tags: [claude-code, agent-platform, architecture, wrapper, tooling, confluence-distilled]
---

# Wrap the agent CLI rather than reimplementing the agent loop

The durable way to build a platform around a coding agent is to **spawn the vendor's own CLI as a child process** and add everything around it — not to re-implement the agent loop against the model API.

> *"Vinnstack does not replace Claude Code — it **runs** Claude Code. Every agent turn is a spawn of the official `claude` CLI."*
>
> *"Claude Code is the engine; Vinnstack is the cockpit, the memory, and the flight plan."*

**Why wrapping beats re-implementing.** The agent loop is the fastest-moving part of the stack — tool protocols, context management, sub-agent orchestration, model defaults all change underneath you. A wrapper inherits every improvement for free; a re-implementation inherits a permanent maintenance debt and drifts further behind each release. You also inherit the vendor's tool implementations, permission model, and streaming format rather than rebuilding them.

**What a terminal-first agent genuinely lacks** — the four gaps worth building into the wrapper, and a decent checklist for any agent platform:

| Gap | What the surface adds |
|---|---|
| **No memory between sessions** | Persist every chat, decision and journal entry; re-hydrate on the next session; inject a curated long-term memory note into each run |
| **No domain context** | Feed it code knowledge graphs, the team's notes vault, and a skill library |
| **No visibility into multi-agent work** | Render sub-agent swarms live, with cost, verdicts, and replayable history |
| **No structured workflow** | Drive a governed pipeline (Epic → interrogation → PRD → Story) with a **human gate at each step** |

**The seam that makes it work:** the wrapper owns *state and policy*; the CLI owns *reasoning and tool execution*. Anything durable — memory, permissions, context assembly, run history — lives in the surface. Anything about how the model thinks stays in the engine. Keep that line clean and upgrading the engine is a version bump.

> [!tip] Streaming and cost accounting come from the engine, not your estimates
> Consuming the CLI's stream-json events gives real token counts and the run's **authoritative** usage for a per-turn cost line. Re-implementing would mean estimating tokens yourself and being subtly wrong about money.

> [!warning] A wrapper inherits the engine's failure modes too
> If the CLI changes its output format, flags, or auth flow, your surface breaks. Pin the version you spawn, test against new releases before adopting, and keep the parsing layer thin enough to fix quickly.

Related: [[Construct agent permissions per spawn instead of negotiating them per prompt]] · [[Extract reusable skills automatically from settled agent exchanges]].

Source: [[Vinnstack — Agentic OS]] and [[Vinnstack vs. Claude Code (native)]] (TK, Confluence).

## Related

- [[Construct agent permissions per spawn instead of negotiating them per prompt]]
