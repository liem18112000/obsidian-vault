---
ai_hash: d1b143698e0ea7c6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities:
- Performance tenant 45b05710-b9d4-4d3e-935e-83c4525369fa
- materialize-index
- '2026-09-09'
- data materialisation
- read-path (sort) indexes
- earchive-materialize-index state
- count/shard fan-out indexes
- tendata data
- materialise read path
- luz-mongodb04-cluster-rs
- performance-mongodb-clusters
- cluster 04
- rs-2
- documents
- 2,206,000
- target indexes
- idx_shard
- _shard
- idx_isPublic_shard
- _isPublic
- idx_effectiveSecurityClassCodes_shard
- codes
- idx_isPublic_updatedDate
- _updatedDate
- idx_effectiveSecurityClassCodes_updatedDate
- idx_folderNames
- _folderNames
- _id_
- idx_updatedDate
- _effectiveSecurityClassCodes
- skill
- Read-only materialize-index readiness check via hello + listIndexes
- performance-tenant-clusters
- Verdict
- NOT ready
source: session 2026-09-09
status: seedling
tags:
- earchive
- materialize
- performance
- mongodb
- index
title: 'Perf tenant 45b05710 materialize-index status (2026-09-09): data materialised,
  read indexes missing'
type: observation
---

# Perf tenant 45b05710 materialize-index status (2026-09-09): data materialised, read indexes missing

Snapshot of the earchive-materialize-index state for Performance tenant `45b05710-b9d4-4d3e-935e-83c4525369fa` as of 2026-09-09: the count/shard fan-out indexes are built, but the read-path (sort) indexes are **not** — so the tendata data is materialised while the materialise read path is not yet index-backed.

**Where:** cluster `luz-mongodb04-cluster-rs` (ns `performance-mongodb-clusters`, first hex char `4` → cluster 04), primary was `rs-2`. `documents` count = **2,206,000**.

**Of the 6 target indexes:**
- BUILT (count fan-out trio): `idx_shard {_shard:1}` 56 MB · `idx_isPublic_shard {_isPublic:1,_shard:1}` 12 KB partial · `idx_effectiveSecurityClassCodes_shard {codes:1,_shard:1}` **298 MB**.
- MISSING (read/sort trio): `idx_isPublic_updatedDate {_isPublic:1,_updatedDate:-1}` · `idx_effectiveSecurityClassCodes_updatedDate {codes:1,_updatedDate:-1}` · `idx_folderNames {_folderNames:1}`.
- Also present (non-target): `_id_`, `idx_updatedDate {_updatedDate:-1}` 106 MB.

**Read:** the 298 MB `_effectiveSecurityClassCodes_shard` index proves the `_effectiveSecurityClassCodes` + `_shard` fields are already populated → **data materialisation is done**; only the read-path indexes need creating. Verdict at check time: **NOT ready**. Fix = run the skill in default mode (it skips the present 3, creates the missing 3, `background:true`) — heavy on 2.2M docs.

See [[Read-only materialize-index readiness check via hello + listIndexes]] for how this was checked.

## Related

- [[performance-tenant-clusters]]
- [[Read-only materialize-index readiness check via hello + listIndexes]]

%% ai-graph-start %%

**Related notes:**
- [[Read-only materialize-index readiness check via hello + listIndexes]]
- [[eArchive documents collection has 7 materialise-related indexes, not 4]]
- [[Performance-env mongo cluster for a tenant = luz-mongodbNN by first hex char]]
- [[dev-staging luz-docs IT failures cluster on the materialize read-path]]
- [[Luz eArchive tenant mongo database collection list]]

**Relations:**
- Performance tenant 45b05710-b9d4-4d3e-935e-83c4525369fa — *has status for* — materialize-index
- materialize-index — *status date* — 2026-09-09
- materialize-index — *status includes* — data materialisation
- materialize-index — *status includes* — read-path (sort) indexes missing
- earchive-materialize-index state — *is for* — Performance tenant 45b05710-b9d4-4d3e-935e-83c4525369fa
- count/shard fan-out indexes — *are* — built
- read-path (sort) indexes — *are* — not built
- tendata data — *is* — materialised
- materialise read path — *is* — not yet index-backed
- luz-mongodb04-cluster-rs — *is in namespace* — performance-mongodb-clusters
- luz-mongodb04-cluster-rs — *is identified as* — cluster 04
- rs-2 — *was primary of* — luz-mongodb04-cluster-rs
- documents — *count is* — 2,206,000
- target indexes — *total count* — 6
- idx_shard — *is a* — count/shard fan-out indexes
- idx_isPublic_shard — *is a* — count/shard fan-out indexes
- idx_effectiveSecurityClassCodes_shard — *is a* — count/shard fan-out indexes
- idx_isPublic_updatedDate — *is a* — read-path (sort) indexes
- idx_effectiveSecurityClassCodes_updatedDate — *is a* — read-path (sort) indexes
- idx_folderNames — *is a* — read-path (sort) indexes
- idx_shard — *status* — BUILT
- idx_isPublic_shard — *status* — BUILT
- idx_effectiveSecurityClassCodes_shard — *status* — BUILT
- idx_isPublic_updatedDate — *status* — MISSING
- idx_effectiveSecurityClassCodes_updatedDate — *status* — MISSING
- idx_folderNames — *status* — MISSING
- idx_shard — *uses field* — _shard
- idx_isPublic_shard — *uses field* — _isPublic
- idx_isPublic_shard — *uses field* — _shard
- idx_effectiveSecurityClassCodes_shard — *uses field* — codes
- idx_effectiveSecurityClassCodes_shard — *uses field* — _shard
- idx_isPublic_updatedDate — *uses field* — _isPublic
- idx_isPublic_updatedDate — *uses field* — _updatedDate
- idx_effectiveSecurityClassCodes_updatedDate — *uses field* — codes
- idx_effectiveSecurityClassCodes_updatedDate — *uses field* — _updatedDate
- idx_folderNames — *uses field* — _folderNames
- idx_shard — *size* — 56 MB
- idx_isPublic_shard — *size* — 12 KB
- idx_effectiveSecurityClassCodes_shard — *size* — 298 MB
- idx_updatedDate — *size* — 106 MB
- idx_effectiveSecurityClassCodes_shard — *proves* — data materialisation is done
- read-path (sort) indexes — *need* — creating
- Verdict — *is* — NOT ready
- Fix — *is to run* — skill
- skill — *skips* — idx_shard
- skill — *skips* — idx_isPublic_shard
- skill — *skips* — idx_effectiveSecurityClassCodes_shard
- skill — *creates* — idx_isPublic_updatedDate
- skill — *creates* — idx_effectiveSecurityClassCodes_updatedDate
- skill — *creates* — idx_folderNames
- skill — *mode* — default
- skill — *setting* — background:true
- skill — *has impact* — heavy on 2.2M docs
- materialize-index — *status checked via* — Read-only materialize-index readiness check via hello + listIndexes
- performance-tenant-clusters — *is related to* — materialize-index
- Read-only materialize-index readiness check via hello + listIndexes — *is related to* — materialize-index

%% ai-graph-end %%