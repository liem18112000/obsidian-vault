---
ai_hash: ea4c9245c8bf81b3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities:
- Deterministic scorer
- LLM-agent-ification
- ADK
- Custom EvalMetric
- LlmAgent
- Scorer
- Reproducibility
- LLM-driven scoring
- test-agent-v2
- test_evaluation
- evaluate_pack
- evaluate_plan
- KGA
- TPD
- Vertex generators
- google.adk.evaluation
- EvalMetric (ADK class)
- EvaluationResult
- PerInvocationResult
- EvalStatus
- adk eval
- eval/adk_metrics.py
- Native judged metrics
- hallucinations_v1
- rubric_based_final_response_quality_v1
- final_response_match_v2
- Judged tier
- Judge harness
- ModelProvider
- I8
- Judge model
- Scoring
- Default scorer
- TPD's I3 per-call budget
- When agent-ifying LLM calls
- Per-call latency budget
- Non-essential generators
- Flag
- test-agent-v2 KGA
- Explore steps
- Node-overlap rubric
- Coverage rubric
- Mutation rubric
- Oracle rubric
- Placeholders rubric
- Entities rubric
- Regex fabrication rubric
source: session 2026-09-08
status: seedling
tags:
- google-adk
- evaluation
- adk-eval
- determinism
- antipattern
title: A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK
  via custom EvalMetric, not LlmAgent
type: lesson
---

# A deterministic scorer is a negative case for LLM-agent-ification — reuse ADK via custom EvalMetric, not LlmAgent

When "reuse the agent framework more" is the goal, a **deterministic scorer/evaluator is the negative case** — you should NOT convert its scoring into an `LlmAgent`. A scorer's value is **reproducibility**: the same input must always yield the same score. Making scoring LLM-driven destroys that.

Concrete case (test-agent-v2 `test_evaluation`): the live `evaluate_pack`/`evaluate_plan` path is 100% deterministic (node-overlap, coverage, mutation, oracle, placeholders, entities, and **regex** fabrication rubrics) — zero LLM calls, by design. The KGA/TPD pattern of "convert raw-Vertex generators → `LlmAgent(output_schema)`" does NOT transfer.

The correct ADK reuse points for an evaluator are different primitives:
- **Custom `EvalMetric` functions** (ADK `google.adk.evaluation`: `EvalMetric`, `EvaluationResult`, `PerInvocationResult`, `EvalStatus`) that wrap the deterministic engine — this is `adk eval`, and test_evaluation already does it in `eval/adk_metrics.py`.
- ADK's **native judged metrics** (`hallucinations_v1`, `rubric_based_final_response_quality_v1`, `final_response_match_v2`) for the judged tier — reuse ADK's judge harness instead of a bespoke one.
- Route any judge model through the **ModelProvider** (I8).

Rule: keep the scoring deterministic + read-only by default; put any LLM-judged tier on a **separate, opt-in, offline** path — never merge it into the default scorer. (Same spirit as TPD's I3 per-call budget.)

Related: [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]], [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]].

## Related

- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]

%% ai-graph-start %%

**Related notes:**
- [[test-agent-v2 KGA has no live LlmAgent — explore steps are the first ADK LlmAgent target]]
- [[When agent-ifying LLM calls, preserve the per-call latency budget by gating non-essential generators behind a flag]]
- [[ADK custom eval metric is a function returning EvaluationResult, loaded by dotted path]]
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]

**Relations:**
- Deterministic scorer — *is_negative_case_for* — LLM-agent-ification
- ADK — *reused_via* — Custom EvalMetric
- ADK — *not_reused_via* — LlmAgent
- Scoring — *should_not_be_converted_to* — LlmAgent
- Scorer — *values* — Reproducibility
- LLM-driven scoring — *destroys* — Reproducibility
- test_evaluation — *is_concrete_case_in* — test-agent-v2
- evaluate_pack — *is_deterministic* — true
- evaluate_plan — *is_deterministic* — true
- evaluate_pack — *uses_rubric* — Node-overlap rubric
- evaluate_pack — *uses_rubric* — Coverage rubric
- evaluate_pack — *uses_rubric* — Mutation rubric
- evaluate_pack — *uses_rubric* — Oracle rubric
- evaluate_pack — *uses_rubric* — Placeholders rubric
- evaluate_pack — *uses_rubric* — Entities rubric
- evaluate_pack — *uses_rubric* — Regex fabrication rubric
- evaluate_pack — *has_zero_llm_calls* — true
- evaluate_plan — *has_zero_llm_calls* — true
- KGA/TPD pattern — *converts* — Vertex generators to LlmAgent
- KGA/TPD pattern — *does_not_transfer_to* — Deterministic scorer
- Evaluator — *uses_adk_primitive* — Custom EvalMetric
- Evaluator — *uses_adk_primitive* — Native judged metrics
- Custom EvalMetric — *is_defined_in_package* — google.adk.evaluation
- google.adk.evaluation — *includes_class* — EvalMetric (ADK class)
- google.adk.evaluation — *includes_class* — EvaluationResult
- google.adk.evaluation — *includes_class* — PerInvocationResult
- google.adk.evaluation — *includes_class* — EvalStatus
- Custom EvalMetric — *wraps* — deterministic engine
- Custom EvalMetric — *is_used_in* — adk eval
- test_evaluation — *implements_metrics_in* — eval/adk_metrics.py
- ADK — *provides* — Native judged metrics
- Native judged metrics — *includes* — hallucinations_v1
- Native judged metrics — *includes* — rubric_based_final_response_quality_v1
- Native judged metrics — *includes* — final_response_match_v2
- Native judged metrics — *is_for* — Judged tier
- ADK — *provides* — Judge harness
- Judge model — *routes_through* — ModelProvider
- ModelProvider — *is_identified_as* — I8
- Scoring — *should_be* — deterministic
- Scoring — *should_be* — read-only
- Judged tier — *should_be_on* — separate path
- Judged tier — *should_be_on* — opt-in path
- Judged tier — *should_be_on* — offline path
- Judged tier — *should_not_be_merged_into* — Default scorer
- Rule (Judged tier) — *shares_spirit_with* — TPD's I3 per-call budget
- Deterministic scorer — *is_related_to* — When agent-ifying LLM calls
- Deterministic scorer — *is_related_to* — Per-call latency budget
- Per-call latency budget — *gated_by* — Flag
- Flag — *for* — Non-essential generators
- test-agent-v2 KGA — *has_no_live* — LlmAgent
- Explore steps — *are_first_LlmAgent_target_in* — test-agent-v2 KGA

%% ai-graph-end %%