---
ai_hash: c0c2bc03cf80550b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- GreenNode vDB
- GreenNode vStorage
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- greennode
- vngcloud
- cloud
- dbaas
title: GreenNode is VNG Cloud's AI cloud exposing vDB and vStorage managed services
type: concept
---

# GreenNode is VNG Cloud's AI cloud exposing vDB and vStorage managed services

GreenNode is the AI-cloud brand of **VNG Cloud** (a Vietnamese provider). Its managed data services are exposed as two product families:

- **vDB** — managed databases: PostgreSQL, Redis (branded "Memory store"), and Kafka.
- **vStorage** — object storage, accessible via both an **S3-compatible** API and OpenStack **Swift**; regional endpoints look like `https://hcm03.vstorage.vngcloud.vn`.

Because GreenNode and VNG Cloud are the same platform, documentation is mirrored across `docs.greennode.ai` and `docs.vngcloud.vn` (docs.vngcloud.vn URLs 307-redirect to docs.greennode.ai). The IAM/console lives at `*.console.greennode.ai`.

## Related
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]

%% ai-graph-start %%

**Related notes:**
- [[GreenNode Cloud is VNG Cloud rebranded (same IAM, gateway, Terraform provider)]]
- [[GreenNode cloud runs on VNG Cloud infrastructure]]
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]
- [[VNG Cloud Terraform provider maps managed Postgres and Redis to vdb resources]]

%% ai-graph-end %%