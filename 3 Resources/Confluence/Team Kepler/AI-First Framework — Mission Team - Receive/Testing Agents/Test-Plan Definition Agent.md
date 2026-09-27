---
ai_hash: b12a66a370a252ec
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49741398036'
confluence_path: 'Team Kepler > AI-First Framework — Mission Team: Receive > Testing
  Agents > Agent Loop 3 - Test-Plan Definition'
created: 2026-09-10
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- ai-agents
title: Test-Plan Definition Agent
type: source
updated: 2026-09-10
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741398036/Test-Plan+Definition+Agent
---

# Test-Plan Definition Agent

*Confluence source · Team Kepler › AI-First Framework — Mission Team: Receive › Testing Agents › Agent Loop 3 - Test-Plan Definition · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49741398036/Test-Plan+Definition+Agent) · updated 2026-09-10*

## Overview

- TPD is one Cloud Run service, ADK-native, driven over MCP/A2A.

- A `TpdRouter` (a custom `BaseAgent`) dispatches by prefix verb **and by live session state** to two nested sub-agents:

  - an `InterrogationAgent` `define`

  - an `ImplementOrchestrator` `implement` (which itself nests `interrogate` + `generate`).

- Both phases are **interrogative**

  - A resumable `…Session` state machine asks the human the judgement calls one round per turn

  - Everything persists to a shared GCS memory bank keyed by one `context_id`.

- The output is a scored, traceable BDD suite plus a codegraph **coverage matrix**.

## Architecture

- **Transport / control plane.**

  - The single MCP **gateway** (`gateway/mcp_server.py`, 6 tools: `define_plan`, `approve_plan`, `implement_plan`, `get_plan`, `get_scenarios`, `get_coverage`) forwards each tool call to the agent as an A2A `message/send` (JSON-RPC).

  - The ASGI app is `main:app` (`to_a2a(root_agent)` + `BearerAuthMiddleware`).

- Long LLM work runs **off the event loop** (`asyncio.to_thread`) so a multi-second Vertex call never starves Cloud Run's `/livez` probe.

- `TpdRouter` **(**`test_plan_definition/agent.py`**).**

  - A deterministic, **state-aware** prefix-verb router. `get-*` / `approve` are handled inline; otherwise it reads `plan-state.json` / `implement-state.json` and routes a **live** interrogation's next answer-turn back to the right sub-agent automatically — `sub_agents = [define, implement]`.

- **Shared engine.**

  - `common/interrogate` (the round engine: `generate_round`, `ingest`, the `RoundQuestions` registry),

  - `common/testplan` (`models`, `memory` writers, `coverage`, `llm` LlmAgents)

  - model **provider** (`agent_model()` → `VertexClaudeProvider`).

- **Persistence.**

  - The GCS **memory bank** at `memory/test-plan/<ctx>/`

  - **graphify codegraph** at `memory/graphify/<repo>/` (whose endpoints + hubs are the coverage denominator).

![[image-20260910-062629.png]]

## The Coverage Matrix (codegraph-driven)

`build_coverage_matrix` (`common/testplan/coverage.py`) builds the matrix whose **denominator = logic units**:

- **REQUIREMENT units** — every grounded note + insight (the ACs/behaviours).

- **CODE units** — the codegraph's inbound **endpoints** + core **god-node hubs** (graphify is symbol-level, so these are the branch proxy), read from the `common/codegraph/store.read_registry`.

Each requirement unit is covered for kind *K* iff a scenario cites it (via `source_refs`) with kind *K*.

Code units are marked **reached** when a scenario or covered note **names** them.

Outputs: a **traceability** view, a **GAP report** (uncovered `unit × kind` cells + unreached code units), a coverage **%** (`requirement_pct` / `code_pct`), and `coverage.json` / `coverage-matrix.md` (read via `get_coverage`).

It degrades to requirement-only when no codegraph exists and says so.

![[image-20260910-063445.png]]

## Persistence & resumability

All under `memory/test-plan/<ctx>/`, keyed by the one `context_id`; read-back always goes through the **JSON sidecars** (schema-drift tolerant), `.md` files are presentational:

```
questions.json  answers.json  decisions.json          # define interrogation trail
plan.json  plan.md  plan-brief.md                     # the plan (+ test_design)
implement-state.json  implement-decisions.json  implement-brief.md   # implement interrogation
test-data.json  scenarios.json  scenarios.md  steps.json
features/<name>.feature                               # BDD export
assured.json                                          # checkpoint (when enabled)
coverage.json  coverage-matrix.md                     # matrix + gap report
state.json                                            # resumable define session
```

Two session state files (`state.json`, `implement-state.json`, each `{done}`) drive resumability: the router routes a live interrogation's answer-turn back to its sub-agent, which **rehydrates** the session and continues from the persisted round.

## Invariants (do not regress)

- **I3 — one LLM call by default.** Only `generate_scenarios` calls the model on the default generate path (interrogation is heuristic); three serial blocking Vertex calls once blew the Cloud-Run timeout. `detail` / `assured` are the only opt-ins.

- **Human owns every gate.** Client-owned Yes/No before each round + `approve`; **status-as-lock** (`TestPlan.status` *is* the lock; a `draft` blocks implement).

- **No caps.** Open elicited `test_kinds`, uncapped cases; never a fixed 4-kind / 8-note ceiling.

- **Off the request path.** Long LLM work via `asyncio.to_thread`; loop state persisted so a kill resumes.

- **Layering.** `common/*` never imports `test_plan_definition`; the two agents never import each other.

%% ai-graph-start %%

**Related notes:**
- [[Agent Loop 3 - Test-Plan Definition]]
- [[Sub Agentic Loop 3.2 - Implement]]
- [[Sub Agentic Loop 3.1 - Define]]
- [[Agent Loop 4 - Test-Plan Execution]]
- [[TPD agentic loop single-pass DEFINE to APPROVE to IMPLEMENT]]

%% ai-graph-end %%