---
title: "Blocking on CompletableFuture.get in a custom pool recreates the bottleneck"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Concurrency Design Patterns (LUZ)"
tags: [java, concurrency, completablefuture, threadpool, async, confluence-distilled]
---

# Blocking on CompletableFuture.get in a custom pool recreates the bottleneck

Submitting work to a custom thread pool and then immediately calling `CompletableFuture::get()` or `::join()` does not make the work asynchronous — it moves the blocking from one thread to another and **adds** a bottleneck rather than removing one.

```java
// Anti-pattern: the calling thread blocks anyway
CompletableFuture<List<Address>> future = executor.supply(() -> {
    NormalizeParallelResponse r = eireneRestClient.normalizeParallel(request);
    return AddressConverter.fromNormalizeParallelResponseBackToAddresses(r);
});
return future.get();      // <- caller (often an I/O thread) parks here
```

Two things go wrong at once:

1. **The caller still blocks.** Whatever thread invoked this — typically a container I/O or request thread from a pool sized for throughput — parks until the task finishes. You have paid the cost of a context switch and gained nothing.
2. **You have added a second, smaller queue.** Custom pools are almost always sized smaller than the default worker pool. Work that previously contended for a large pool now contends for a small one *while* holding a thread in the large one. Under load the custom pool becomes the new bottleneck, and the blocked callers pile up behind it.

The correct shape is to return the future and compose on it, so no thread waits:

```java
// Fire, compose, return — nothing blocks
return deliveryRunningExecutor
    .run(() -> importDocument(tenantId, token, carriers, ctx, tracking))
    .exceptionally(throwable -> {
        LOGGER.log(Level.SEVERE, "Deliver async failed; suspending for retry", throwable);
        deliveryProcessingStatusTrackingService.suspendTrackingDeliveryProcessingStatus(deliveryId);
        return null;
    });
```

Note the error handling: because nothing blocks, you cannot catch with `try/catch` around a `get()`. Use `exceptionally` / `handle` / `whenComplete` to attach the failure path to the future itself. Dropping the `exceptionally` is the usual way an async refactor silently starts swallowing exceptions.

**The rule of thumb:** if a method both submits to an executor and blocks on the result before returning, the executor is pure overhead — delete it and call the code synchronously, or push the future all the way up to a caller that can actually do something else while it runs. Async only pays off when the blocking stops at a boundary that can park cheaply (a request thread in a reactive stack, a message-loop, or the top of a batch job).

Source: [[Concurrency Design Patterns]] (LUZ, Confluence).

## Related

- [[Concurrency Design Patterns]]
