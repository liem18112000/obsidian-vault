---
title: "Score async API designs on crash recovery and multi-instance, not latency"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Large Payload Cases Research (FUT)"
tags: [api-design, async, polling, webhooks, resilience, confluence-distilled]
---

# Score async API designs on crash recovery and multi-instance, not latency

When choosing how an API should handle a long-running or large-payload request, the tempting comparison is *how fast does the client get a response*. That column almost never decides anything. The columns that do are **crash recovery** and **multi-instance**.

A research page evaluated several async shapes — return a `requestId` immediately, return a `trackingId` and poll, webhook callback, or just answer synchronously — against a fixed set of columns:

| Storage | Return to client | Tracking storage | Retry | Crash recovery | Multi-instance |
|---|---|---|---|---|---|

Scored that way, the in-memory variants collapse:

- **Library + memory** — tracking survives a restart, but the **payload is lost**, so recovery is nominal: you can tell the client the job existed, not finish it.
- **Memory only** — tracking lives only while the process does. No recovery at all.
- Under multiple instances, one of these was marked outright **BROKEN** — instance A holds the state, the client's next poll lands on instance B.

And the winning options were consistently the boring ones:

- For *"immediate results + service callback"*: **go synchronous**. The note is explicit — *"no need to use a batching library in this case."* If the caller waits anyway, the async machinery is pure complexity.
- For *"trackingId + polling"*: **call the third party first, then persist everything** across the collections that back tracking.

> [!tip] Two questions that settle most of these arguments
> **1. If this pod dies mid-request, what does the client see?** If the answer is "a tracking id that will never resolve", the design is unfinished.
> **2. If the next poll hits a different replica, does it work?** Any state in process memory answers no. See [[In-memory job throttles silently break when you scale to multiple replicas]] — same root cause, different feature.

> [!warning] Not persisting the request payload is a real constraint, so write it down
> The chosen option deliberately does **not** store the request payload. That is a legitimate trade — payloads are large and the whole point was to avoid holding them — but it means **no downstream step may need the original request for its mapping logic**. If one does, the payload (or the fields it needs) has to go into the tracking metadata. This is exactly the kind of constraint that is obvious to whoever chose it and invisible to whoever extends the feature six months later.

Related: [[If upstream holds memory until you ack, your write latency is their OOM risk]] — the reason large payloads push you toward async in the first place.

Source: [[Large Payload Cases - Research]] (FUT, Confluence).

## Related

- [[In-memory job throttles silently break when you scale to multiple replicas]]
