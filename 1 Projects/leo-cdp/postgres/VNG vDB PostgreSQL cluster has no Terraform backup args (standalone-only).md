---
ai_hash: f75cad7f79d7ea5f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-16
entities:
- VNG vDB PostgreSQL cluster
- Terraform
- vngcloud_vdb_postgresql_cluster
- backup_auto
- backup_duration
- backup_time
- vngcloud_vdb_relational_database
- backup_policy_id
- backup_location_id
- VNG Backup Center
- backup_id
- LEO CDP Customer360 repo
- prod
- pg_topology
- postgres module variables
- vStorage
- vDB PITR
- pgvector
- HNSW-IVFFlat indexes
- physical backup
- logical restore
- '2024-08-01'
- HA topology
- standalone topology
- Backup Center IDs
- logical pg_dump CronJob
- new instance/cluster
- 2–14 day retention range
source: session 2026-08-16 postgres/docs research
status: seedling
tags:
- vngcloud
- postgres
- backup
- terraform
- gotcha
- leo-cdp
title: VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)
type: lesson
---

# VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)

The VNG Cloud vDB Terraform resource `vngcloud_vdb_postgresql_cluster` (HA topology) exposes **no** `backup_auto` / `backup_duration` / `backup_time` arguments. Those exist **only** on the standalone `vngcloud_vdb_relational_database`. A cluster instead takes `backup_policy_id` + `backup_location_id`, which reference a Backup Policy/Location defined in VNG **Backup Center**, plus `backup_id` to restore-from.

**Why it matters:** in the LEO CDP Customer360 repo, prod uses `pg_topology = "cluster"`, so any `backup_auto`/`backup_duration` set in the postgres module variables silently do nothing for prod. Prod backups must be wired via Backup Center IDs, and supplemented with a logical `pg_dump` CronJob to vStorage.

Also: vDB **PITR is not documented** (snapshot-based only), restore always **creates a new instance/cluster**, and the 2–14 day retention range is the standalone limit (verify cluster policy range in console). pgvector is available on vDB only for instances created ≥ 2024-08-01.

See [[pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is]].

## Related

- [[pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is]]

%% ai-graph-start %%

**Related notes:**
- [[vngcloud_vdb_postgresql_cluster ignores backup_auto (cluster backups go via VNG Backup Center)]]
- [[pgBackRest runs on the DB host, so it cannot back up a managed DB like VNG vDB]]
- [[pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is]]
- [[vDB PostgreSQL supports PostGIS and pgvector plus the fuzzy-match extensions]]
- [[postgis-postgis 16-3.5 image does not bundle pgvector]]

**Relations:**
- vngcloud_vdb_postgresql_cluster — *is_a* — VNG vDB PostgreSQL cluster
- vngcloud_vdb_postgresql_cluster — *has_topology* — HA topology
- vngcloud_vdb_postgresql_cluster — *does_not_expose_arg* — backup_auto
- vngcloud_vdb_postgresql_cluster — *does_not_expose_arg* — backup_duration
- vngcloud_vdb_postgresql_cluster — *does_not_expose_arg* — backup_time
- vngcloud_vdb_relational_database — *has_topology* — standalone topology
- vngcloud_vdb_relational_database — *exposes_arg* — backup_auto
- vngcloud_vdb_relational_database — *exposes_arg* — backup_duration
- vngcloud_vdb_relational_database — *exposes_arg* — backup_time
- VNG vDB PostgreSQL cluster — *requires_arg* — backup_policy_id
- VNG vDB PostgreSQL cluster — *requires_arg* — backup_location_id
- VNG vDB PostgreSQL cluster — *requires_arg_for_restore* — backup_id
- backup_policy_id — *references* — VNG Backup Center
- backup_location_id — *references* — VNG Backup Center
- LEO CDP Customer360 repo — *uses* — prod
- prod — *configures_pg_topology_as* — cluster
- backup_auto — *in_postgres_module_variables_is_ineffective_for* — prod
- backup_duration — *in_postgres_module_variables_is_ineffective_for* — prod
- prod — *manages_backups_via* — Backup Center IDs
- prod — *supplements_backups_with* — logical pg_dump CronJob
- logical pg_dump CronJob — *stores_to* — vStorage
- vDB PITR — *is_status* — not documented
- vDB PITR — *is_type* — snapshot-based only
- restore — *action* — creates new instance/cluster
- standalone topology — *has_retention_limit* — 2–14 day retention range
- pgvector — *available_on_vDB_from* — 2024-08-01
- logical restore — *of_pgvector_rebuilds* — HNSW-IVFFlat indexes
- physical backup — *of_pgvector_copies* — HNSW-IVFFlat indexes
- pgvector logical restore rebuilds HNSW-IVFFlat indexes; physical backup copies them as-is — *is_related_to* — pgvector

%% ai-graph-end %%