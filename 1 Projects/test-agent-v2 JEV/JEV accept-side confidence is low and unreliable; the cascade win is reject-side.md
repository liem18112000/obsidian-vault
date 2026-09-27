---
ai_hash: 63ad66000c69cf3c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities:
- JEV
- J4 calibration
- test-agent-v2
- Vertex LLM judge
- LLM
- confident-accept cascade
- accept fast-path
- reject-side cascade
- JEV score()
- pack summary + [kind] title bullets
- full scenario steps
- _decision_gate
- regeneration
- Latency
- Calibrate a cascade threshold against the exact gate condition, not a looser proxy
- fork 1
- fork 2
- Feature
source: session 2026-09-22 JEV J4
status: seedling
tags:
- jev
- test-agent-v2
- cascade
- calibration
- assured-gen
title: JEV accept-side confidence is low and unreliable; the cascade win is reject-side
type: observation
---

# JEV accept-side confidence is low and unreliable; the cascade win is reject-side

JEV J4 calibration (test-agent-v2, N=15, real JEV Score vs real Vertex LLM judge as oracle) found the confident-**accept** cascade does **not** pay off, and pinned why.

**Data:** every suite JEV *accepted* carried low confidence (≤0.36) and only 1 of 3 was a correct accept; the LLM-accepted suites sat at conf mean 0.35 / max 0.41. So above τ=0.40 the accept fast-path never fires (0% take-rate = pure overhead); below it, it fires only on false-accepts (precision 0.00). No safe τ exists.

**Root cause:** JEV `score()` is fed only `pack summary + [kind] title` bullets, while the LLM judge reads the full scenario **steps**. JEV is structurally under-informed on exactly the accept call → unsure AND unreliable there. By contrast it rejects clearly-bad suites (mismatch/one) at high confidence (0.70–0.98), all correct.

**Consequence:** the win is on the **reject** side, not the accept side — but `_decision_gate` only fast-paths accepts (a reject always falls to the LLM because its textual issues drive regeneration). Two forks to make JEV pay off: (1) feed it judge-equivalent state (full steps) then re-calibrate; (2) build a reject-side cascade (confident reject → skip judge → regenerate). Feature stays OFF until a fork pays off.

Latency context: JEV 0.77s vs LLM median-of-3 40s (~39s/gate) — real but unreachable on the accept side.

Related: [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]].

## Related

- [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]]

%% ai-graph-start %%

**Related notes:**
- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]
- [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]
- [[Integrating Jev into Test-Agent-V2 for Enhanced Decision-Making]]
- [[Understanding JEV - Mechanism, Primitives, and Calibration in Decision Making]]

**Relations:**
- J4 calibration — *is for* — JEV
- J4 calibration — *used* — test-agent-v2
- J4 calibration — *used as oracle* — Vertex LLM judge
- confident-accept cascade — *does not pay off for* — JEV
- JEV — *accepted suites have* — low confidence
- JEV — *accepted suites have precision* — 0.00
- LLM-accepted suites — *have confidence mean* — 0.35
- LLM-accepted suites — *have confidence max* — 0.41
- accept fast-path — *never fires* — above τ=0.40
- accept fast-path — *fires only on* — false-accepts below τ=0.40
- JEV score() — *is fed* — pack summary + [kind] title bullets
- Vertex LLM judge — *reads* — full scenario steps
- JEV — *is structurally under-informed on* — accept call
- JEV — *is unsure on* — accept call
- JEV — *is unreliable on* — accept call
- JEV — *rejects* — clearly-bad suites
- clearly-bad suites rejected by JEV — *at* — high confidence
- clearly-bad suites rejected by JEV — *are all* — correct
- win — *is on the* — reject side
- _decision_gate — *only fast-paths* — accepts
- reject — *falls to* — LLM
- reject textual issues — *drive* — regeneration
- JEV — *needs two forks to pay off* — fork 1
- JEV — *needs two forks to pay off* — fork 2
- fork 1 — *involves* — feeding JEV judge-equivalent state
- fork 1 — *involves* — re-calibrating JEV
- fork 2 — *involves* — building a reject-side cascade
- reject-side cascade — *skips* — LLM
- reject-side cascade — *triggers* — regeneration
- Feature — *stays OFF until* — a fork pays off
- JEV — *has latency* — 0.77s
- LLM — *has median-of-3 latency* — 40s
- LLM — *has latency per gate* — ~39s
- Latency — *is unreachable on the* — accept side
- JEV — *is related to* — Calibrate a cascade threshold against the exact gate condition, not a looser proxy

%% ai-graph-end %%