---
title: "Check who consumes a result before optimising it - eArchive counted 128k docs for a boolean"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: eArchive Performance — Detail Overview (2026-06-15)"
tags: [performance, api-design, mongodb, earchive, luz-docs, optimization]
---

# Check who consumes a result before optimising it - eArchive counted 128k docs for a boolean

One of the eight eArchive bottlenecks was not a slow query — it was a query that should never have run. **The UI requested a total match count on every search, and never displayed it.** The number was used only for a yes/no check ("are there any results?").

Counting matches over a 128,000-document collection is among the most expensive things the API can do: a `$group` count is `O(n)` and [[An index only helps an aggregation before the first group, unwind, or lookup|cannot be indexed away]]. The whole cost existed to produce **one bit**.

The fix is to ask the cheap question instead — `limit(1)` and check for existence, or read the page's own result length. The count only needs to be exact if a human reads the digits.

The transferable habit: **before optimising an expensive call, check what consumes its result.** Chasing the count query's performance would have been weeks of genuine but pointless work; deleting it was immediate. The question "who reads this, and how precisely?" ranks above "how do I make this faster?" — and an expensive value that feeds a boolean is the clearest possible signal that the API shape, not the query plan, is wrong.

This pattern hides well because the endpoint looks legitimately used — it *is* called, on every request. Only tracing the value to its render site reveals it never arrives anywhere.

Tracked as `LUZ-153656`; the related refactor removed `$facet` so search and count stopped being computed together (`LUZ-153934`).

## Related

- [[An index only helps an aggregation before the first group, unwind, or lookup]]
- [[Casting a field inside $match makes it un-indexable - $toString on _id cost 16 seconds]]

## Related

- [[An index only helps an aggregation before the first group]]
- [[unwind]]
- [[or lookup]]
