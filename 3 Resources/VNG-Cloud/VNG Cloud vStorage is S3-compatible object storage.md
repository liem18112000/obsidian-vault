---
ai_hash: 5be9379961a5874f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-25
entities: []
source: LEOCDP web-tracking plan, session 2026-08-25
status: seedling
tags:
- vng-cloud
- vstorage
- s3
- terraform
- object-storage
- gotcha
title: VNG Cloud vStorage is S3-compatible object storage
type: howto
---

# VNG Cloud vStorage is S3-compatible object storage

VNG Cloud **vStorage** is S3-compatible object storage. Because the native `vngcloud/vngcloud` Terraform provider does **not** expose object storage (only vServer, vDB, vLB, vKS), you manage vStorage buckets with the standard `hashicorp/aws` provider pointed at the vStorage S3 endpoint.

**Why it's tricky / gotchas:**
- Turn ON path-style addressing (`s3_use_path_style = true`).
- Switch OFF every AWS-only preflight the provider does by default: STS, IMDS, account-id lookup, and the region allow-list (the `skip_*` flags). vStorage doesn't implement those APIs.
- The vStorage **PROJECT** (region + storage quota/package + billing period) is a **manual console prerequisite** — Terraform can't create it (project creation is a billing action outside the S3 API). Terraform only manages buckets *inside* the project, authenticated by an S3 key created in that project.
- vStorage also doesn't implement AWS sub-resources like public-access-block or ownership controls; omit them. Bucket versioning IS supported (PutBucketVersioning).

**Rate card (quoted):** storage 1 TB ≈ 1,000,000 VND/month; bandwidth 1 GB ≈ 580 VND.

**Dev parity:** in the LEOCDP repo, dev uses **MinIO** with the identical S3 API, so the same boto3/minio code works in both dev and prod by only swapping the endpoint + credentials. See `deployments/storage` (Terraform) and `all-data-simulator/s3_data_util.py`.

Related: [[Lightweight-but-scalable web event collector pattern]] uses vStorage as its raw event lake.

## Related

- [[Lightweight-but-scalable web event collector pattern]]

%% ai-graph-start %%

**Related notes:**
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]
- [[VNG Cloud vStorage is a drop-in S3 backend for path-style S3 clients (MinIO swap)]]
- [[vStorage project is a paid prerequisite Terraform cannot create]]
- [[VNG Cloud IaC = Terraform provider (no first-party CLI); vStorageregistry via S3+docker]]

%% ai-graph-end %%