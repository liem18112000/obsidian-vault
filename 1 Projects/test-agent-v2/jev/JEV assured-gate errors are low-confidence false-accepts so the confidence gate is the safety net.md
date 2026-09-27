---
title: "JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net"
created: 2026-09-22
type: observation
status: seedling
source: "session 2026-09-22, EXPERIMENT-jev-calibration.md"
tags: [jev, test-agent-v2, calibration, assured-loop, finding]
---

# JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net

In the test-agent-v2 assured-generation gate, the first live calibration (real JEV Score vs the Vertex LLM judge on 3 golden plans × 4 quality variants) found that **every** JEV mistake was a *false-accept*: JEV graded a weak/mismatched suite as shippable (e.g. score 0.96 while the LLM judge gave 0.00). Critically, **all those false-accepts carried low JEV confidence** (0.37–0.63), while JEV was highly confident (0.89–0.99) exactly on the clear rejects it got right.

So the cascade `confidence ≥ τ` gate is doing precisely its job — it filters the dangerous over-lenient accepts. Fast-path precision was 1.00 for τ ≥ 0.65; the shipped default τ=0.80 is safe (100% precise, 25% take-rate on this set).

Load-bearing caveat: the run only validated the REJECT side. The LLM judge rejected all 12 suites (even the "full" goldens — likely thin offline-seeded suites + the known define-brief-scoping bug, plus JEV seeing only `[kind] title` bullets while the judge sees full steps), so there were **zero accept-side data points** — the actual latency win (JEV green-lighting a good suite to skip the LLM) is still unmeasured. Do not move the production τ on N=12; get accept-side data first. Method: [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]].

## Related

- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]

## Correction — fixed-oracle re-run (2026-09-22)

The "every JEV error was a low-confidence false-accept" claim above came from a run whose LLM **oracle was broken** (the `loads_obj` tool-call-envelope bug forced every judge verdict to 0.0). After fixing it and re-running: **JEV agreed with the LLM judge on 12/12 items** — there were *no* JEV errors to filter. So this run neither confirms nor refutes the confidence gate; with zero disagreements on N=12 the τ sweep has no precision cliff and **cannot calibrate τ**. New tension surfaced: the one correct *accept* (`LUZ-701/full`) carried **low confidence 0.37**, so at the conservative default τ=0.80 it falls back to the LLM and the latency win is not captured. Decision: keep τ=0.80, widen the golden set (borderline + genuinely-good suites), re-run. See [[loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0]].
