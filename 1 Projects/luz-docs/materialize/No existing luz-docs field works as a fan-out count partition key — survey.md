---
ai_hash: a7282059b86b891c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-16
entities:
- luz-docs
- fan-out count partition key
- LUZ-154613
- _id
- ObjectId
- gateway
- _createdDate
- _updatedDate
- BSON Date
- JsonStoreSearchQueryUtil.buildDateFromStringQuery
- _versionNumber
- _isPublic
- _createdBy
- _updatedBy
- name
- _sizeInBytes
- dedicated stored field
- _countShard
- SHARD_SPACE
- Partition the materialized count on a uniform _countShard int, not _id
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index
  (full scan)
- $expr
- $toObjectId
- $dateFromString
- scan
- undercount
- cardinality/spread
- plain scalar
- present on 100% of materialised docs
- native indexed range
- balance K buckets
- correct+fast+balanced fan-out
- range query
source: LUZ-154613 session 2026-06-16
status: seedling
tags:
- luz-docs
- materialize
- mongodb
- partition
- design
title: No existing luz-docs field works as a fan-out count partition key — survey
type: argument
---

# No existing luz-docs field works as a fan-out count partition key — survey

Survey of existing luz-docs document fields as a fan-out count partition key (LUZ-154613). Requirements: (1) present on 100% of materialised docs (else silent undercount), (2) plain scalar so the gateway does a NATIVE indexed range — typed fields force coercion ($toObjectId / $dateFromString) inside $expr which full-scans, (3) enough cardinality/spread to balance K buckets.

Result — NO existing field satisfies all three:
- _id: always present, but ObjectId → gateway coerces only for eq/$in; range needs $expr+$toObjectId → scan.
- _createdDate/_updatedDate: always present, high cardinality, but BSON Date → string bounds need $dateFromString (the date analogue of $toObjectId; see JsonStoreSearchQueryUtil.buildDateFromStringQuery) → $expr → scan. Also time-skewed.
- _versionNumber: plain int, always present, but ~all docs = 1 → degenerate (one bucket).
- _isPublic: bool (2 values); _createdBy/_updatedBy: few distinct users/tenant → no spread.
- name / _sizeInBytes: plain + good spread BUT not guaranteed on every doc → undercount.

Therefore a dedicated stored field is required for a correct+fast+balanced fan-out: _countShard = uniform int in [0, SHARD_SPACE), native indexed range, equal cuts balance with no quantile. This is the justification for adding a field rather than reusing one.

## Related

- [[Partition the materialized count on a uniform _countShard int, not _id]]
- [[MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan)]]

%% ai-graph-start %%

**Related notes:**
- [[Partition the materialized count on a uniform _countShard int, not _id]]
- [[Fan-out count needs an explicit key-absent sub-count to stay exact during shard backfill]]
- [[luz-docs parallelized count undercounts documents missing _shard]]
- [[Random shard key gives balanced fan-out partitions (equal-width = equal-work only if uniform)]]
- [[_shard fan-out uses idx_shard (IXSCAN exact slice); local port-forward masks the speedup]]

**Relations:**
- luz-docs — *needs* — fan-out count partition key
- LUZ-154613 — *is a* — survey
- survey — *evaluates fields for* — fan-out count partition key
- fan-out count partition key — *requires* — present on 100% of materialised docs
- fan-out count partition key — *requires* — plain scalar
- fan-out count partition key — *requires* — cardinality/spread
- _id — *is a field of* — luz-docs
- _id — *is type* — ObjectId
- _id — *is* — present on 100% of materialised docs
- ObjectId — *requires* — $expr
- ObjectId — *requires* — $toObjectId
- $expr — *causes* — scan
- $toObjectId — *causes* — scan
- _createdDate — *is a field of* — luz-docs
- _createdDate — *is type* — BSON Date
- _createdDate — *is* — present on 100% of materialised docs
- _updatedDate — *is a field of* — luz-docs
- _updatedDate — *is type* — BSON Date
- _updatedDate — *is* — present on 100% of materialised docs
- BSON Date — *requires* — $expr
- BSON Date — *requires* — $dateFromString
- $dateFromString — *causes* — scan
- JsonStoreSearchQueryUtil.buildDateFromStringQuery — *uses* — $dateFromString
- _versionNumber — *is a field of* — luz-docs
- _versionNumber — *is type* — plain scalar
- _versionNumber — *is* — present on 100% of materialised docs
- _versionNumber — *lacks* — cardinality/spread
- _isPublic — *is a field of* — luz-docs
- _isPublic — *lacks* — cardinality/spread
- _createdBy — *is a field of* — luz-docs
- _createdBy — *lacks* — cardinality/spread
- _updatedBy — *is a field of* — luz-docs
- _updatedBy — *lacks* — cardinality/spread
- name — *is a field of* — luz-docs
- name — *lacks* — present on 100% of materialised docs
- name — *causes* — undercount
- _sizeInBytes — *is a field of* — luz-docs
- _sizeInBytes — *lacks* — present on 100% of materialised docs
- _sizeInBytes — *causes* — undercount
- dedicated stored field — *is required for* — correct+fast+balanced fan-out
- _countShard — *is a* — dedicated stored field
- _countShard — *is type* — plain scalar
- _countShard — *has range* — [0, SHARD_SPACE)
- _countShard — *enables* — native indexed range
- _countShard — *enables* — balance K buckets
- Partition the materialized count on a uniform _countShard int, not _id — *justifies* — _countShard
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan) — *explains issue with* — _id
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan) — *explains issue with* — $expr
- MongoDB $expr + $toObjectId for _id range is correct but does not use the _id index (full scan) — *explains issue with* — $toObjectId
- range query — *uses* — $expr
- range query — *uses* — $toObjectId
- range query — *uses* — $dateFromString

%% ai-graph-end %%