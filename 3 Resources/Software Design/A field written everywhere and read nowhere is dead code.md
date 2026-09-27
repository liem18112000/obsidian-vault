---
ai_hash: 2e4aa1bb926c71e6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
source: session 2026-09-25
status: seedling
tags:
- code-review
- dead-code
- refactoring
- gotcha
title: A field written everywhere and read nowhere is dead code
type: lesson
---

# A field written everywhere and read nowhere is dead code

A field that is set at creation, carried into storage, and asserted in tests — but never read by any query or branch — is dead, no matter how alive the grep looks.

The trap is that grep says "12 hits, clearly used". The fix is to classify every hit by role:

| role | means |
|---|---|
| **write** | assignment at construction, serialization, projection into a store |
| **test** | an assertion that the write happened |
| **read** | a query filters/sorts on it, or a branch depends on it |

If the **read** column is empty, the field does nothing. Writes and tests only prove it is *stored*, not that it is *used* — a test asserting `obj.field == "x"` passes forever on a field nobody consults.

Two outcomes, and picking between them is the real decision:

- **Missed opportunity → wire it.** The data is already being captured; something downstream should be consulting it. Cheapest possible feature.
- **Genuinely dead → delete it.** Especially when the state it describes cannot occur. I removed a `supersedes` pointer because nothing was ever marked superseded — the codebase expressed that idea a different way (recency ordering). A field for an impossible state is weight with no lift.

Before deleting, check the deserializer tolerates the leftover key in already-persisted data. A `{k: v for k, v in d.items() if k in known_fields}` constructor drops unknown keys and makes the removal safe.

## The counter-check before deleting

[[An uncalled method isn't automatically dead code — check facade/convention symmetry]] is the
opposing heuristic, and both are right about different things:

- That one guards **methods in a symmetric family** — an uncalled sibling may be a deliberate API
  surface, so deleting the odd one breaks a contract.
- This one is about **data fields**, where there is no symmetry argument: a field's only purpose is
  to be read. A never-read field is not holding a slot open for anything.

So: for a *method*, ask whether peers keep equivalent uncalled members. For a *field*, ask whether
the state it describes can even occur. If it cannot, delete.

Related: [[A read filtered on a value no writer produces fails by returning empty]]

## Related

- [[A read filtered on a value no writer produces fails by returning empty]]
- [[An uncalled method isn't automatically dead code — check facade/convention symmetry]]

%% ai-graph-start %%

**Related notes:**
- [[Dead-code refcount scans flag intentional seams as unused; vet before deleting]]
- [[A read filtered on a value no writer produces fails by returning empty]]
- [[An uncalled method isn't automatically dead code — check facadeconvention symmetry]]
- [[Check every stage that writes a field, not just the one that defines it]]
- [[Idempotency guards keyed on object presence break when hydration materializes the object]]

%% ai-graph-end %%