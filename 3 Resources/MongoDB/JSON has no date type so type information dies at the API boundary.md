---
ai_hash: 835ed2777c6dd298
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Adapt luz-jsonstore to persist MongoDB Date via API (LUZ)'
status: seedling
tags:
- json
- mongodb
- bson
- dates
- serialization
- migration
- confluence-distilled
title: JSON has no date type so type information dies at the API boundary
type: lesson
---

# JSON has no date type so type information dies at the API boundary

JSON has exactly six types, and **date is not one of them**. So any service that accepts JSON and writes to a typed store loses date semantics at the boundary: the client sends `"2026-01-15T09:30:00Z"`, the API sees a `String`, and MongoDB stores a `String`. Range queries, sorts, and TTL indexes all then behave lexicographically rather than temporally — which silently *looks* right for ISO-8601 until a timezone offset or a differing precision appears.

The fix applied in `luz-jsonstore`: on write (`INSERT`/`UPDATE`), recursively walk the incoming body and promote anything matching an ISO-8601 pattern to a real BSON `Date`.

```java
public static final Pattern ISO_DATE_PATTERN =
    Pattern.compile("^\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}(\\.\\d+)?Z$");
```

Recursion matters — dates hide inside nested objects and arrays, so a top-level field scan misses most of them.

> [!warning] Pattern-sniffing types is a heuristic, and it will be wrong sometimes
> Any string that *looks* like a timestamp gets converted, whether or not it is one. A reference number, an externally-supplied identifier, or a free-text field that happens to contain an ISO date becomes a `Date` and stops round-tripping as the string the client sent. The safe alternatives, in order of preference:
> - **Declare it** — a schema or field list saying which paths are dates. Explicit beats inferred.
> - **Tag it** — a convention like `{"$date": "..."}` (what MongoDB Extended JSON does) so the intent is in the payload.
> - **Sniff it** — only when you control every producer and can accept the false positives.

**A transitional detail worth recognising.** The implementation writes *both* representations:

```java
output.put(k, v);                        // original string
output.put(k + "_convertedDate", processedValue);   // BSON Date
```

That is a **dual-write migration**, not a final design: old readers keep using the string field while new readers move to the date field, and nothing breaks at the moment of deploy. It is the right move during a migration — but it doubles those fields on every document and leaves every query needing to know which field to trust. Dual-writes need a scheduled end: backfill, cut readers over, then drop the old field. Without that step the "temporary" duplicate becomes permanent schema.

Related: [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]] — another case where the storage layer's real types drive the design.

Source: [[Adapt luz-jsonstore to allow luz-docs to persist data as MongoDB Date via API]] (LUZ, Confluence).

%% ai-graph-start %%

**Related notes:**
- [[Use a BSON endpoint instead of JSON to store MongoDB dates as native Date]]
- [[Adapt luz-jsonstore to allow luz-docs to persist data as MongoDB Date via API]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug]]
- [[End-to-end BSON API testing with the Node bson package]]

%% ai-graph-end %%