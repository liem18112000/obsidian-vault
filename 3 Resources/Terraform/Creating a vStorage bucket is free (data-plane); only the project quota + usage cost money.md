---
ai_hash: 2716f0f1115a05e4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18
status: seedling
tags:
- vngcloud
- vstorage
- s3
- billing
- object-storage
- terraform
title: Creating a vStorage bucket is free (data-plane); only the project quota + usage
  cost money
type: lesson
---

# Creating a vStorage bucket is free (data-plane); only the project quota + usage cost money

In VNG vStorage (and S3-compatible object storage generally), **creating a bucket is a free data-plane operation** — it does NOT place a billing order or charge anything. This is fundamentally different from creating the vStorage **project**, which places a paid prepaid order (and, per the code-114 bug, charged even on failure).

What you actually pay for:
- the **project quota** (prepaid, chosen at project creation in the console) — already paid before any Terraform runs;
- **storage used** (per TB of data actually stored) and **bandwidth** (per GB) — accrue with usage.

Therefore `terraform apply` on the leo-customer360 storage module (which only creates `aws_s3_bucket` [+ optional versioning] via the S3 PutBucket API) creates **empty buckets = 0 bytes = 0 incremental cost**. The module's `estimated_*` cost variables are display-only outputs that provision nothing.

Rule of thumb: with S3-compatible clouds, bucket/object API calls are data-plane (no order); only the capacity/subscription purchase is a billing order. Related: [[vStorage create-project code 114 is account-side, not a payload bug]], [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]].

## Related

- [[vStorage create-project code 114 is account-side, not a payload bug]]

%% ai-graph-start %%

**Related notes:**
- [[vStorage project is a paid prerequisite Terraform cannot create]]
- [[Deleting a prepaid cloud resource does not auto-refund; failed-provision charges need a manual support refund]]
- [[VNG Cloud vStorage is S3-compatible object storage]]
- [[vStorage create-project code 114 is account-side, not a payload bug]]
- [[vStorage API project creation needs a billing order (payment method or POC wallet)]]

%% ai-graph-end %%