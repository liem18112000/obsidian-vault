---
title: "JEV accept-side confidence is low and unreliable; the cascade win is reject-side"
created: 2026-09-22
type: observation
status: seedling
source: "session 2026-09-22 JEV J4"
tags: [jev, test-agent-v2, cascade, calibration, assured-gen]
---

# JEV accept-side confidence is low and unreliable; the cascade win is reject-side

JEV J4 calibration (test-agent-v2, N=15, real JEV Score vs real Vertex LLM judge as oracle) found the confident-**accept** cascade does **not** pay off, and pinned why.

**Data:** every suite JEV *accepted* carried low confidence (≤0.36) and only 1 of 3 was a correct accept; the LLM-accepted suites sat at conf mean 0.35 / max 0.41. So above τ=0.40 the accept fast-path never fires (0% take-rate = pure overhead); below it, it fires only on false-accepts (precision 0.00). No safe τ exists.

**Root cause:** JEV `score()` is fed only `pack summary + [kind] title` bullets, while the LLM judge reads the full scenario **steps**. JEV is structurally under-informed on exactly the accept call → unsure AND unreliable there. By contrast it rejects clearly-bad suites (mismatch/one) at high confidence (0.70–0.98), all correct.

**Consequence:** the win is on the **reject** side, not the accept side — but `_decision_gate` only fast-paths accepts (a reject always falls to the LLM because its textual issues drive regeneration). Two forks to make JEV pay off: (1) feed it judge-equivalent state (full steps) then re-calibrate; (2) build a reject-side cascade (confident reject → skip judge → regenerate). Feature stays OFF until a fork pays off.

Latency context: JEV 0.77s vs LLM median-of-3 40s (~39s/gate) — real but unreachable on the accept side.

Related: [[Calibrate a cascade threshold against the exact gate condition, not a looser proxy]].

## Related

- [[Calibrate a cascade threshold against the exact gate condition]]
- [[not a looser proxy]]
