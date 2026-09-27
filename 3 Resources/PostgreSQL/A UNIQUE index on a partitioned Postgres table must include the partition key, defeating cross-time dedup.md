---
ai_hash: 285e47542ecae013
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: session 2026-09-10 LEO CDP email tracking
status: seedling
tags:
- postgres
- partitioning
- dedup
- gotcha
- leo-cdp
title: A UNIQUE index on a partitioned Postgres table must include the partition key,
  defeating cross-time dedup
type: gotcha
---

# A UNIQUE index on a partitioned Postgres table must include the partition key, defeating cross-time dedup

Postgres requires every UNIQUE (or primary-key) index on a partitioned table to include **all partitioning columns**. So a "dedup" unique index on a table partitioned \`BY RANGE (event_time)\` must carry \`event_time\` in its key.

Consequence: that index can only enforce uniqueness *within a single event_time value*. It **cannot** dedup "the same logical event delivered twice at different times" — e.g. two identical webhook/pixel callbacks arriving seconds apart get distinct \`event_time\`s and both pass. The dedup index on \`cdp_raw_events\` is \`(tenant_id, source_system, event_dedup_key, event_time) WHERE event_dedup_key IS NOT NULL\` for exactly this reason.

Therefore \`ON CONFLICT\` on that index is useless for arrival-time dedup. Do it at the application layer instead: before inserting, \`SELECT 1 ... WHERE tenant_id=? AND source_system=? AND event_dedup_key=? LIMIT 1\` and skip if present. Choose the dedup_key to encode the intended uniqueness grain (e.g. \`campaign:profile:email-opened\` for one unique-open per recipient, or the provider's message/event id for webhook retries).

## Related

- [[Postgres ON CONFLICT DO UPDATE references the target row by unqualified table name]]

%% ai-graph-start %%

**Related notes:**
- [[Postgres partitioned-table UNIQUE index must include the partition key]]
- [[Postgres ON CONFLICT DO UPDATE references the target row by unqualified table name]]
- [[Migration-free idempotent upserts via deterministic uuid5 primary keys]]
- [[Customer360 CDP Kafka ingestion topic layout]]
- [[Identity-keyed CDP API breaks content-hash idempotency]]

%% ai-graph-end %%