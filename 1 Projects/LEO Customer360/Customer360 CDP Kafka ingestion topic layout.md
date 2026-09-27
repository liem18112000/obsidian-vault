---
ai_hash: 985f1139ee69b512
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities:
- Customer360 CDP Kafka ingestion topic layout
- Kafka
- Customer360 CDP
- Topics
- Dead Letter Queues (DLQs)
- Per-tenant isolation edge
- terraform/ Kafka module
- cdp.raw-events
- cdp_raw_events
- cdp.raw-profiles
- cdp_raw_profiles_stage
- cdp.raw-events.dlq
- cdp.raw-profiles.dlq
- identity-resolution.md
- Object-storage backup
- DLQ consumers
- vStorage
- ingestion bucket
- backups bucket
- cdp.profile-resolved
- Phase 2 activation edge
- CIR
- Master profile
- Segmentation
- Personalization
- Notification
- Email
- data_synch producer
- <tenant_code>.events
- <tenant_code>-app SASL user
- Router
- Producer key
- tenant_id
- identity-hint
- external_customer_id
- device_id
- cookie_id
- session_id
- hashed-email
- raw_profile_id
- Postgres
- source_system
- event_dedup_key
- LEO Customer360 GreenNode Terraform infrastructure
- vngcloud_vdb_kafka_topic
- retention
- cleanup.policy
- shared infra + tenant_id model
- High-volume behavioral/transactional data
- Source-system + CRM/file imports
- ingestion code
source: session 2026-08-03
status: seedling
tags:
- kafka
- ingestion
- cdp
- multitenancy
- dlq
- customer360
title: Customer360 CDP Kafka ingestion topic layout
type: howto
---

# Customer360 CDP Kafka ingestion topic layout

The Customer360 CDP Kafka ingestion layout is **shared topics keyed by tenant, plus DLQs, plus an opt-in per-tenant isolation edge** — matching the app's "shared infra + tenant_id" model. Provisioned by the `terraform/` Kafka module.

**Topics** (dev / prod partitions, replicas 3):
- `cdp.raw-events` (6/12) → `cdp_raw_events`. High-volume behavioral/transactional.
- `cdp.raw-profiles` (6/12) → `cdp_raw_profiles_stage`. Source-system + CRM/file imports.
- `cdp.raw-events.dlq` (3/6) and `cdp.raw-profiles.dlq` (3/3) — the design spec (`identity-resolution.md`) **requires a DLQ + object-storage backup**; these were the gap in the first draft. DLQ consumers also mirror raw bytes to the vStorage `ingestion`/`backups` bucket.
- `cdp.profile-resolved` — Phase 2 activation edge (emitted after CIR resolves a master profile; consumed by segmentation/personalization/notification/email). Commented out until a `data_synch` producer exists.
- `<tenant_code>.events` — opt-in per-tenant hard-isolation ingestion topic with a scoped `<tenant_code>-app` SASL user; needs a router to fold it back into `cdp_raw_events`.

**Keying decision (the one to get right):** producer key = `"<tenant_id>:<identity-hint>"` where identity-hint = first present of external_customer_id / device_id / cookie_id / session_id / hashed-email. This co-locates a tenant's data *and* keeps one visitor's event timeline ordered within a partition. `raw_profile_id` can't be the key — it's resolved server-side after ingestion. Dedup uniqueness in Postgres is `(tenant_id, source_system, event_dedup_key)`.

**Status:** Kafka is greenfield (no producers/consumers yet); topics are provisioned in advance so ingestion code can point at them later.

## Related
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[vngcloud_vdb_kafka_topic exposes only retention, not cleanup.policy]]

%% ai-graph-start %%

**Related notes:**
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[vngcloud_vdb_kafka_topic exposes only retention, not cleanup.policy]]
- [[LEO CDP has two complementary ingestion lanes converging at CIR]]
- [[Kafka sink append-only log, idempotency via dedupe_key message key]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]

**Relations:**
- Customer360 CDP Kafka ingestion topic layout — *follows* — shared infra + tenant_id model
- Customer360 CDP Kafka ingestion topic layout — *includes* — Topics
- Customer360 CDP Kafka ingestion topic layout — *includes* — Dead Letter Queues (DLQs)
- Customer360 CDP Kafka ingestion topic layout — *includes* — Per-tenant isolation edge
- Topics — *provisioned by* — terraform/ Kafka module
- cdp.raw-events — *is a* — Topics
- cdp.raw-events — *maps to* — cdp_raw_events
- cdp.raw-events — *handles* — High-volume behavioral/transactional data
- cdp.raw-profiles — *is a* — Topics
- cdp.raw-profiles — *maps to* — cdp_raw_profiles_stage
- cdp.raw-profiles — *handles* — Source-system + CRM/file imports
- cdp.raw-events.dlq — *is a* — Dead Letter Queues (DLQs)
- cdp.raw-profiles.dlq — *is a* — Dead Letter Queues (DLQs)
- identity-resolution.md — *requires* — Dead Letter Queues (DLQs)
- identity-resolution.md — *requires* — Object-storage backup
- DLQ consumers — *mirror raw bytes to* — vStorage
- vStorage — *contains* — ingestion bucket
- vStorage — *contains* — backups bucket
- cdp.profile-resolved — *is a* — Topics
- cdp.profile-resolved — *is a* — Phase 2 activation edge
- cdp.profile-resolved — *emitted after* — CIR
- CIR — *resolves* — Master profile
- cdp.profile-resolved — *consumed by* — Segmentation
- cdp.profile-resolved — *consumed by* — Personalization
- cdp.profile-resolved — *consumed by* — Notification
- cdp.profile-resolved — *consumed by* — Email
- cdp.profile-resolved — *requires* — data_synch producer
- <tenant_code>.events — *is a* — Per-tenant isolation edge
- <tenant_code>.events — *uses* — <tenant_code>-app SASL user
- <tenant_code>.events — *needs* — Router
- Router — *folds* — <tenant_code>.events
- <tenant_code>.events — *folds into* — cdp_raw_events
- Producer key — *is composed of* — tenant_id
- Producer key — *is composed of* — identity-hint
- identity-hint — *can be* — external_customer_id
- identity-hint — *can be* — device_id
- identity-hint — *can be* — cookie_id
- identity-hint — *can be* — session_id
- identity-hint — *can be* — hashed-email
- raw_profile_id — *cannot be* — Producer key
- Dedup uniqueness — *stored in* — Postgres
- Dedup uniqueness — *defined by* — tenant_id
- Dedup uniqueness — *defined by* — source_system
- Dedup uniqueness — *defined by* — event_dedup_key
- Kafka — *is* — greenfield
- Topics — *are* — provisioned in advance
- ingestion code — *points at* — Topics
- LEO Customer360 GreenNode Terraform infrastructure — *is related to* — Customer360 CDP Kafka ingestion topic layout
- vngcloud_vdb_kafka_topic — *is related to* — Customer360 CDP Kafka ingestion topic layout
- vngcloud_vdb_kafka_topic — *exposes* — retention
- vngcloud_vdb_kafka_topic — *does not expose* — cleanup.policy
- Customer360 CDP — *uses* — Kafka

%% ai-graph-end %%