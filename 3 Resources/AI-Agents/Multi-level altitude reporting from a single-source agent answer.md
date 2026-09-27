---
ai_hash: 582b88459ec76bb4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities:
- Multi-level altitude reporting
- single-source agent answer
- agent frameworks
- agent
- single-source result
- Info / Data / Answer
- source of truth
- audience altitudes
- Level 1 · C-level
- Level 2 · PO / Customer
- Level 3 · Developer
- Level 4 · Deep-dive
- outcome
- risk
- cost
- features
- value delivered
- status
- APIs
- changes
- how-to
- tickets
- root cause
- internals
- traces
- Interrogation (Q/A) mode
- Agent skeleton = Instruction + Skills-Resources + Tools + Context
source: session 2026-08-17 agent-framework-skeleton diagram
status: seedling
tags:
- agents
- reporting
- design-pattern
title: Multi-level altitude reporting from a single-source agent answer
type: concept
---

# Multi-level altitude reporting from a single-source agent answer

A reporting pattern for agent frameworks: the agent computes **one** single-source result ("Info / Data / Answer" — the source of truth), then **renders that same result at several audience altitudes** instead of producing separate analyses. The answer is fixed; only the framing and level of detail change.

Four altitudes used in practice (detail increases as you go down):
- **Level 1 · C-level** — outcome, risk, cost; ~3 bullets.
- **Level 2 · PO / Customer** — features, value delivered, status.
- **Level 3 · Developer** — APIs, changes, how-to, tickets.
- **Level 4 · Deep-dive** — root cause, internals, traces.

**Why it's useful:** one computation, many consumers; guarantees the exec summary and the deep-dive can't contradict each other because they derive from the same core. Pairs with an "Interrogation (Q/A)" mode over the same core for on-demand, conversational follow-ups.

Built as the example use case in the [[Agent skeleton = Instruction + Skills-Resources + Tools + Context]] diagram.

## Related

- [[Agent skeleton = Instruction + Skills-Resources + Tools + Context]]

%% ai-graph-start %%

**Related notes:**
- [[Agent skeleton = Instruction + Skills-Resources + Tools + Context]]
- [[Ground-then-refine gathering grounds, refinement interprets and confirms]]
- [[Retrieval tiering query knowledge sources cheapest and most-trusted first]]
- [[test-agent two A2A agents share a skeleton but diverge in domain engines]]
- [[Testing Agent builds each pipeline stage as a package mirroring the knowledge_gathering skeleton]]

**Relations:**
- Multi-level altitude reporting — *is a reporting pattern for* — agent frameworks
- Multi-level altitude reporting — *involves an* — agent
- agent — *computes* — single-source result
- single-source result — *is also called* — Info / Data / Answer
- Info / Data / Answer — *is the* — source of truth
- Multi-level altitude reporting — *renders* — single-source result
- single-source result — *at* — audience altitudes
- audience altitudes — *include* — Level 1 · C-level
- audience altitudes — *include* — Level 2 · PO / Customer
- audience altitudes — *include* — Level 3 · Developer
- audience altitudes — *include* — Level 4 · Deep-dive
- Level 1 · C-level — *focuses on* — outcome
- Level 1 · C-level — *focuses on* — risk
- Level 1 · C-level — *focuses on* — cost
- Level 2 · PO / Customer — *focuses on* — features
- Level 2 · PO / Customer — *focuses on* — value delivered
- Level 2 · PO / Customer — *focuses on* — status
- Level 3 · Developer — *focuses on* — APIs
- Level 3 · Developer — *focuses on* — changes
- Level 3 · Developer — *focuses on* — how-to
- Level 3 · Developer — *focuses on* — tickets
- Level 4 · Deep-dive — *focuses on* — root cause
- Level 4 · Deep-dive — *focuses on* — internals
- Level 4 · Deep-dive — *focuses on* — traces
- Multi-level altitude reporting — *pairs with* — Interrogation (Q/A) mode
- Multi-level altitude reporting — *is an example use case in* — Agent skeleton = Instruction + Skills-Resources + Tools + Context
- Multi-level altitude reporting — *is related to* — Agent skeleton = Instruction + Skills-Resources + Tools + Context

%% ai-graph-end %%