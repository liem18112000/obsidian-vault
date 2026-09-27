---
title: "Calibrate a cascade threshold against the exact gate condition, not a looser proxy"
created: 2026-09-22
type: lesson
status: seedling
source: "session 2026-09-22 JEV J4"
tags: [calibration, cascade, llm-judge, evaluation, gotcha]
---

# Calibrate a cascade threshold against the exact gate condition, not a looser proxy

When calibrating a confidence threshold for a **cascade / fast-path gate** (trust a cheap model above threshold τ, else fall back to the expensive one), the offline sweep MUST count only the rows the production gate would actually fast-path — not a looser proxy.

**Gotcha:** if the gate fires only on *confident accepts* (`conf >= τ AND score >= bar`) but your sweep measures precision over *all* rows with `conf >= τ`, the confident **rejects** (which the gate never fast-paths) pad the numerator and precision reads a false 1.00 at every τ. You conclude "any τ is safe" when the real accept-side take-rate is 0%.

**Rule:** mirror the exact gate predicate in the sweep. Precision = correct-outcome rate over the *fired* rows only, take-rate = |fired|/N.

Hit on JEV J4 calibration (test-agent-v2): the naive sweep showed 60% take @ 1.00 precision; the gate-accurate sweep (accepts only) showed 0% take-rate at any safe τ. The 60% were all correct rejects the accept-only gate discards.

Related: [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]].

## Related

- [[JEV accept-side confidence is low and unreliable; the cascade win is reject-side]]
