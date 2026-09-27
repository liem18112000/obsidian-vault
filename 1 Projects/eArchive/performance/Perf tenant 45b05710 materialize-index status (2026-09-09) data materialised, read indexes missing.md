---
title: "Perf tenant 45b05710 materialize-index status (2026-09-09): data materialised, read indexes missing"
created: 2026-09-09
type: observation
status: seedling
source: "session 2026-09-09"
tags: [earchive, materialize, performance, mongodb, index]
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
