---
ai_hash: ab7cedb78e1ba4cd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: MongoDB Refactoring Query Optimization (TK)'
status: seedling
tags:
- mongodb
- query-building
- security
- aggregation
- edge-cases
- confluence-distilled
title: Omit query stages built from empty lists; empty and absent mean opposite things
type: gotcha
---

# Omit query stages built from empty lists; empty and absent mean opposite things

A filter built from a list that turns out to be **empty** should be omitted from the query, not passed through as an empty operand. Building it unconditionally produces a stage that is at best wasted work and at worst silently wrong.

The concrete case: a document search where the caller's JWT carries security classes.

```json
{ "exp": 1757952770, "iat": 1757909570, "security_classes": [] }
```

A tenant with **no** security classes yields an empty array. The fix was to stop supplying it:

> Query sent without supplying empty arrays to operators like `$in`, `$setIntersection`, etc.

**Why empty operands are a trap:**

- **The semantics flip depending on the operator.** `{field: {$in: []}}` matches **nothing** — so a user with no restrictions gets zero results instead of unrestricted access, exactly inverting the intent. `$setIntersection` with an empty set returns empty, which then poisons whatever consumed it. The bug is not a slow query; it is the wrong answer.
- **Even when harmless, it costs.** An extra pipeline stage that cannot change the result set still has to be planned and executed per document.
- **It obscures the plan.** Degenerate stages make `explain()` output harder to read and can steer the planner away from an index it would otherwise use.

**The rule: build the query conditionally.** Append the security-class stage only when the list is non-empty; the same for any `$in`, `$or`, or `$and` assembled from a collection. And remove stages that cannot affect the outcome — the same refactor also targeted *"remove useless stage"*.

> [!warning] Empty and absent mean opposite things for a permission filter
> *Absent* filter = "no restriction, return everything the tenant can see". *Empty* filter = "restricted to nothing, return zero rows". These are opposite outcomes produced by nearly identical code, and which one you get depends on an implicit branch nobody wrote down. Decide explicitly what an empty permission set means in your domain, write it as a named condition, and test both cases.

> [!tip] Make the degenerate case a test, not a code review item
> "Does this behave with an empty list?" is the single highest-yield test for any dynamically-assembled query. Add one fixture with an empty collection for each filter you build from a list.

Source: [[MongoDB Refactoring Query Optimization (securityClassCodes, condition for empty array, Remove]] (TK, Confluence).

%% ai-graph-start %%

**Related notes:**
- [[MongoDB Refactoring Query Optimization (securityClassCodes, condition for empty array, Remove]]
- [[jsonstore $in vs $nin ObjectId conversion gap]]
- [[A read filtered on a value no writer produces fails by returning empty]]
- [[Empty array in Postgres WHERE NOT (id = ANY(...)) deletes every row]]
- [[Shape-keyed test mocks break when production query shapes change]]

%% ai-graph-end %%