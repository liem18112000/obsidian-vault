---
ai_hash: 9443c4293ef5d55b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
source: openrig docs/reference/wave-sdlc.md, 2026-09-25
status: seedling
tags:
- openrig
- code-review
- metrics
- calibration
title: 'CONTEXT-GAP vs JUDGMENT-GAP: dispose every miss by cause'
type: model
---

# CONTEXT-GAP vs JUDGMENT-GAP: dispose every miss by cause

Label every review miss with its CAUSE in one word. The word is nearly worthless on its own; the RATE across many misses is the calibration signal.

From openrig's wave review. Two values:

- **CONTEXT-GAP** — the spec or its routes lacked what the moment needed. A planning finding; the fix lands upstream, in the inputs.
- **JUDGMENT-GAP** — the context WAS there and the call was still wrong. A builder finding; the fix is a check.

> The rate this produces is the calibration the dispatch shape tunes on.

That is the whole point. A run dominated by CONTEXT-GAP says stop tuning the process and invest in the inputs. A run dominated by JUDGMENT-GAP says the inputs are fine — add a check. Without the disposition you argue about which one to fix from anecdote, forever.

**It is often derivable, not judged.** I expected to need a model to assign it and did not. For "this output cites no requirement": if the input set was EMPTY there was nothing to cite → CONTEXT-GAP; if the input set was populated and the generator still cited none → JUDGMENT-GAP. Free, deterministic, no LLM.

**Put it where the cause is knowable.** I nearly bolted it onto a runtime failure-triage type, then realised triage answers "why did this fail when it ran" and cannot see the inputs at all. The disposition asks "why was this authored badly" — a different question needing different data. Wrong home = a field nobody can populate honestly.

Related: [[The fidelity law a one-line intent is a 201 lossy compression|The fidelity law: a one-line intent is a 20:1 lossy compression]] · [[Price one layer lower before accepting a fix]]

## Related

- [[The fidelity law a one-line intent is a 201 lossy compression|The fidelity law: a one-line intent is a 20:1 lossy compression]]
- [[Price one layer lower before accepting a fix]]

%% ai-graph-start %%

**Related notes:**
- [[The fidelity law a one-line intent is a 201 lossy compression]]
- [[Price one layer lower before accepting a fix]]
- [[Find then adversarial-refute verify pass cuts AI reviewer false positives]]

%% ai-graph-end %%