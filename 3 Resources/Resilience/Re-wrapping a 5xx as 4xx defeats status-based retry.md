---
ai_hash: cf203d48e6c72f2a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: PROD investigation 2026-08-03 (invoice PDF 503)
status: seedling
tags:
- retry
- gotcha
- http-status
- fault-tolerance
- luz-store
title: Re-wrapping a 5xx as 4xx defeats status-based retry
type: lesson
---

# Re-wrapping a 5xx as 4xx defeats status-based retry

A `catch` block that converts an upstream HTTP **5xx** into a **4xx** silently disables any retry layer that decides retryability from the HTTP status code. Retry interceptors typically retry 500/502/503/504 and treat 4xx as a permanent client error — so if you catch the callee's 503 and rethrow it as `400 BAD_REQUEST`, the interceptor sees a 4xx and gives up on the first attempt. A transient blip becomes a hard, permanent failure, and the retry code looks like it works but never fires.

## Where it bit us
luz-store `LuzDocsCreatorIvyRestClientService.createDocument()` is annotated `@InvoiceRunV2Retryable` (interceptor retries 5xx, 3 attempts, exponential backoff <=30s). But its body did:

```java
catch (Exception e) {
    throw new ClientErrorException("Can not create document! ... status code 503", Status.BAD_REQUEST);
}
```

So the Ivy engine's real 503 was reshaped into a 400 *before* the interceptor's `isRetryable()` ran. Result: a momentary `luz-webclient` overload during an invoice run permanently marked invoices `PDF_CREATED_FAILED` — the retry never happened. (PROD, 2026-08-03, invoice run 8751ac56.)

## The rule
- When rethrowing across a retry boundary, **preserve the upstream status** (if the cause is a `WebApplicationException`, rethrow with its original status; only map genuine client errors to 4xx), **or**
- make the retry predicate inspect the **original cause chain**, not just the top exception's status.
- Watch the tell: an error *message* that says "503" while the thrown exception *type/status* is 400 — that mismatch is the smell.

## Related
[[Invoice Run v2 PDF creation flow]] [[MicroProfile Fault Tolerance retry]]

## Related

- [[Invoice Run v2 PDF creation flow]]
- [[MicroProfile Fault Tolerance retry]]

%% ai-graph-start %%

**Related notes:**
- [[MicroProfile @Retry can't do exponential backoff or HTTP-status-aware retry — use a manual loop]]
- [[CDI self-invocation bypasses interceptor proxy]]
- [[Snapshot for rollback must live outside retry boundary]]
- [[Naive textPayload substring matching produces false-positive log hits]]
- [[TECHNICAL_ERROR is not retried in-flight but is retry-eligible on invoice-item rerun]]

%% ai-graph-end %%