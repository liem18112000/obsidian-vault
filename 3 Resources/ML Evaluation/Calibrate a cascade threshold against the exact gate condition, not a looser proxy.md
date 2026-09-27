---
ai_hash: 8b04af8a2cee018c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-22
entities: []
source: session 2026-09-22 JEV J4
status: seedling
tags:
- calibration
- cascade
- llm-judge
- evaluation
- gotcha
title: Calibrate a cascade threshold against the exact gate condition, not a looser
  proxy
type: lesson
---

# Calibrate a cascade threshold against the exact gate condition, not a looser proxy

When calibrating a confidence threshold for a **cascade / fast-path gate** (trust a cheap model above threshold τ, else fall back to the expensive one), the offline sweep MUST count only the rows the production gate would actually fast-path — not a looser proxy.

**Gotcha:** if the gate fires only on *confident accepts* (`conf >= τ AND score >= bar`) but your sweep measures precision over *all* rows with `conf >= τ`, the confident **rejects** (which the gate never fast-paths) pad the numerator and precision reads a false 1.00 at every τ. You conclude "any τ is safe" when the real accept-side take-rate is 0%.

**Rule:** mirror the exact gate predicate in the sweep. Precision = correct-outcome rate over the *fired* rows only, take-rate = |fired|/N.

Hit on JEV J4 calibration (test-agent-v2): the naive sweep showed 60% take @ 1.00 precision; the gate-accurate sweep (accepts only) showed 0% take-rate at any safe τ. The 60% were all correct rejects the accept-only gate discards.

Related: [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]].

## Related

- [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]]

%% ai-graph-start %%

**Related notes:**
- [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]]
- [[Calibrate a cheap-model to LLM cascade threshold using the LLM judge as oracle]]
- [[JEV assured-gate errors are low-confidence false-accepts so the confidence gate is the safety net]]
- [[loads_obj largest-span rule returns the tool-call envelope so JudgeVerdict silently scored 0.0]]

%% ai-graph-end %%