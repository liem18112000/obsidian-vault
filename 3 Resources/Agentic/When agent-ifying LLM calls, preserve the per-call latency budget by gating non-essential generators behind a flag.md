---
ai_hash: 5c15f4021a22d99d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- LLM calls
- latency budget
- non-essential generators
- flag
- framework agents
- ADK LlmAgent
- refactor
- latency/cost
- model calls per request
- implement step
- scenario generation
- test-data generation
- steps generation
- TPD_LLM_DETAIL flag
- serial blocking Vertex calls
- async event loop
- Cloud Run
- call-count assertion test
- invariant
- Agent-ification
- test-agent-v2 TPD
- raw-Vertex generators
- I3
- event-loop-blocking
source: session 2026-09-08
status: seedling
tags:
- google-adk
- llmagent
- latency
- cloud-run
- invariant
- gotcha
title: When agent-ifying LLM calls, preserve the per-call latency budget by gating
  non-essential generators behind a flag
type: lesson
---

# When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag

When converting hand-rolled LLM calls into framework agents (e.g. ADK `LlmAgent`s), a refactor that is behaviour-preserving on outputs can still **regress latency/cost** if it changes *how many* model calls fire per request. Preserve any existing per-call budget explicitly.

Concrete case (test-agent-v2 TPD, invariant **I3**): the `implement` step must make **exactly one** LLM call by default (scenario generation), with test-data and steps generation gated behind a `TPD_LLM_DETAIL` flag (default off = detailed heuristics). This exists because three *serial blocking* Vertex calls once blocked the async event loop past Cloud Run's liveness/request timeout and killed the instance. So when agent-ifying the generators, the rule is: convert them to `LlmAgent`s, but keep the non-essential ones **flag-gated** so the default path's call count is unchanged, and add a **call-count assertion test** to lock it.

General principle: treat "number of model calls per request" as an invariant with its own test, independent of output correctness. Agent-ification tends to make it *easy* to fan out more calls — guard against it. (Making the calls async `LlmAgent`s does fix the *event-loop-blocking* half of the problem, but not the *latency budget* half.)

Related: [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]].

## Related

- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]

%% ai-graph-start %%

**Related notes:**
- [[run_json_agent needed a per-call timeout or a slow Vertex call hangs implement past the server ceiling]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]
- [[Deployed TPD implement_plan trips Cloud Run liveness (event-loop blocked by Vertex gen)]]
- [[Shared model quota makes LLM fan-out worthless]]

**Relations:**
- agent-ifying LLM calls — *preserves* — latency budget
- agent-ifying LLM calls — *gates* — non-essential generators
- non-essential generators — *gated behind* — flag
- hand-rolled LLM calls — *converts to* — framework agents
- framework agents — *example* — ADK LlmAgent
- refactor — *can regress* — latency/cost
- refactor — *changes* — model calls per request
- implement step — *must make* — one LLM call
- one LLM call — *for* — scenario generation
- test-data generation — *gated by* — TPD_LLM_DETAIL flag
- steps generation — *gated by* — TPD_LLM_DETAIL flag
- TPD_LLM_DETAIL flag — *default state* — off
- serial blocking Vertex calls — *blocked* — async event loop
- async event loop — *caused timeout in* — Cloud Run
- non-essential generators — *converted to* — LlmAgents
- non-essential generators — *kept* — flag-gated
- call-count assertion test — *added to ensure* — model calls per request
- model calls per request — *is an* — invariant
- invariant — *has its own* — test
- Agent-ification — *tends to increase* — model calls per request
- async LlmAgents — *fixes* — event-loop-blocking
- async LlmAgents — *does not fix* — latency budget
- test-agent-v2 TPD — *has* — raw-Vertex generators
- ADK LlmAgent conversion — *targets* — raw-Vertex generators
- test-agent-v2 TPD — *has invariant* — I3
- I3 — *concerns* — implement step
- test-agent-v2 TPD — *related to* — test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets

%% ai-graph-end %%