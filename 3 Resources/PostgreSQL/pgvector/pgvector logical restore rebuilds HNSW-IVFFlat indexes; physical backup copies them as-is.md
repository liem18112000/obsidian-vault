---
ai_hash: e990777a013ec42f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-16
entities: []
source: session 2026-08-16 postgres/docs research
status: seedling
tags:
- pgvector
- postgres
- backup
- restore
- gotcha
title: pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies
  them as-is
type: lesson
---

# pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is

A **logical** `pg_dump` stores vector column *data* fine, but does **not** store HNSW/IVFFlat index contents — it emits the `CREATE INDEX ... USING hnsw/ivfflat` DDL and the index is **rebuilt from scratch on restore**. For a large embedding table the rebuild can take far longer than loading the rows, so it dominates RTO.

Cut the rebuild cost by raising, for the restore session, `maintenance_work_mem` (build is much faster when the graph fits in it) and `max_parallel_maintenance_workers` (parallel HNSW builds since pgvector 0.6.0). In containers, `--shm-size` / `/dev/shm` must be **≥ maintenance_work_mem** or the parallel build errors out. IVFFlat must be built **after** data load (it clusters existing rows); HNSW has no such dependency — `pg_dump` ordering (data then indexes) is already correct.

A **physical** backup (pg_basebackup / pgBackRest / base+WAL / PITR) copies the index files **as-is** — no rebuild — so it is the better track for large vector tables. Either way the restore target must have the pgvector binary present at a version ≥ source.

Related: [[postgis-postgis 16-3.5 image does not bundle pgvector]], [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]].

## Related

- [[postgis-postgis 16-3.5 image does not bundle pgvector]]

%% ai-graph-start %%

**Related notes:**
- [[postgis-postgis 16-3.5 image does not bundle pgvector]]
- [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]]
- [[pgBackRest runs on the DB host, so it cannot back up a managed DB like VNG vDB]]
- [[vngcloud_vdb_postgresql_cluster ignores backup_auto (cluster backups go via VNG Backup Center)]]
- [[pgBackRest archive_command needs the binary IN the postgres image; the scheduler is a separate sidecar]]

%% ai-graph-end %%