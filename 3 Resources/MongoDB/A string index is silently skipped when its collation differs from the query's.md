---
title: "A string index is silently skipped when its collation differs from the query's"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Facet Count Fan-out Techniques in MongoDB (2026-06-26)"
tags: [mongodb, collation, indexing, performance, gotcha, luz-docs]
---

# A string index is silently skipped when its collation differs from the query's

MongoDB will use a string index **only if the index carries the same collation as the query**. If they differ, the index is not an error and not a warning — it is **silently ignored**, and the query falls back to a `COLLSCAN`.

This is the quietest performance cliff in MongoDB, because everything looks correct: the index exists, `getIndexes()` lists it, the query returns the right documents. Only `explain()` shows the collection scan.

It bit the `luz-docs` facet pipeline because the pipeline runs under an explicit non-simple collation:

```js
COLLATION_DEFAULT = { locale: "en", caseFirst: "UPPER" }
```

An index built without that collation (i.e. with the default simple binary collation) is invisible to those queries. The index must be **created with the matching collation**, not merely created.

Two rules that follow:

- **Collation is part of an index's identity**, like its key order. Two indexes with identical keys and different collations are different indexes, and only one of them will serve a given query.
- **When an index "isn't being used", check collation before re-examining key order.** ESR violations and collation mismatches produce the same symptom — a `COLLSCAN` where you expected an `IXSCAN` — but only one of them is visible in the index definition you are staring at.

Related cost on the same path: a non-simple collation also forces an in-memory sort, which is how deep paging hits the 32 MB limit.

## Related

- [[An index only helps an aggregation before the first group, unwind, or lookup]]
- [[Non-simple Mongo collation forces in-memory sort that hits the 32MB limit on deep paging]]

## Related

- [[An index only helps an aggregation before the first group]]
- [[unwind]]
- [[or lookup]]
- [[Non-simple Mongo collation forces in-memory sort that hits the 32MB limit on deep paging]]
