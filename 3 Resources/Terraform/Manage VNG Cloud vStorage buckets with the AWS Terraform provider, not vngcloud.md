---
ai_hash: 835cadc5d5e0e840
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- terraform
- vngcloud
- vstorage
- s3
- object-storage
- gotcha
title: Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud
type: lesson
---

# Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud

VNG Cloud **vStorage** object storage speaks the **S3 API**, and the native `vngcloud/vngcloud` Terraform provider does **not** expose it (that provider only covers vServer, vDB, vLB, vKS). So manage vStorage buckets with the standard `hashicorp/aws` provider aimed at the vStorage endpoint.

Provider block essentials:
- `endpoints { s3 = "https://<region>.vstorage.vngcloud.vn" }` — region pattern e.g. `hcm01`, `hcm03`, `hcm04`, `han01`.
- `s3_use_path_style = true` (S3-compatible stores use path-style `endpoint/bucket`, not virtual-host).
- Turn OFF every AWS-only preflight, since this isn't real AWS: `skip_credentials_validation`, `skip_metadata_api_check`, `skip_region_validation`, `skip_requesting_account_id` all `true`. `region` becomes a signature label only.

Gotcha: AWS-only bucket sub-resources (`aws_s3_bucket_public_access_block`, ownership controls) are **not implemented** by vStorage — omit them. Object **versioning** (`aws_s3_bucket_versioning` / PutBucketVersioning) **is** supported.

Same pattern applies to any S3-compatible store (MinIO, OVH, Backblaze B2). Used in `leo-customer360` `deployments/storage`, mirroring `deployments/postgres`.

See [[vStorage S3 keys differ from vIAM client credentials used by vDB]].

## Related

- [[vStorage S3 keys differ from vIAM client credentials used by vDB]]

%% ai-graph-start %%

**Related notes:**
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]
- [[VNG Cloud vStorage is S3-compatible object storage]]
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]
- [[vStorage S3 keys differ from vIAM client credentials used by vDB]]
- [[VNG Cloud IaC = Terraform provider (no first-party CLI); vStorageregistry via S3+docker]]

%% ai-graph-end %%