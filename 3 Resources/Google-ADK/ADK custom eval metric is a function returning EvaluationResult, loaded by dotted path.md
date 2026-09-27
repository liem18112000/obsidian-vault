---
title: "ADK custom eval metric is a function returning EvaluationResult, loaded by dotted path"
created: 2026-09-08
type: lesson
status: seedling
source: "Plan B build 2026-09-08, google-adk 2.8.0"
tags: [google-adk, evaluation, metrics]
---

# ADK custom eval metric is a function returning EvaluationResult, loaded by dotted path

Google ADK's evaluation framework (google.adk.evaluation, ~2.8.0) is extensible **without subclassing**: a custom metric is a plain function with the signature

  (eval_metric: EvalMetric, actual_invocations: list[Invocation], expected_invocations: list[Invocation] | None, conversation_scenario) -> EvaluationResult

ADK's `custom_metric_evaluator` loads it by a **dotted `custom_function_path`** on the `EvalMetric` (`EvalMetric(metric_name=..., threshold=..., custom_function_path='pkg.mod.fn')`). The function returns `EvaluationResult(overall_score: float, overall_eval_status: EvalStatus, per_invocation_results=[PerInvocationResult(actual_invocation, expected_invocation, score, eval_status)])` (EvalStatus.PASSED/FAILED/NOT_EVALUATED). Threshold semantics: PASS when score >= threshold — so a **leak/gate metric** is score = 1.0 (ok) / 0.0 (leak) with threshold 1.0.

Key uses: (1) this lets you wrap an EXISTING scoring engine as an ADK metric — the function reads whatever it needs (e.g. persisted artifacts by a context id carried in `Invocation.user_content`) and returns the ADK result, so numbers reproduce while google-adk's eval types are genuinely on-path; (2) alongside the prebuilt metrics (PrebuiltMetrics.TOOL_TRAJECTORY_AVG_SCORE deterministic; HALLUCINATIONS_V1 / RUBRIC_BASED_FINAL_RESPONSE_QUALITY_V1 / FINAL_RESPONSE_MATCH_V2 judged). Gotcha: `metric_evaluator_registry` imports pandas (the eval extra) — plain `google-adk` lacks it. Related: [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]], [[ADK workflow agents orchestrate deterministically without an LLM-driven loop]].

## Related

- [[ADK to_a2a returns a Starlette app and accepts a custom Runner and AgentCard]]
