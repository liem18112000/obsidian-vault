---
title: "Check every stage that writes a field, not just the one that defines it"
created: 2026-09-25
type: lesson
status: seedling
source: "session 2026-09-25"
tags: [pipelines, data-modelling, gotcha]
---

# Check every stage that writes a field, not just the one that defines it

A field can hold a different KIND of value depending on which pipeline stage last wrote it. Matching logic written for one shape is dead code when the other shape arrives.

I wrote a matcher against an `out_of_scope` list, reading the type that declared it: human prose like `"QR code fallback removal"`. So I matched on word overlap. But a later stage — a classifier — overwrote that same field with pack **node IDs** (`jira:LUZ-1`) and persisted the change. By the time my code ran, it was comparing prose tokens against identifiers. It would essentially never fire in production, and the unit test I had written passed happily because it used the declaring stage's shape.

**The habit:** before writing a matcher, grep every site that WRITES the field, not just the dataclass that declares it. A field is a contract between all its writers and all its readers; the declaration only tells you the type, never the shape.

This is especially likely when a pipeline stage "improves" an upstream value — reclassifying, normalising, resolving names to IDs — and persists the improvement. That write is invisible from the declaration site and usually well-intentioned.

The fix that worked was to handle both shapes explicitly and say so: exact ID-set intersection first (precise, no threshold), falling back to token overlap for the prose case. Discriminating them was one predicate — IDs have a colon and no whitespace.

Related: [[A read filtered on a value no writer produces fails by returning empty]] · [[Prove a new branch is load-bearing by reverting it]]

## Related

- [[A read filtered on a value no writer produces fails by returning empty]]
- [[Prove a new branch is load-bearing by reverting it]]
