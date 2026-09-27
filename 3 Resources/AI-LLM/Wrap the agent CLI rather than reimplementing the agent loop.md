---
ai_hash: 00dba7dd32590dbe
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities:
- Agent CLI
- Agent loop
- Platform
- CLI
- Model API
- Vinnstack
- Claude Code
- Engine
- Wrapper
- Re-implementation
- Improvement
- Maintenance debt
- Tool protocols
- Context management
- Sub-agent orchestration
- Model defaults
- Vendor's tool implementations
- Permission model
- Streaming format
- Terminal-first agent
- Memory
- Domain context
- Multi-agent work
- Structured workflow
- Human gate
- State
- Policy
- Reasoning
- Tool execution
- Streaming
- Cost accounting
- Stream-json events
- Token counts
- Usage
- Output format
- Flags
- Auth flow
- Parsing layer
- Epic
- Interrogation
- PRD
- Story
- Construct agent permissions per spawn instead of negotiating them per prompt
- Extract reusable skills automatically from settled agent exchanges
- Vinnstack — Agentic OS
- Vinnstack vs. Claude Code (native)
source: 'Confluence: Vinnstack Agentic OS (TK)'
status: seedling
tags:
- claude-code
- agent-platform
- architecture
- wrapper
- tooling
- confluence-distilled
title: Wrap the agent CLI rather than reimplementing the agent loop
type: lesson
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

%% ai-graph-start %%

**Related notes:**
- [[Vinnstack — Agentic OS]]
- [[Vinnstack vs. Claude Code (native)]]
- [[Vinnstack AI calls are stateless headless claude CLI runs, not an agent runtime]]
- [[Construct agent permissions per spawn instead of negotiating them per prompt]]
- [[vinnstack spawns the local claude CLI for subscription-authenticated automation]]

**Relations:**
- Wrapper — *wraps* — Agent CLI
- Wrapper — *avoids* — Re-implementation
- Platform — *builds around* — CLI
- Re-implementation — *targets* — Model API
- Vinnstack — *runs* — Claude Code
- Claude Code — *is a type of* — Engine
- Vinnstack — *is* — Cockpit
- Vinnstack — *is* — Memory
- Vinnstack — *is* — Flight plan
- Wrapper — *inherits* — Improvement
- Re-implementation — *incurs* — Maintenance debt
- Wrapper — *inherits* — Vendor's tool implementations
- Wrapper — *inherits* — Permission model
- Wrapper — *inherits* — Streaming format
- Terminal-first agent — *lacks* — Memory
- Terminal-first agent — *lacks* — Domain context
- Terminal-first agent — *lacks* — Multi-agent work
- Terminal-first agent — *lacks* — Structured workflow
- Wrapper — *provides* — Memory
- Wrapper — *provides* — Domain context
- Wrapper — *provides* — Multi-agent work
- Wrapper — *provides* — Structured workflow
- Wrapper — *owns* — State
- Wrapper — *owns* — Policy
- CLI — *owns* — Reasoning
- CLI — *owns* — Tool execution
- Engine — *provides* — Streaming
- Engine — *provides* — Cost accounting
- CLI — *provides* — Stream-json events
- Stream-json events — *yields* — Token counts
- Stream-json events — *yields* — Usage
- Wrapper — *inherits* — Failure modes
- CLI — *changes* — Output format
- CLI — *changes* — Flags
- CLI — *changes* — Auth flow
- CLI changes — *can break* — Wrapper
- Structured workflow — *includes* — Epic
- Structured workflow — *includes* — Interrogation
- Structured workflow — *includes* — PRD
- Structured workflow — *includes* — Story
- Structured workflow — *requires* — Human gate
- Wrap the agent CLI rather than reimplementing the agent loop — *is related to* — Construct agent permissions per spawn instead of negotiating them per prompt
- Wrap the agent CLI rather than reimplementing the agent loop — *is related to* — Extract reusable skills automatically from settled agent exchanges
- Wrap the agent CLI rather than reimplementing the agent loop — *has source* — Vinnstack — Agentic OS
- Wrap the agent CLI rather than reimplementing the agent loop — *has source* — Vinnstack vs. Claude Code (native)

%% ai-graph-end %%