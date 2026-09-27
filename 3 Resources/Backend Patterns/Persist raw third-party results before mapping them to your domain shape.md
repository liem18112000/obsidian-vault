---
title: "Persist raw third-party results before mapping them to your domain shape"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Luz Batch TypeScript Sequence Diagram (FUT)"
tags: [async, batch-processing, integration, retry, resilience, confluence-distilled]
---

# Persist raw third-party results before mapping them to your domain shape

When a background job calls a third party and then transforms the answer into your domain shape, make those **two separately-persisted phases** rather than one function. The library stores the raw third-party result between them.

The contract, from an `AsyncHttpProcessor` base class callers extend:

```
Client → Controller → submitOrder() → processAsyncRequest() → HTTP 202

Background phase 1:  processBatchItems()  → library stores RAW results
Background phase 2:  mapBatchResults()    → library stores FINAL results

Polling:  Client → getOrderStatus() → getRequestStatus()
```

An implementer writes only `processBatchItems()` (fetch) and `mapBatchResults()` (transform). Submission, persistence, phase sequencing, and status reporting belong to the library.

**Why the split earns its place:**

- **A mapping bug does not cost you the third-party call.** Raw results are already durable, so you fix the mapper and re-run phase 2 — no re-fetch, no extra API spend, no new rate-limit consumption. If fetch and map are one function, a `NullPointerException` in the transform means calling the vendor again for data you already paid for.
- **Crash recovery has a meaningful resume point.** A process that dies mid-job restarts at phase 2 rather than from nothing. See [[Score async API designs on crash recovery and multi-instance, not latency]] — this is what makes recovery real rather than nominal.
- **The two phases fail for different reasons and deserve different retries.** Phase 1 fails on network, auth, and rate limits — retry with backoff. Phase 2 fails on unexpected payload shapes — retrying unchanged will fail identically, so it needs a fix-and-replay path, not a retry loop. Fusing them forces one retry policy onto two different failure classes.
- **Raw results are your evidence.** When a customer disputes an outcome, the stored raw response shows what the third party actually said, separate from what your mapper made of it.

> [!tip] The generalisable rule
> Persist at the boundary where **the data stops being someone else's and starts being yours**. That boundary is the cheapest place to resume from, and the most useful thing to have kept when something looks wrong.

> [!warning] Storing raw responses has a retention cost
> Raw third-party payloads are larger than your mapped form and may carry personal data you did not intend to keep. Set a TTL on the raw store — long enough to re-map and investigate, short enough to satisfy your retention policy — rather than letting it accumulate indefinitely.

Source: [[Luz Batch TypeScript - Sequence Diagram]] (FUT, Confluence).

## Related

- [[Score async API designs on crash recovery and multi-instance, not latency]]
