---
ai_hash: 7caa6857e62b13be
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- terraform
- vngcloud
- kafka
- compaction
- gotcha
title: vngcloud_vdb_kafka_topic exposes only retention, not cleanup.policy
type: lesson
---

# vngcloud_vdb_kafka_topic exposes only retention, not cleanup.policy

The `vngcloud_vdb_kafka_topic` Terraform resource exposes only `partitions`, `replicas`, `retention_seconds`, and `retention_bytes` — **not** arbitrary topic configs like `cleanup.policy`. So you **cannot enable log compaction on a per-topic basis through the topic resource**.

To get a compacted topic (e.g. a `profile-resolved` change-log keyed by `master_profile_id`), set the compaction config through the **Kafka config group** (`vngcloud_vdb_kafka_config_group.properties`, applied cluster-wide via `config_group_version_id`) or via the vDB console / Kafka admin API after creation. Plan for this when a topic's semantics depend on compaction rather than time/size retention.

## Related
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]

%% ai-graph-start %%

**Related notes:**
- [[Customer360 CDP Kafka ingestion topic layout]]
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[vngcloud_vdb_postgresql_cluster ignores backup_auto (cluster backups go via VNG Backup Center)]]
- [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]]

%% ai-graph-end %%