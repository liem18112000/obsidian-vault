---
ai_hash: 07ccd8e9f2675469
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- test-agent-v2 TPD
- raw-Vertex generators
- ADK LlmAgent
- ADK agent primitives
- KGA
- common/llm/parse.py::loads_array
- agent_model()
- llm/questions.py::claude_plan_questions
- llm/plan.py::claude_brief
- llm/testdata.py::claude_test_data
- llm/scenarios.py::claude_scenarios
- llm/steps.py::claude_steps
- define generators
- implement generators
- detail
- TPD_LLM_DETAIL
- I3 "one call"
- flagship conversion
- _STEP_BATCH
- LLM calls
- reused v1 engine
- PlanSession
- generate_round
- implement_plan orchestrator
- KGA expansion_round
- define question generator
- common/adk/interrogation.py::InterrogationAgent
- KGA refine
- QuestionGen
- D4
- IMPLEMENTATION-PLAN §refine_agent
- docs/ENHANCEMENT-tpd-llmagent.md
- milestones T0–T6
- proposed decision D16
- KGA plan
- test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent
  target
- When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential
  generators behind a flag
- ADK LlmAgent with output_schema cannot use tools or transfer to other agents
- QuestionGen to LlmAgent conversion
- common-level change
- define/QuestionGen seam
- conversion strategy
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
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[ADK LlmAgent with output_schema cannot use tools or transfer to other agents]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[Test-Plan Definition Agent]]
- [[Agent Loop 3 - Test-Plan Definition]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[Fix TPD scenario generator truncation — raise max_tokens, keep one call]]

**Relations:**
- test-agent-v2 TPD — *has* — raw-Vertex generators
- raw-Vertex generators — *are conversion targets for* — ADK LlmAgent
- test-agent-v2 TPD — *is similar to* — KGA
- test-agent-v2 TPD — *does not reuse* — ADK agent primitives
- raw-Vertex generators — *are parsed with* — common/llm/parse.py::loads_array
- agent_model() — *has* — zero TPD callers
- raw-Vertex generators — *are candidates for* — ADK LlmAgent
- llm/questions.py::claude_plan_questions — *is a* — define generator
- llm/plan.py::claude_brief — *is a* — define generator
- llm/testdata.py::claude_test_data — *is an* — implement generator
- llm/testdata.py::claude_test_data — *is gated by* — detail
- llm/testdata.py::claude_test_data — *is gated by* — TPD_LLM_DETAIL
- llm/scenarios.py::claude_scenarios — *is an* — implement generator
- llm/scenarios.py::claude_scenarios — *is also known as* — I3 "one call"
- llm/scenarios.py::claude_scenarios — *is the* — flagship conversion
- llm/steps.py::claude_steps — *is an* — implement generator
- llm/steps.py::claude_steps — *is batched by* — _STEP_BATCH
- test-agent-v2 TPD — *is harder than* — KGA
- LLM calls — *are buried inside* — reused v1 engine
- define generators — *run inside* — PlanSession
- define generators — *run inside* — generate_round
- implement generators — *run inside* — implement_plan orchestrator
- KGA expansion_round — *is top level for* — KGA
- define question generator — *sits behind* — common/adk/interrogation.py::InterrogationAgent
- common/adk/interrogation.py::InterrogationAgent — *is used by* — KGA refine
- QuestionGen to LlmAgent conversion — *is a* — common-level change
- common-level change — *affects* — test-agent-v2 TPD
- common-level change — *affects* — KGA
- QuestionGen to LlmAgent conversion — *is named* — D4
- QuestionGen to LlmAgent conversion — *is anticipated by* — IMPLEMENTATION-PLAN §refine_agent
- conversion strategy — *is* — leaf-first
- conversion strategy — *is* — implement-side
- implement-side conversion — *precedes* — define/QuestionGen seam
- flagship conversion — *is an example of* — implement-side conversion
- docs/ENHANCEMENT-tpd-llmagent.md — *is a* — Plan
- docs/ENHANCEMENT-tpd-llmagent.md — *has* — milestones T0–T6
- docs/ENHANCEMENT-tpd-llmagent.md — *has* — proposed decision D16
- docs/ENHANCEMENT-tpd-llmagent.md — *is twin of* — KGA plan
- docs/ENHANCEMENT-tpd-llmagent.md — *is related to* — test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target
- docs/ENHANCEMENT-tpd-llmagent.md — *is related to* — When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag
- docs/ENHANCEMENT-tpd-llmagent.md — *is related to* — ADK LlmAgent with output_schema cannot use tools or transfer to other agents

%% ai-graph-end %%