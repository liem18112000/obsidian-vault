---
ai_hash: 28facd665078c797
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: eArchive Performance — Detail Overview (2026-06-15)'
status: seedling
tags:
- mongodb
- indexing
- performance
- anti-pattern
- sargability
- earchive
- gotcha
title: Casting a field inside $match makes it un-indexable - $toString on _id cost
  16 seconds
type: lesson
---

# Casting a field inside $match makes it un-indexable - $toString on _id cost 16 seconds

On the eArchive document-detail page, a single-document lookup took **16 seconds** instead of **5 ms**. The cause was one line: `_id` was cast to a string inside the `$match` so it could be compared to a string parameter.

```js
// defeats the index — _id must be computed for every document
{ $match: { $expr: { $eq: [ { $toString: "$_id" }, id ] } } }

// uses the index
{ $match: { _id: ObjectId(id) } }
```

**Any expression applied to a field inside `$match` makes that field un-indexable**, because the index stores the *stored* value, not the result of a function over it. MongoDB cannot seek to `toString(_id) == "abc"`; it must materialise every document, compute the cast, and compare. A 3000× slowdown from a type convenience.

The general rule: **convert the parameter to the field's type, never the field to the parameter's type.** The same trap appears as `$toObjectId` on an `_id` range, `$toLower` on a name comparison, `$dateToString` on a timestamp filter, and `$toString` on any id — and it is not MongoDB-specific. In SQL it is exactly the non-sargable predicate: `WHERE CAST(id AS TEXT) = '123'` scans, `WHERE id = 123` seeks.

Why it is easy to miss in review: the query is **correct**, returns the right document, and passes every test. Only `explain()` distinguishes `COLLSCAN` from `IXSCAN`, and on a small dev dataset even the timing looks fine.

## Related

- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]
- [[An index only helps an aggregation before the first group, unwind, or lookup]]

## Related

- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]
- [[An index only helps an aggregation before the first group]]
- [[unwind]]
- [[or lookup]]

%% ai-graph-start %%

**Related notes:**
- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]
- [[jsonstore $in vs $nin ObjectId conversion gap]]
- [[An index only helps an aggregation before the first group, unwind, or lookup]]
- [[Check who consumes a result before optimising it - eArchive counted 128k docs for a boolean]]
- [[Mongo _id range with hex-string bounds matches nothing unless gateway coerces to ObjectId]]

%% ai-graph-end %%