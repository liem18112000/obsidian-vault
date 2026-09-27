---
title: "Read-only materialize-index readiness check via hello + listIndexes"
created: 2026-09-09
type: howto
status: seedling
source: "session 2026-09-09"
tags: [earchive, materialize, mongodb, kubectl, gotcha, index]
---

# Read-only materialize-index readiness check via hello + listIndexes

To check whether a tenant is ready for the eArchive materialise run **without side effects**, do a read-only probe instead of running the `earchive-materialize-index` skill (which uses an insert+drop write-probe to find the primary and then *creates* missing indexes).

**How:** connect per replica with `directConnection=true` and run `db.command({ hello: 1 })`. `hello.isWritablePrimary` distinguishes primary vs secondary — pure read, so loop `rs-0/1/2` until you land on the primary. Then read state with `coll.indexes()` (names + key shapes + partialFilter) and `db.command({ collStats: "documents" })` (per-index `indexSizes` + `count`). Compare index key shapes against the 6 targets to get present/missing.

**Gotcha 1 — no `currentOp`:** the per-tenant mongo user (user=pass=authSource=`tenantId`) is **not authorized** for `admin.currentOp`, so you cannot use it to detect in-progress background index builds. You do not need it: a live background build **does** appear in `listIndexes`, so a target index simply *absent* from `listIndexes` = build not started. Absence is a sufficient "not ready" signal.

**Gotcha 2 — port mapping:** the skill port-forwards `${PORT}:${PORT}` with `PORT` defaulting to `27017`. If you write your own forward and override the *local* port, you must still map to pod **27017** (`localPort:27017`) — mongod listens on 27017 inside the pod regardless of your local port. Map `27099:27099` and kubectl will accept the local TCP connection (so a `/dev/tcp` readiness probe *passes*) but the mongo handshake fails with `ECONNREFUSED`, which looks like a dead primary.

Reusable across [[performance-tenant-clusters]] and [[dev-tenant-clusters]] tenants.

## Related

- [[performance-tenant-clusters]]
- [[dev-tenant-clusters]]
