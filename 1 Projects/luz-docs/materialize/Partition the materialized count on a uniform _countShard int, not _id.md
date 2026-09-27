---
ai_hash: ac47f9c571ac8dc1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-16
entities:
- Partitioning
- Materialized count
- _countShard
- _id
- MongoDB gateway
- luz_jsonstore
- PLAIN types
- ObjectId
- Indexes
- Gateway
- Divide-and-conquer count
- Stored uniform integer shard field
- Design
- luz-docs
- LUZ-154613
- _isPublic
- _effectiveSecurityClassCodes
- SHARD_SPACE
- Materialize compute
- Migration executor
- Index {_effectiveSecurityClassCodes:1,_countShard:1}
- Index {_isPublic:1,_countShard:1}
- Sub-count clause
- $gte
- $lt
- $oid
- $expr
- $toObjectId
- Date coercion
- Hash field
- _idStr
- Plain int
- ObjectId range-coercion wall
- Uniform hash
- Buckets
- Load-skew problem
- Quantile boundary sampling
- Deterministic-from-_id
- Backfill
- Idempotent
- JsonStore
- Mongo schema
- Team
- Frozen JsonStore gateway
- _id-range count fan-out
- BitmapHLL
- MongoDB $expr + $toObjectId
- Int
- String
- Existing docs
- Index-seek goal
- Simple equal int cuts
- Permission to add field
- Permission to add indexes
- This solution
- _id index
- Full scan
source: LUZ-154613 session 2026-06-16
status: seedling
tags:
- luz-docs
- materialize
- mongodb
- sharding
- performance
- design
title: Partition the materialized count on a uniform _countShard int, not _id
type: howto
---

# Partition the materialized count on a uniform _countShard int, not _id

When the MongoDB gateway (luz_jsonstore) forwards range operators only for PLAIN types (int/string) but not ObjectId, and you may add indexes but not change the gateway, the right partition key for a divide-and-conquer count is a stored uniform integer shard field — NOT _id.

Design (luz-docs LUZ-154613):
- Add `_countShard` (int) = deterministic uniform hash of _id mapped into [0, SHARD_SPACE), e.g. SHARD_SPACE = 1<<30. Stamp it in the materialize compute alongside _isPublic/_effectiveSecurityClassCodes; backfill existing docs in the migration executor.
- Index {_effectiveSecurityClassCodes:1,_countShard:1} and {_isPublic:1,_countShard:1} so each sub-count seeks its int slice INSIDE the security-matched interval (the DESIGN §5 index-seek goal).
- Sub-count clause is plain: {_countShard:{$gte:lo,$lt:hi}} → gateway forwards natively → index used. No $oid, no $expr+$toObjectId (which scans), no Date coercion.

Why a hash field beats _id or _idStr: plain int dodges the ObjectId range-coercion wall; uniform hash spreads evenly so buckets balance with simple equal int cuts → the §8 load-skew problem disappears and quantile boundary sampling is unnecessary. Deterministic-from-_id makes the backfill idempotent.

This is correct + fast + balanced with no JsonStore change — the only constraint relaxation needed is permission to add the field + indexes (Mongo schema), which the team granted.

## Related

- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]
- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]

%% ai-graph-start %%

**Related notes:**
- [[Frozen JsonStore gateway makes _id-range count fan-out a dead end — pivot to bitmapHLL]]
- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]
- [[No existing luz-docs field works as a fan-out count partition key — survey]]
- [[Mongo _id range with hex-string bounds matches nothing unless gateway coerces to ObjectId]]
- [[Divide-and-Conquer Visible-Document Count]]

**Relations:**
- Materialized count — *is partitioned by* — _countShard
- Materialized count — *is not partitioned by* — _id
- MongoDB gateway — *is also known as* — luz_jsonstore
- MongoDB gateway — *forwards range operators for* — PLAIN types
- MongoDB gateway — *does not forward range operators for* — ObjectId
- PLAIN types — *include* — Int
- PLAIN types — *include* — String
- _countShard — *is a type of* — Stored uniform integer shard field
- _countShard — *is an* — Int
- Design — *is documented in* — luz-docs
- Design — *is associated with* — LUZ-154613
- _countShard — *is a deterministic uniform hash of* — _id
- _countShard — *is mapped into* — [0, SHARD_SPACE)
- _countShard — *is stamped in* — Materialize compute
- _isPublic — *is stamped in* — Materialize compute
- _effectiveSecurityClassCodes — *is stamped in* — Materialize compute
- Migration executor — *backfills* — Existing docs
- Index {_effectiveSecurityClassCodes:1,_countShard:1} — *is an* — Indexes
- Index {_isPublic:1,_countShard:1} — *is an* — Indexes
- Index {_effectiveSecurityClassCodes:1,_countShard:1} — *supports* — Sub-count clause
- Index {_isPublic:1,_countShard:1} — *supports* — Sub-count clause
- Sub-count clause — *uses* — _countShard
- Sub-count clause — *uses* — $gte
- Sub-count clause — *uses* — $lt
- Gateway — *forwards natively* — Sub-count clause
- _countShard — *avoids* — $oid
- _countShard — *avoids* — $expr+$toObjectId
- _countShard — *avoids* — Date coercion
- Hash field — *is preferred over* — _id
- Hash field — *is preferred over* — _idStr
- Plain int — *dodges* — ObjectId range-coercion wall
- Uniform hash — *spreads* — evenly
- Uniform hash — *solves* — Load-skew problem
- Uniform hash — *makes unnecessary* — Quantile boundary sampling
- Deterministic-from-_id — *makes* — Backfill
- Backfill — *is* — Idempotent
- This solution — *does not require* — JsonStore change
- This solution — *requires* — Permission to add field
- This solution — *requires* — Permission to add indexes
- Permission to add field — *is part of* — Mongo schema
- Permission to add indexes — *is part of* — Mongo schema
- Team — *granted* — Permission to add field
- Team — *granted* — Permission to add indexes
- Frozen JsonStore gateway — *makes* — _id-range count fan-out
- _id-range count fan-out — *is a* — dead end
- _id-range count fan-out — *is related to* — BitmapHLL
- MongoDB $expr + $toObjectId — *is correct for* — _id range
- MongoDB $expr + $toObjectId — *does not use* — _id index
- MongoDB $expr + $toObjectId — *causes* — Full scan
- _countShard — *is a* — Hash field
- _id — *is a* — ObjectId
- Sub-count clause — *is* — plain
- Sub-count clause — *uses* — Indexes
- Index-seek goal — *is achieved by* — Indexes
- Buckets — *balance with* — Simple equal int cuts
- Load-skew problem — *disappears* — true
- This solution — *is* — correct
- This solution — *is* — fast
- This solution — *is* — balanced
- Divide-and-conquer count — *uses* — Partitioning
- _countShard — *is a* — partition key
- _id — *is not a good* — partition key
- _countShard — *is a* — uniform int

%% ai-graph-end %%