---
title: "Claim work across pods with an expiring lease column on the row"
created: 2026-09-27
type: concept
status: seedling
source: "Confluence: Batching Design (FUT)"
tags: [leases, concurrency, multi-pod, batch-processing, crash-recovery, confluence-distilled]
---

# Claim work across pods with an expiring lease column on the row

When several pods poll the same table for work, you need exactly one to pick up each row — without a distributed lock service. A **lease column on the row itself** does it, using the database you already have.

The shape, from a batch-processing library:

```
requests(id, tenant_id, metadata, state, notification_lease_expires_at, …)
batches (id, batch_type,          state, result_check_lease_expires_at,  …)
```

and three operations per lease:

```java
boolean claimBatchForResultCheck(batchId, leaseDurationMs);  // conditional write
void    clearResultCheckLease(batchId);                      // release on success
void    recoverExpiredLeases();                              // reclaim from dead pods
```

**Claiming is a conditional update, not a read-then-write.** One statement sets `lease_expires_at = now() + duration` *where the row is unclaimed or its lease has expired*, and returns whether it affected a row. The `boolean` return is the whole concurrency control: `true` means you own it, `false` means someone else does and you move on. Because check and claim are a single atomic statement, no two pods can both win — the same rule as [[Client-assigned idempotency keys with a unique constraint beat distributed locks]].

**Expiry is what makes it crash-safe.** A pod that dies mid-work never calls `clearResultCheckLease`, so its lease simply expires and `recoverExpiredLeases()` returns the row to the pool. Compare a boolean `claimed` flag: a crash strands the row permanently and someone has to clear it by hand.

**Separate leases for separate phases.** Note there are two lease columns for two different activities — notification and result-checking — each with its own duration. A single "locked" flag would serialise unrelated work on the same row; per-activity leases let notification and result-check proceed independently.

> [!tip] Size the lease to the work, not to the poll interval
> The lease must outlast the longest plausible unit of work, or a slow-but-healthy pod loses its claim and a second pod starts duplicate work. Too long, and a crashed pod's rows sit idle. Where work duration varies, renew the lease mid-flight (heartbeat it) rather than picking a pessimistic single value — see [[Redis TTL should express liveness and be refreshed by a heartbeat]].

> [!warning] Leases give at-least-once, not exactly-once
> If a pod stalls past its lease and then wakes up, two pods briefly believe they own the row. The work itself must therefore be idempotent, or guarded by a state check on write. A lease reduces duplicate work; it does not eliminate it.

Source: [[Batching Design]] (FUT, Confluence).

## Related

- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
