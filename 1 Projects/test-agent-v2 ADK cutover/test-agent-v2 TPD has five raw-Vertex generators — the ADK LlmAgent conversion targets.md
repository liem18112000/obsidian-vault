---
title: "test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets"
created: 2026-09-08
type: observation
status: seedling
source: "session 2026-09-08"
tags: [google-adk, test-agent-v2, test-plan-definition, llmagent, refactor]
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
