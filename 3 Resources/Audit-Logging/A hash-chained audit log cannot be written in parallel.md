---
ai_hash: d48612b16214ebc8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: LUZ Critical Concerns - Brief Summary (2025-11-17)'
status: seedling
tags:
- audit-logging
- cryptography
- concurrency
- luz-audit
- architecture
title: A hash-chained audit log cannot be written in parallel
type: concept
---

# A hash-chained audit log cannot be written in parallel

In a hash-chained audit log, entry *n*'s fingerprint is computed over entry *n* **plus entry *n−1*'s fingerprint**. That dependency is what makes the chain tamper-evident — and it is also what makes it strictly sequential: you cannot compute log 3's fingerprint until log 2's is final.

The consequence is a throughput ceiling that **no amount of hardware removes**. One tenant is one writer, so adding application servers does not increase that tenant's write rate; the extra servers just contend for the same "last fingerprint" record. In LUZ Audit this capped a tenant at roughly 100 logs/sec, and every writer also fought over an optimistic lock on the `LastFingerprint` document.

This is the core tension of chained logging: **integrity is bought with serialization**. Recognise it early, because it is an architectural ceiling, not a tuning problem — profiling will keep pointing at lock contention while the real cause is the data dependency itself.

The escape hatch is to break the chain into independently-buildable segments and link the *segments* instead of every record — see [[Merkle checkpoints restore parallel writes to a hash-chained audit log]].

## Related

- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]

## Related

- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]

%% ai-graph-start %%

**Related notes:**
- [[Merkle checkpoints restore parallel writes to a hash-chained audit log]]
- [[LUZ Audit spent 5 database operations per log entry, ~47ms, from chain maintenance]]
- [[Per-record signatures prove authenticity but not completeness, so deletion and reordering go undetected]]
- [[A hash chain proves integrity but not authorship, so a database admin can silently rebuild it]]
- [[LUZ Critical Concerns - Brief Summary]]

%% ai-graph-end %%