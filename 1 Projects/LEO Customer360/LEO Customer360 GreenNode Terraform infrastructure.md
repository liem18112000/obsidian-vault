---
ai_hash: c09de733fc444472
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities:
- LEO Customer360 GreenNode Terraform infrastructure
- Terraform
- leo-customer360/terraform/
- Customer360 CDP
- GreenNode
- VNG Cloud
- PostgreSQL
- Redis
- Kafka
- vStorage
- S3
- infras/setup-redis-vstorage-kafka-postgressql
- Hybrid multi-tenancy
- shared cluster
- shared Kafka topics
- per-tenant topics
- SASL users
- shared vStorage buckets
- per-tenant buckets
- tenant_id
- app
- Postgres RLS
- tenants
- per-tenant infra
- Per-environment PostgreSQL topology
- standalone single node
- dev environment
- HA cluster
- prod environment
- topology variable
- Networking
- VNG network
- subnet
- create_network
- modules
- network module
- postgres module
- redis module
- kafka module
- vstorage module
- db-bootstrap module
- stack/
- environments
- Idempotency
- in-database bootstrap
- extensions
- role
- schema
- seed
- VNG Cloud Terraform provider
- vDB service-to-resource mapping
- Terraform resource
- AWS S3 provider
- buckets
- superusers
- non-superuser role
- vDB PostgreSQL
- PostGIS
- pgvector
- fuzzy-match extensions
source: session 2026-08-03
status: seedling
tags:
- terraform
- greennode
- vngcloud
- customer360
- multitenancy
title: LEO Customer360 GreenNode Terraform infrastructure
type: howto
---

# LEO Customer360 GreenNode Terraform infrastructure

A Terraform project under `leo-customer360/terraform/` provisions the four managed data services the Customer360 CDP needs on **GreenNode (VNG Cloud)**: PostgreSQL, Redis, Kafka, and vStorage (S3). Branch: `infras/setup-redis-vstorage-kafka-postgressql`.

**Design decisions made:**
- **Hybrid multi-tenancy** — one shared cluster per service, with a per-tenant edge: shared Kafka topics keyed by `tenant_id` **plus** optional per-tenant topics + scoped SASL users, and shared vStorage buckets **plus** per-tenant buckets. Matches the app, which isolates tenants in-data (Postgres RLS), not per-tenant infra.
- **Per-environment PostgreSQL topology** — standalone single node in `dev`, HA cluster in `prod`, via a `topology` variable.
- **Networking supports both** — reference an existing VNG network/subnet by default, or create one behind `create_network`.

**Layout:** `modules/{network,postgres,redis,kafka,vstorage,db-bootstrap}` -> `stack/` composition -> `environments/{dev,prod}`. Idempotency is native to Terraform; in-database bootstrap (extensions/role/schema/seed) is a separate idempotent step in `db-bootstrap`.

## Related
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]
- [[Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser role]]
- [[vDB PostgreSQL supports PostGIS and pgvector plus the fuzzy-match extensions]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud Terraform provider maps managed Postgres and Redis to vdb resources]]
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[Customer360 GreenNode region split compute HCM03, vStorage HCM04]]
- [[Customer360 Kubernetes deployment (local kind + GreenNode VKS)]]
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]

**Relations:**
- LEO Customer360 GreenNode Terraform infrastructure — *is a Terraform project* — Terraform
- LEO Customer360 GreenNode Terraform infrastructure — *located under* — leo-customer360/terraform/
- LEO Customer360 GreenNode Terraform infrastructure — *provisions* — PostgreSQL
- LEO Customer360 GreenNode Terraform infrastructure — *provisions* — Redis
- LEO Customer360 GreenNode Terraform infrastructure — *provisions* — Kafka
- LEO Customer360 GreenNode Terraform infrastructure — *provisions* — vStorage
- Customer360 CDP — *needs* — PostgreSQL
- Customer360 CDP — *needs* — Redis
- Customer360 CDP — *needs* — Kafka
- Customer360 CDP — *needs* — vStorage
- PostgreSQL — *deployed on* — GreenNode
- Redis — *deployed on* — GreenNode
- Kafka — *deployed on* — GreenNode
- vStorage — *deployed on* — GreenNode
- GreenNode — *is* — VNG Cloud
- vStorage — *is* — S3
- LEO Customer360 GreenNode Terraform infrastructure — *uses branch* — infras/setup-redis-vstorage-kafka-postgressql
- LEO Customer360 GreenNode Terraform infrastructure — *implements* — Hybrid multi-tenancy
- Hybrid multi-tenancy — *uses* — shared cluster
- Hybrid multi-tenancy — *includes* — shared Kafka topics
- Hybrid multi-tenancy — *includes* — per-tenant topics
- per-tenant topics — *uses* — SASL users
- Hybrid multi-tenancy — *includes* — shared vStorage buckets
- Hybrid multi-tenancy — *includes* — per-tenant buckets
- shared Kafka topics — *keyed by* — tenant_id
- Hybrid multi-tenancy — *matches* — app
- app — *isolates tenants using* — Postgres RLS
- app — *does not use* — per-tenant infra
- LEO Customer360 GreenNode Terraform infrastructure — *implements* — Per-environment PostgreSQL topology
- Per-environment PostgreSQL topology — *is standalone single node in* — dev environment
- Per-environment PostgreSQL topology — *is HA cluster in* — prod environment
- Per-environment PostgreSQL topology — *configured via* — topology variable
- LEO Customer360 GreenNode Terraform infrastructure — *includes* — Networking
- Networking — *references* — VNG network
- Networking — *references* — subnet
- Networking — *can create* — VNG network
- Networking — *can create* — subnet
- LEO Customer360 GreenNode Terraform infrastructure — *has layout component* — modules
- modules — *includes* — network module
- modules — *includes* — postgres module
- modules — *includes* — redis module
- modules — *includes* — kafka module
- modules — *includes* — vstorage module
- modules — *includes* — db-bootstrap module
- LEO Customer360 GreenNode Terraform infrastructure — *has layout component* — stack/
- LEO Customer360 GreenNode Terraform infrastructure — *has layout component* — environments
- environments — *includes* — dev environment
- environments — *includes* — prod environment
- Idempotency — *is native to* — Terraform
- in-database bootstrap — *is an idempotent step* — db-bootstrap module
- in-database bootstrap — *includes* — extensions
- in-database bootstrap — *includes* — role
- in-database bootstrap — *includes* — schema
- in-database bootstrap — *includes* — seed
- VNG Cloud Terraform provider — *has* — vDB service-to-resource mapping
- vStorage — *has no* — Terraform resource
- buckets — *managed via* — AWS S3 provider
- Postgres RLS — *bypassed by* — superusers
- app — *needs* — non-superuser role
- vDB PostgreSQL — *supports* — PostGIS
- vDB PostgreSQL — *supports* — pgvector
- vDB PostgreSQL — *supports* — fuzzy-match extensions

%% ai-graph-end %%