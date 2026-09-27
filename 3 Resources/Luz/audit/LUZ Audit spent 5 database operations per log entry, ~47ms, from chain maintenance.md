---
ai_hash: e04f108cbcb9b574
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: LUZ Critical Concerns - Brief Summary (2025-11-17)'
status: seedling
tags:
- luz-audit
- performance
- mongodb
- write-amplification
- kepler
title: LUZ Audit spent 5 database operations per log entry, ~47ms, from chain maintenance
type: observation
---

# LUZ Audit spent 5 database operations per log entry, ~47ms, from chain maintenance

Writing **one** LUZ audit log cost **five database operations**: insert the log, read the current `LastFingerprint`, compute the hash, update `LastFingerprint`, then update the log with its fingerprint. That is a **5x write amplification** and it measured ~**47 ms per log**.

The number matters more than it looks. Amplification does not just multiply latency — it multiplies **lock contention**, because steps 2 and 4 touch the *same* single-document hot spot for every writer in the tenant. So the cost grows worse than linearly as concurrency rises, and the optimistic-lock retries that follow are themselves counted as more operations.

The diagnostic habit worth keeping: **count the database round-trips per logical write before optimising anything else.** A 5x amplification on a hot document explains an order-of-magnitude throughput problem on its own, and no index or connection-pool tuning will touch it. The fix has to remove the round-trips (batch the fingerprint update, or move to checkpointed batches) rather than make them faster.

Related ceiling: [[A hash-chained audit log cannot be written in parallel]].

## Related

- [[A hash-chained audit log cannot be written in parallel]]
- [[LUZ Audit went from 23 to 778 logs per second by dropping the REST hop to MongoDB]]

## Related

- [[A hash-chained audit log cannot be written in parallel]]
- [[LUZ Audit went from 23 to 778 logs per second by dropping the REST hop to MongoDB]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ Audit went from 23 to 778 logs per second by dropping the REST hop to MongoDB]]
- [[A hash-chained audit log cannot be written in parallel]]
- [[LUZ Critical Concerns - Brief Summary]]
- [[Luz Audit System - Performance Optimization Proposal]]
- [[LUZ Audit Refactor- 2025-2026]]

%% ai-graph-end %%