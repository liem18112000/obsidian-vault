---
ai_hash: c9535cb3ac0eaa9a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18 (live test)
status: seedling
tags:
- s3
- aws-provider
- terraform
- vngcloud
- minio
- gotcha
title: 'S3-compatible CreateBucket InvalidLocationConstraint: set region us-east-1
  to omit LocationConstraint'
type: lesson
---

# S3-compatible CreateBucket InvalidLocationConstraint: set region us-east-1 to omit LocationConstraint

When creating a bucket on an **S3-compatible** store (VNG vStorage, MinIO, Ceph RadosGW, etc.) with the AWS SDK / Terraform `aws_s3_bucket`, a non-`us-east-1` provider region causes:

```
InvalidLocationConstraint: The specified location-constraint is not valid   (HTTP 400 on CreateBucket)
```

**Why:** the AWS SDK sends `CreateBucketConfiguration/LocationConstraint = <region>` for every region EXCEPT `us-east-1` (its legacy default), for which it OMITS the constraint entirely. S3-compatible backends only accept their own zonegroup name (or an empty constraint), so a value like `hcm04` is rejected.

**Fix:** set the provider **`region = "us-east-1"`** so no LocationConstraint is sent. The real region is chosen by the **endpoint host** (e.g. `hcm04.vstorage.vngcloud.vn`), not by the signing region — vStorage accepts the us-east-1 SigV4 signing label fine. Verified live: with region=us-east-1 a CreateBucket against the hcm04 endpoint succeeds; with region=hcm04 it 400s.

Applies alongside the other S3-compat provider flags: `s3_use_path_style=true`, `skip_credentials_validation`, `skip_region_validation`, `skip_requesting_account_id`, `skip_metadata_api_check`, and a custom `endpoints{ s3 = ... }`.

Related: [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]], [[Creating a vStorage bucket is free (data-plane); only the project quota + usage cost money]].

## Related

- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]

%% ai-graph-start %%

**Related notes:**
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]
- [[Confirm an S3-compatible object store region with a signed curl ListBuckets]]
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]

%% ai-graph-end %%