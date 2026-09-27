---
ai_hash: 7afbc4dd1b18d251
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Analyze N+1 queries for REST API calculate payslip (LUZ)'
status: seedling
tags:
- performance
- n-plus-1
- hibernate
- rest
- batching
- profiling
- confluence-distilled
title: N+1 hides at the service-call layer too, not just in the ORM
type: lesson
---

# N+1 hides at the service-call layer too, not just in the ORM

N+1 is usually taught as an ORM problem — one query for the parents, N more for the children. In a service-oriented system it appears **twice**, at two different layers, and fixing only the ORM layer leaves most of the latency in place.

A single "calculate payslip for one employee" request showed both:

**Layer 1 — inside the ORM.** Loading the salary configuration pulled a list of salary-item *types*, and each type triggered another query to load itself. Classic lazy-collection N+1.
→ Fix with an **entity graph** or a purpose-written query that fetches the needed associations in one go.

**Layer 2 — across service boundaries.** The same request produced a fan of REST calls to a neighbouring service:

```
GET /luz_person/api/states?ids=4
GET /luz_person/api/…/companies?module=luz_compensation&ids=1,6,1
GET /luz_person/api/cities?ids=1113,3158,3121,2040
GET /luz_person/api/states?ids=27
POST /luz_person/api/…/persons/fetch
```

Note `states` is called **twice with different ids** — the caller looked up states one aggregate at a time instead of collecting the ids and asking once. Elsewhere, fetching company info walked workplaces and insurance contracts **one by one**.
→ Fix by collecting ids and issuing a **single batch call**. The `?ids=1113,3158,3121,2040` shape shows the API already supports it; the caller just wasn't using it.

**The diagnostic method is the real takeaway:** capture the **access log and the database log for the same single request**, side by side. The access log exposes cross-service N+1 that a profiler pointed at one JVM will never show; the query log exposes ORM N+1 that the access log cannot see. Either alone gives you half the picture, and the half you're missing is usually the expensive one — a remote round trip costs far more than a local query.

> [!tip] The tell in a log
> Look for **the same endpoint or the same table queried repeatedly with different single ids** inside one request. That pattern is always a batching opportunity, whether the repetition is SQL or HTTP.

> [!warning] Batching moves the problem if the batch is unbounded
> `?ids=…` with a list built from an unbounded parent set will eventually produce a URL too long, or a query with thousands of bind parameters. Chunk the batch (say 100 ids per call) rather than swapping N+1 for one request that fails at scale.

Related: [[PATCH removes the read-modify-write round trips that PUT-replace forces]] — the same instinct, removing round trips the design did not need.

Source: [[Analyze N+1 queries for REST API calculate payslip for 1 employee]] (LUZ, Confluence).

## Related

- [[PATCH removes the read-modify-write round trips that PUT-replace forces]]

%% ai-graph-start %%

**Related notes:**
- [[Analyze N+1 queries for REST API calculate payslips for overview salary processing]]
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[How to resolve hibernate N+1 select's problem]]
- [[Pass fetched objects down recursion instead of IDs to avoid N+1 re-fetch]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]

%% ai-graph-end %%