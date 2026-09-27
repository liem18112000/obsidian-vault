---
ai_hash: 55b31145a4ec9297
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
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
- [[test-agent-v2 TPD has five raw-Vertex generators — the ADK LlmAgent conversion targets]]
- [[ADK custom eval metric is a function returning EvaluationResult, loaded by dotted path]]
- [[Adopt Google ADK only when the LLM drives the tool loop; else stay a2a-sdk-direct]]

%% ai-graph-end %%