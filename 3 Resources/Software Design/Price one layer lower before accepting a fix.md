---
ai_hash: e3f7bf21d6380633
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
source: openrig docs/reference/product-management-pass.md, 2026-09-25
status: seedling
tags:
- decision-making
- refactoring
- openrig
- root-cause
title: Price one layer lower before accepting a fix
type: model
---

# Price one layer lower before accepting a fix

Before accepting the fix your hands reached for, price the layer BELOW it. Decide with both prices visible, not with the first one that occurred to you.

From openrig's SDLC conventions. The tendency is to fix high — high fixes are easy to write and easy to test — and sometimes narrow is exactly right. The tells are learnable:

- **The BONE tell** (set the bone, fix low): the lower fix collapses several open problems at once, and is often SMALLER than the patch — it deletes or reuses instead of adding.
- **The BAND-AID tell** (patch is right): the lower layer is stable, correct and merely inconvenient; the defect is genuinely local; nothing downstream shares it.
- **The floor**: a fix below the primitives you own is someone else's lane, and a new engine where composition suffices fails the simplicity bar.

**The move, mechanically:** name the layer you instinctively chose; name the layer below it; state what each fixes and what each leaves broken; then choose.

It settled a real decision for me. The fix I reached for was "add an intent hierarchy" — new structure, new fields. One layer down was "route the knowledge we already store by the position field we already write". That showed every bone tell: it collapsed a whole class of bug, it reused an existing field instead of adding one, and it was smaller. The hierarchy would have been ceremony on top of the actual defect.

This is the vertical twin of "context, not instructions": that protects synthesis ACROSS seats, this protects it ACROSS layers.

Related: [[Implementation is the best reviewer a design doc gets]] · [[The fidelity law: a one-line intent is a 20:1 lossy compression]]

## Related

- [[Implementation is the best reviewer a design doc gets]]
- [[The fidelity law: a one-line intent is a 20:1 lossy compression]]

%% ai-graph-start %%

**Related notes:**
- [[The fidelity law a one-line intent is a 201 lossy compression]]
- [[Implementation is the best reviewer a design doc gets]]
- [[CONTEXT-GAP vs JUDGMENT-GAP dispose every miss by cause]]

%% ai-graph-end %%