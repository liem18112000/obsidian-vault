---
ai_hash: baa69eb9e9da130d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- JEV
- assured-gate errors
- low-confidence false-accepts
- confidence gate
- safety net
- test-agent-v2 assured-generation gate
- live calibration
- JEV Score
- Vertex LLM judge
- golden plans
- JEV mistake
- false-accept
- JEV confidence
- clear rejects
- cascade `confidence ≥ τ` gate
- dangerous over-lenient accepts
- Fast-path precision
- threshold (τ)
- shipped default τ=0.80
- suites
- define-brief-scoping bug
- accept-side data points
- latency win
- Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle
- fixed-oracle re-run
- LLM oracle
- '`loads_obj` tool-call-envelope bug'
- judge verdict
- LUZ-701/full
- loads_obj largest-span rule
- golden set
source: session 2026-09-22, EXPERIMENT-jev-calibration.md
status: seedling
tags:
- jev
- test-agent-v2
- calibration
- assured-loop
- finding
title: JEV assured-gate errors are low-confidence false-accepts so the confidence
  gate is the safety net
type: observation
---

# JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net

In the test-agent-v2 assured-generation gate, the first live calibration (real JEV Score vs the Vertex LLM judge on 3 golden plans × 4 quality variants) found that **every** JEV mistake was a *false-accept*: JEV graded a weak/mismatched suite as shippable (e.g. score 0.96 while the LLM judge gave 0.00). Critically, **all those false-accepts carried low JEV confidence** (0.37–0.63), while JEV was highly confident (0.89–0.99) exactly on the clear rejects it got right.

So the cascade `confidence ≥ τ` gate is doing precisely its job — it filters the dangerous over-lenient accepts. Fast-path precision was 1.00 for τ ≥ 0.65; the shipped default τ=0.80 is safe (100% precise, 25% take-rate on this set).

Load-bearing caveat: the run only validated the REJECT side. The LLM judge rejected all 12 suites (even the "full" goldens — likely thin offline-seeded suites + the known define-brief-scoping bug, plus JEV seeing only `[kind] title` bullets while the judge sees full steps), so there were **zero accept-side data points** — the actual latency win (JEV green-lighting a good suite to skip the LLM) is still unmeasured. Do not move the production τ on N=12; get accept-side data first. Method: [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]].

## Related

- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]

## Correction — fixed-oracle re-run (2026-09-22)

The "every JEV error was a low-confidence false-accept" claim above came from a run whose LLM **oracle was broken** (the `loads_obj` tool-call-envelope bug forced every judge verdict to 0.0). After fixing it and re-running: **JEV agreed with the LLM judge on 12/12 items** — there were *no* JEV errors to filter. So this run neither confirms nor refutes the confidence gate; with zero disagreements on N=12 the τ sweep has no precision cliff and **cannot calibrate τ**. New tension surfaced: the one correct *accept* (`LUZ-701/full`) carried **low confidence 0.37**, so at the conservative default τ=0.80 it falls back to the LLM and the latency win is not captured. Decision: keep τ=0.80, widen the golden set (borderline + genuinely-good suites), re-run. See [[loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0]].

%% ai-graph-start %%

**Related notes:**
- [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]
- [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]]
- [[loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0]]
- [[Assured test generation keep an LLM test only if it builds, passes, and raises coverage]]

**Relations:**
- JEV assured-gate errors — *ARE* — low-confidence false-accepts
- confidence gate — *IS_A* — safety net
- test-agent-v2 assured-generation gate — *HAS_ERRORS* — assured-gate errors
- live calibration — *COMPARES* — JEV Score
- live calibration — *COMPARES* — Vertex LLM judge
- JEV mistake — *WAS_A* — false-accept
- false-accept — *CARRIED* — low JEV confidence
- JEV — *WAS_HIGHLY_CONFIDENT_ON* — clear rejects
- cascade `confidence ≥ τ` gate — *FILTERS* — dangerous over-lenient accepts
- cascade `confidence ≥ τ` gate — *IS_A* — confidence gate
- Fast-path precision — *WAS_FOR* — threshold (τ)
- shipped default τ=0.80 — *IS_A* — threshold (τ)
- Vertex LLM judge — *REJECTED* — suites
- JEV — *SEES* — `[kind] title` bullets
- Vertex LLM judge — *SEES* — full steps
- latency win — *IS_FROM* — JEV green-lighting good suite
- latency win — *AVOIDS* — Vertex LLM judge
- production threshold (τ) — *REQUIRES* — accept-side data points
- Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle — *IS_A* — Method
- fixed-oracle re-run — *IS_A* — Correction
- LLM oracle — *WAS_BROKEN_BY* — `loads_obj` tool-call-envelope bug
- `loads_obj` tool-call-envelope bug — *CAUSED* — judge verdict to be 0.0
- JEV — *AGREED_WITH* — LLM oracle
- fixed-oracle re-run — *DOES_NOT_CONFIRM* — confidence gate
- fixed-oracle re-run — *DOES_NOT_REFUTE* — confidence gate
- LUZ-701/full — *HAD_CONFIDENCE* — 0.37
- shipped default τ=0.80 — *CAUSES* — fallback to LLM
- fallback to LLM — *PREVENTS* — latency win
- Decision — *IS_TO_KEEP* — shipped default τ=0.80
- Decision — *IS_TO_WIDEN* — golden set
- `loads_obj` tool-call-envelope bug — *IS_DESCRIBED_BY* — loads_obj largest-span rule

%% ai-graph-end %%