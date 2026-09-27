---
ai_hash: 5ef766cbe3b44b19
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities:
- Customer360 GreenNode
- HCM03
- HCM04
- Compute
- vDB
- Network
- vStorage
- vServer
- PostgreSQL
- Redis
- Kafka
- VPC
- Console host
- hcm-3.console.greennode.ai
- HCM03-1A
- Object storage
- demo-leocdp
- hcm04.vstorage.vngcloud.vn
- S3
- Latency
- Egress cost
- High-throughput ingestion backup
- Kafka DLQ mirroring
- Production
- terraform/.env
- zone_id
- vng_vserver_base_url
- vng_vlb_base_url
- vstorage_region
- vstorage_s3_endpoint
- vng_vdb_base_url
- LEO Customer360 GreenNode Terraform infrastructure
- VNG Cloud resource ID prefixes and the HCM zone_id label gotcha
- Confirm an S3-compatible object store region with a signed curl ListBuckets
- VNG Cloud
source: session 2026-08-03
status: seedling
tags:
- greennode
- region
- hcm03
- hcm04
- vstorage
- customer360
title: 'Customer360 GreenNode region split: compute HCM03, vStorage HCM04'
type: observation
---

# Customer360 GreenNode region split: compute HCM03, vStorage HCM04

The Customer360 GreenNode deployment is **cross-region**:

- **Compute + vDB + network → HCM03** — vServer, PostgreSQL/Redis/Kafka, VPC. Console host `hcm-3.console.greennode.ai`, zone `HCM03-1A`.
- **vStorage → HCM04** — object storage, bucket `demo-leocdp`, endpoint `hcm04.vstorage.vngcloud.vn` (confirmed via a signed ListBuckets: HCM04 → 200, HCM03 → 403).

This is valid because S3 buckets are reached by endpoint, not by VPC. But cross-region S3 adds latency and possible egress cost — for high-throughput ingestion backup / Kafka DLQ mirroring in production, consider creating a vStorage bucket in HCM03 to co-locate with compute.

In `terraform/.env`: `zone_id` + `vng_vserver_base_url` + `vng_vlb_base_url` point at HCM03; `vstorage_region` + `vstorage_s3_endpoint` point at HCM04; `vng_vdb_base_url` is region-agnostic.

## Related
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[VNG Cloud resource ID prefixes and the HCM zone_id label gotcha]]
- [[Confirm an S3-compatible object store region with a signed curl ListBuckets]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud resource ID prefixes and the HCM zone_id label gotcha]]
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[AppsFlyer connector S3 config is vStorage-only - VSTORAGE_ env vars]]

**Relations:**
- Customer360 GreenNode — *has deployment type* — cross-region
- Customer360 GreenNode — *deploys compute in* — HCM03
- Customer360 GreenNode — *deploys vDB in* — HCM03
- Customer360 GreenNode — *deploys network in* — HCM03
- Customer360 GreenNode — *deploys vStorage in* — HCM04
- HCM03 — *hosts* — Compute
- HCM03 — *hosts* — vDB
- HCM03 — *hosts* — Network
- HCM03 — *includes* — vServer
- HCM03 — *includes* — PostgreSQL
- HCM03 — *includes* — Redis
- HCM03 — *includes* — Kafka
- HCM03 — *includes* — VPC
- HCM03 — *has console host* — hcm-3.console.greennode.ai
- HCM03 — *has zone* — HCM03-1A
- HCM04 — *hosts* — vStorage
- vStorage — *is a type of* — Object storage
- HCM04 — *has bucket* — demo-leocdp
- HCM04 — *has endpoint* — hcm04.vstorage.vngcloud.vn
- S3 — *buckets are reached by* — endpoint
- cross-region S3 — *adds* — Latency
- cross-region S3 — *adds* — Egress cost
- vStorage bucket in HCM03 — *considered for* — High-throughput ingestion backup
- vStorage bucket in HCM03 — *considered for* — Kafka DLQ mirroring
- Kafka DLQ mirroring — *occurs in* — Production
- terraform/.env — *configures* — zone_id
- terraform/.env — *configures* — vng_vserver_base_url
- terraform/.env — *configures* — vng_vlb_base_url
- terraform/.env — *configures* — vstorage_region
- terraform/.env — *configures* — vstorage_s3_endpoint
- terraform/.env — *configures* — vng_vdb_base_url
- zone_id — *points to* — HCM03
- vng_vserver_base_url — *points to* — HCM03
- vng_vlb_base_url — *points to* — HCM03
- vstorage_region — *points to* — HCM04
- vstorage_s3_endpoint — *points to* — HCM04
- vng_vdb_base_url — *is* — region-agnostic
- Customer360 GreenNode — *is related to* — LEO Customer360 GreenNode Terraform infrastructure
- VNG Cloud — *is related to* — VNG Cloud resource ID prefixes and the HCM zone_id label gotcha
- S3 — *is related to* — Confirm an S3-compatible object store region with a signed curl ListBuckets
- vStorage — *is* — S3-compatible object store

%% ai-graph-end %%