---
ai_hash: d61c5517aa77e095
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- terraform
- vngcloud
- vstorage
- s3
- gotcha
title: vStorage has no Terraform resource so manage buckets via the AWS S3 provider
type: lesson
---

# vStorage has no Terraform resource so manage buckets via the AWS S3 provider

GreenNode/VNG Cloud **vStorage has no native `vngcloud_*` Terraform resource**. Because it is **S3-compatible**, manage buckets with the **`hashicorp/aws`** provider pointed at the vStorage S3 endpoint.

Provider config essentials:

```hcl
provider "aws" {
  access_key = var.vstorage_access_key
  secret_key = var.vstorage_secret_key
  region     = "hcm03"
  skip_credentials_validation = true
  skip_requesting_account_id  = true
  skip_metadata_api_check     = true
  skip_region_validation      = true
  s3_use_path_style           = true
  endpoints { s3 = "https://hcm03.vstorage.vngcloud.vn" }
}
```

Then `aws_s3_bucket` / `aws_s3_bucket_versioning` work normally and idempotently.

**Auth gotcha:** the S3 Access/Secret key is created per **vStorage Service Account** in the IAM console (*Create S3 key* — the secret is shown **only once**). These are **separate** credentials from the `vngcloud` provider's IAM `client_id`/`client_secret`. Confirm path-style vs virtual-hosted addressing for your region. The same S3-compatible endpoint can also host Terraform remote state (`backend "s3"` with the same skip_* flags + `use_path_style`).

## Related
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[GreenNode is VNG Cloud's AI cloud exposing vDB and vStorage managed services]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]

%% ai-graph-start %%

**Related notes:**
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[VNG Cloud vStorage is S3-compatible object storage]]
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]
- [[VNG Cloud IaC = Terraform provider (no first-party CLI); vStorageregistry via S3+docker]]
- [[vStorage S3 keys differ from vIAM client credentials used by vDB]]

%% ai-graph-end %%