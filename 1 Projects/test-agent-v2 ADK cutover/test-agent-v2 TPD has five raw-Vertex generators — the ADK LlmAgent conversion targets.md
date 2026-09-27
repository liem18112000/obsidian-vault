---
ai_hash: c3135dc0e72faa16
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- test-agent-v2 TPD
- raw-Vertex generators
- ADK LlmAgent
- KGA
- ADK agent primitives
- common/llm/parse.py::loads_array
- agent_model()
- LlmAgent(output_schema=…)
- llm/questions.py::claude_plan_questions
- llm/plan.py::claude_brief
- llm/testdata.py::claude_test_data
- llm/scenarios.py::claude_scenarios
- llm/steps.py::claude_steps
- I3 "one call"
- flagship conversion
- _STEP_BATCH
- v1 engine
- PlanSession
- generate_round
- implement_plan
- expansion_round
- common/adk/interrogation.py::InterrogationAgent
- QuestionGen
- D4
- IMPLEMENTATION-PLAN §refine_agent
- docs/ENHANCEMENT-tpd-llmagent.md
- milestones T0–T6
- proposed decision D16
- KGA plan
- test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent
  target
- When agent-ifying LLM calls
- preserve the per-call latency budget by gating non-essential generators behind a
  flag
- ADK LlmAgent with output_schema cannot use tools or transfer to other agents
- define generators
- implement generators
- define question generator
- conversion
- common-level change
- both agents
- LLM calls
- KGA refine
- cross-cutting define/QuestionGen seam
source: session 2026-09-08
status: seedling
tags:
- google-adk
- test-agent-v2
- test-plan-definition
- llmagent
- refactor
title: test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion
  targets
type: observation
---

# test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets

In test-agent-v2, the `test_plan_definition` (TPD) agent — like KGA — reuses **none** of ADK's agent primitives; it has **five** raw-Vertex `complete()` generators, all hand-parsed with `common/llm/parse.py::loads_array`, and `agent_model()` has zero TPD callers.

The five generators (all `LlmAgent(output_schema=…)` candidates):
1. `llm/questions.py::claude_plan_questions` — define, per round (heuristic fallback).
2. `llm/plan.py::claude_brief` — define finalize (free-text restater, max_tokens=700).
3. `llm/testdata.py::claude_test_data` — implement, gated `detail`/`TPD_LLM_DETAIL`.
4. `llm/scenarios.py::claude_scenarios` — implement, the single always-on call (**the I3 "one call"**) → the **flagship** conversion.
5. `llm/steps.py::claude_steps` — implement, gated + **batched** (`_STEP_BATCH=8`).

Why TPD is harder than KGA: the LLM calls are buried **inside the reused v1 engine** (define generators run inside `PlanSession`/`generate_round`; implement generators inside the sync `implement_plan` orchestrator), not at the agent's top level like KGA's `expansion_round`. And the define question generator sits behind the **shared** `common/adk/interrogation.py::InterrogationAgent`, which **KGA refine also uses** — so converting QuestionGen to an `LlmAgent` is a common-level change touching both agents (the D4-named "QuestionGen becomes a real LlmAgent" move; also anticipated by IMPLEMENTATION-PLAN §refine_agent). Hence: convert **leaf-first**, implement-side (scenario flagship) before the cross-cutting define/QuestionGen seam.

Plan: `docs/ENHANCEMENT-tpd-llmagent.md` (milestones T0–T6, proposed decision D16). Twin of the KGA plan.

Related: [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]], [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]], [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]].

## Related

- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[When agent-ifying LLM calls]]
- [[preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]
- [[TPD IMPLEMENT makes one LLM call by default (scenarios only)]]
- [[A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent]]

**Relations:**
- test-agent-v2 TPD — *has* — five raw-Vertex generators
- raw-Vertex generators — *are* — ADK LlmAgent conversion targets
- test-agent-v2 TPD — *reuses* — none of ADK agent primitives
- raw-Vertex generators — *are hand-parsed with* — common/llm/parse.py::loads_array
- agent_model() — *has* — zero TPD callers
- llm/questions.py::claude_plan_questions — *is a candidate for* — LlmAgent(output_schema=…)
- llm/questions.py::claude_plan_questions — *defines* — per round
- llm/plan.py::claude_brief — *is a candidate for* — LlmAgent(output_schema=…)
- llm/plan.py::claude_brief — *defines* — finalize
- llm/testdata.py::claude_test_data — *is a candidate for* — LlmAgent(output_schema=…)
- llm/testdata.py::claude_test_data — *implements* — gated detail
- llm/testdata.py::claude_test_data — *implements* — TPD_LLM_DETAIL
- llm/scenarios.py::claude_scenarios — *is a candidate for* — LlmAgent(output_schema=…)
- llm/scenarios.py::claude_scenarios — *implements* — the single always-on call
- llm/scenarios.py::claude_scenarios — *is* — the I3 "one call"
- llm/scenarios.py::claude_scenarios — *is* — the flagship conversion
- llm/steps.py::claude_steps — *is a candidate for* — LlmAgent(output_schema=…)
- llm/steps.py::claude_steps — *implements* — gated
- llm/steps.py::claude_steps — *is* — batched
- llm/steps.py::claude_steps — *uses* — _STEP_BATCH=8
- TPD — *is harder than* — KGA
- LLM calls — *are buried inside* — v1 engine
- define generators — *run inside* — PlanSession
- define generators — *run inside* — generate_round
- implement generators — *run inside* — implement_plan
- KGA's expansion_round — *is at* — agent's top level
- define question generator — *sits behind* — common/adk/interrogation.py::InterrogationAgent
- common/adk/interrogation.py::InterrogationAgent — *is used by* — KGA refine
- QuestionGen — *becomes* — LlmAgent
- QuestionGen — *is a* — common-level change
- common-level change — *touches* — both agents
- QuestionGen — *is anticipated by* — IMPLEMENTATION-PLAN §refine_agent
- conversion — *strategy is* — leaf-first
- conversion — *strategy is* — implement-side
- conversion — *strategy is* — scenario flagship
- conversion — *should happen before* — cross-cutting define/QuestionGen seam
- Plan — *is documented in* — docs/ENHANCEMENT-tpd-llmagent.md
- docs/ENHANCEMENT-tpd-llmagent.md — *includes* — milestones T0–T6
- docs/ENHANCEMENT-tpd-llmagent.md — *includes* — proposed decision D16
- docs/ENHANCEMENT-tpd-llmagent.md — *is a twin of* — KGA plan
- test-agent-v2 TPD — *is related to* — test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target
- test-agent-v2 TPD — *is related to* — When agent-ifying LLM calls
- test-agent-v2 TPD — *is related to* — preserve the per-call latency budget by gating non-essential generators behind a flag
- test-agent-v2 TPD — *is related to* — ADK LlmAgent with output_schema cannot use tools or transfer to other agents

%% ai-graph-end %%