---
ai_hash: 82ef5cc17a488111
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22, tools/jev_calibrate.py
status: seedling
tags:
- calibration
- cascade
- llm-judge
- confidence-threshold
- evaluation
title: Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle
type: howto
---

# Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle

To pick the confidence threshold τ for a cascade that fronts an expensive LLM judge with a cheap fast model (accept the fast verdict when `confidence ≥ τ`, else fall back to the LLM), calibrate by treating the **production LLM judge itself as the reference oracle** — the cascade only needs to make the *same* accept/reject decision the LLM would.

Procedure: run both the fast model and the LLM on a set of real, VARYING-quality inputs; for each candidate τ, define fast-path = items with `confidence ≥ τ`, then measure **precision** (fraction of the fast-path where fast-accept == LLM-accept) and **take-rate** (fast-path size ÷ N). Recommend the smallest τ whose precision meets a target (e.g. 0.90). This directly trades latency/cost (take-rate) against correctness (precision) with one number.

Score both sides on **byte-identical input** (extract the state builder into one shared function) or the calibration silently measures a different thing than production. Get a spread of quality (degrade good inputs: drop items, mismatch, truncate) so confidence has range to calibrate against. Watch for a degenerate oracle: if the LLM rejects (or accepts) *everything*, you have only measured one side of the gate. Applied to test-agent-v2 JEV cascade — see [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]] and [[TypeSafe SDK Python system_one usage (v0.7.1)]].

## Related

- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]

%% ai-graph-start %%

**Related notes:**
- [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]]
- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]
- [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]]
- [[System One Models (Jev) fast type-safe calibrated decision models, not chat LLMs]]
- [[loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0]]

%% ai-graph-end %%