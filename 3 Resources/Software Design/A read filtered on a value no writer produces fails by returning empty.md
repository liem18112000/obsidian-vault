---
ai_hash: b001dd4bf9d1ccc1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-25
entities: []
source: session 2026-09-25
status: seedling
tags:
- debugging
- sql
- gotcha
- silent-failure
title: A read filtered on a value no writer produces fails by returning empty
type: lesson
---

# A read filtered on a value no writer produces fails by returning empty

When a query filters on a constant, check that some writer actually produces that constant. If none does, the query returns empty forever — and empty is not an error, so nothing alerts.

The case that taught me: a semantic-recall query filtered `WHERE scope = 'shared'`, while the only writer in the codebase hardcoded `scope="context"` at creation. Nothing anywhere wrote `"shared"` except a test fixture. That arm of recall could never return a single row. It had presumably been dead since it was written, looking perfectly healthy the whole time, because a query returning zero rows is indistinguishable from "nothing matched yet".

**The check is one grep.** For every string/enum literal in a WHERE clause or filter predicate, grep the codebase for a writer emitting it. No writer → dead branch.

This is a close cousin of the write-only field: both are cases where each half looks correct in isolation and only the *join* between them is broken. Reviewing a diff will never catch it, because neither half is where the bug lives.

**Fix the READ, not the writer.** The tempting one-character fix is to make the writer emit the privileged value. Resist it when the value carries meaning — here `shared` denoted knowledge promoted beyond its origin, so having capture write it would have laundered every local record into a global one. Widening the read to accept both values preserved the distinction; promotion stayed a deliberate act.

Related: [[A field written everywhere and read nowhere is dead code]] · [[Prove a new branch is load-bearing by reverting it]]

## Related

- [[A field written everywhere and read nowhere is dead code]]
- [[Prove a new branch is load-bearing by reverting it]]

%% ai-graph-start %%

**Related notes:**
- [[A field written everywhere and read nowhere is dead code]]
- [[Check every stage that writes a field, not just the one that defines it]]
- [[Dead-code refcount scans flag intentional seams as unused; vet before deleting]]
- [[Idempotency guards keyed on object presence break when hydration materializes the object]]
- [[Prove a new branch is load-bearing by reverting it]]

%% ai-graph-end %%