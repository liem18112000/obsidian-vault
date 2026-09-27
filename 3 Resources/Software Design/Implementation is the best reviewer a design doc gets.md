---
title: "Implementation is the best reviewer a design doc gets"
created: 2026-09-25
type: argument
status: seedling
source: "session 2026-09-25"
tags: [design-docs, documentation, practice]
---

# Implementation is the best reviewer a design doc gets

Writing a design doc produces confident claims about how the system behaves. Implementing it is what tests those claims — and a meaningful share of them will be wrong.

Implementing my own seven-item research report, three claims died on contact with the code:

1. **A premise about behaviour.** I asserted a resumed session never re-read its intent. It did — the checkpoint carried only progress and the constructor reloaded the source every time. The whole work item evaporated; it became a regression test pinning the good behaviour instead.
2. **A stale invariant.** I cited "exactly one LLM call" as a constraint. That invariant had been deliberately traded away earlier, and the module docstring said so plainly.
3. **A renamed knob.** The config flag I named no longer existed under that name.

Notably, item 1 would have had me *add an input to a hot path* to fix a problem that did not exist. The design doc was not merely inaccurate; acting on it would have made the system worse.

**The practice:** when implementation disproves a documented claim, correct it INLINE and MARK it — `> Corrected <date> during implementation:` — rather than silently editing. A reader then sees that the design was tested against reality, and which parts were. A doc that quietly matches the code looks identical to one nobody ever checked.

Corollary: treat "no code changed" design docs as hypotheses with an expiry date, and re-read them against the code before trusting them.

Related: [[Prove a new branch is load-bearing by reverting it]] · [[Price one layer lower before accepting a fix]]

## Related

- [[Prove a new branch is load-bearing by reverting it]]
- [[Price one layer lower before accepting a fix]]
