---
ai_hash: c6cfd28d16ff3fca
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-07
entities:
- DistributionCacheException.isNotFound()
- luz_docs
- src/main/java/ch/klara/luz/docs/cache/distribution/DistributionCacheException.java
- WebApplicationException
- Response.Status.NOT_FOUND
- MaterializeCache.get()
- sprint-156 materialize review
- CDI self-invocation bypasses interceptor proxy
source: materialize code review 2026-06-07
status: seedling
tags:
- luz-docs
- cache
- bug
- code-review
title: DistributionCacheException.isNotFound is inverted in luz_docs
type: gotcha
---

# DistributionCacheException.isNotFound is inverted in luz_docs

`DistributionCacheException.isNotFound()` in luz_docs (src/main/java/ch/klara/luz/docs/cache/distribution/DistributionCacheException.java) is logically inverted: it returns `true` when the wrapped cause status is **not** 404, and `false` for a genuine 404.

```java
return getCause() instanceof WebApplicationException w
    && w.getResponse().getStatus() != Response.Status.NOT_FOUND.getStatusCode();
```

Any consumer writing the natural idiom `if (!e.isNotFound()) throw e; return null;` gets the opposite of intent: genuine cache misses are rethrown, real 5xx errors are swallowed as a miss. Found live in MaterializeCache.get() during the sprint-156 materialize review. As of that review there is exactly one caller — either fix the method to `==` or invert at every call site. Prefer fixing the method.

## Related

- [[CDI self-invocation bypasses interceptor proxy]]

%% ai-graph-start %%

**Related notes:**
- [[luz-docs getDocumentById returns empty object not null for missing docs]]
- [[CDI self-invocation bypasses interceptor proxy]]
- [[DualCache L1 write ignores per-call TTL (uses domain default)]]
- [[A negative cache must be a distinct state from a cache miss, or its TTL is a dead write]]
- [[Deterministic Mongo pipeline updates return matched-not-modified; treat jsonstore SC_MULTI_STATUS as benign]]

**Relations:**
- DistributionCacheException.isNotFound() — *is_inverted_in* — luz_docs
- DistributionCacheException.isNotFound() — *is_located_at* — src/main/java/ch/klara/luz/docs/cache/distribution/DistributionCacheException.java
- DistributionCacheException.isNotFound() — *returns_true_when_cause_is_not* — 404
- DistributionCacheException.isNotFound() — *returns_false_when_cause_is* — 404
- DistributionCacheException.isNotFound() — *checks_instance_of* — WebApplicationException
- WebApplicationException — *has_status* — Response.Status.NOT_FOUND
- MaterializeCache.get() — *found_issue_with* — DistributionCacheException.isNotFound()
- sprint-156 materialize review — *found_issue_with* — DistributionCacheException.isNotFound()
- sprint-156 materialize review — *found_issue_in* — MaterializeCache.get()
- CDI self-invocation bypasses interceptor proxy — *is_related_to* — DistributionCacheException.isNotFound()

%% ai-graph-end %%