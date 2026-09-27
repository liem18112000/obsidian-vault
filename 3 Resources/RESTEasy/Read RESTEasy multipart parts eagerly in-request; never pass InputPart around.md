---
title: "Read RESTEasy multipart parts eagerly in-request; never pass InputPart around"
created: 2026-09-09
type: lesson
status: seedling
source: "luz-docs-batch incident 2026-09; LUZ-132205"
tags: [resteasy, jax-rs, multipart, async, cdi, kepler-luz, pattern]
---

# Read RESTEasy multipart parts eagerly in-request; never pass InputPart around

To avoid `RESTEASY003880` (see [[RESTEASY003880 means the JAX-RS Providers thread-local is missing off-request-thread]]), **read every multipart part body exactly once, eagerly, inside the JAX-RS resource method while the request context is still on the thread** — convert each part to plain data (`JsonObject`, `byte[]`, a temp file, or a drained `InputStream`) up front. Then pass only that plain data to any audit, async, or batch consumer.

**Never** do these, because each defers the part read to a thread without the request context:
- hand the raw `MultipartFormDataInput` / `InputPart` to an async CDI event (`@ObservesAsync`, `event.fireAsync(...)` on a `ManagedExecutorService`);
- store the `InputPart` and read it in a background/batch worker;
- re-read the same part a second time in a separate layer (e.g. an audit service re-parsing the metadata part the resource already parsed).

**Also dont mask it:** catching `RESTEASY003880` and rethrowing as HTTP 400 "malformed metadata" hides an infra/context fault as a client error. Only treat genuine JSON parse failures as malformed.

This is the resolution adopted in Kepler ticket **LUZ-132205** (luz-eletter, fixVersion 0.02.97.00) — PR trail *"DO NOT pass InputPart around"* → *"New solution"* — where `DocumentDeliveryService.prepareDocumentDataCarriers` had been reading `InputPart` inside a `CompletableFuture`. The comment that settled it: *"Keep the flow in the JAX-RS context, no more thread pool/async tasks."*

The same anti-pattern lives in **luz-docs** (`DocumentUtil.getMetadataFromInputPart` / `saveReferenceFileToTemp`, re-read from `DocumentCreatingService.createDocument` via the audit path) — the fix is to parse metadata + folder ids + file bytes once in-request and pass those objects onward, not the multipart.

## Related

- [[RESTEASY003880 means the JAX-RS Providers thread-local is missing off-request-thread]]
