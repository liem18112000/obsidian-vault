---
title: "When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag"
created: 2026-09-08
type: lesson
status: seedling
source: "session 2026-09-08"
tags: [google-adk, llmagent, latency, cloud-run, invariant, gotcha]
---

# When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag

When converting hand-rolled LLM calls into framework agents (e.g. ADK `LlmAgent`s), a refactor that is behaviour-preserving on outputs can still **regress latency/cost** if it changes *how many* model calls fire per request. Preserve any existing per-call budget explicitly.

Concrete case (test-agent-v2 TPD, invariant **I3**): the `implement` step must make **exactly one** LLM call by default (scenario generation), with test-data and steps generation gated behind a `TPD_LLM_DETAIL` flag (default off = detailed heuristics). This exists because three *serial blocking* Vertex calls once blocked the async event loop past Cloud Run's liveness/request timeout and killed the instance. So when agent-ifying the generators, the rule is: convert them to `LlmAgent`s, but keep the non-essential ones **flag-gated** so the default path's call count is unchanged, and add a **call-count assertion test** to lock it.

General principle: treat "number of model calls per request" as an invariant with its own test, independent of output correctness. Agent-ification tends to make it *easy* to fan out more calls — guard against it. (Making the calls async `LlmAgent`s does fix the *event-loop-blocking* half of the problem, but not the *latency budget* half.)

Related: [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]].

## Related

- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
