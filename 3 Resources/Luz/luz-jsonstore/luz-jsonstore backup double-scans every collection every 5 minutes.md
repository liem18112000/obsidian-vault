---
title: "luz-jsonstore backup double-scans every collection every 5 minutes"
created: 2026-09-09
type: lesson
status: seedling
source: "PROD investigation 2026-09-09"
tags: [luz-jsonstore, mongodb, backup, performance, gotcha]
---

# luz-jsonstore backup double-scans every collection every 5 minutes

luz-jsonstores backup path is a chronic full-scan load on **every** tenant MongoDB and has a connection leak — it does not start the morning outages but removes the headroom the mongods need to survive spikes.

Facts (against `master`, JsonStoreMongoDbService.java):
- A dedicated `luz-jsonstore-backup` deployment runs `OpsService.triggerBackup` on a **5-minute cron** (confirmed in PROD: `"triggerBackup starting ..."` every 5 min all day).
- For each queued (tenant, collection), `getBackupAsZipFname` (:1057) does a **double full-collection scan**: `collection.countDocuments()` (:1102, itself a full COLLSCAN in WiredTiger — unlike `estimatedDocumentCount()`) **then** `collection.find().cursor()` (:1104) streaming every doc to an encrypted zip. Code comment: "300k records produce about 1.2GB".
- `writeOpsBackupEntry` (:1007) fires on **every write** (add/update/delete) when `CH_KLARA_JSONSTORE_ENABLE_BACKUP=true`; it calls `getOpsClient(...)` which opens a new `MongoClient` and **never closes it** → connection + native-memory leak under write bursts. (Throttled to once/15 min per tenant+collection via an in-memory per-pod cache, so the leak is bounded per key but still grows with tenant×collection churn.)

Cheap fixes: use `estimatedDocumentCount()` not `countDocuments()`; drop the redundant count; lengthen the interval; read from secondaries; close/cache the ops client.

## Related
[[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]

## Related

- [[luz-docs enrichment re-run campaign saturates PROD MongoDBs each morning]]
