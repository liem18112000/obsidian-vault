---
ai_hash: 843b516717b278d1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- vngcloud
- vstorage
- vdb
- credentials
- terraform
title: vStorage S3 keys differ from vIAM client credentials used by vDB
type: term
---

# vStorage S3 keys differ from vIAM client credentials used by vDB

VNG Cloud has **two different credential types** and they are not interchangeable:

- **vStorage S3 keys** (`access_key` / `secret_key`) — used to talk to the S3 object-storage API. Created under vStorage console → IAM → Service account → **vStorage credentials → Create a S3 key**. The Secret Key is shown **only once** on creation.
- **vIAM service-account credentials** (`client_id` / `client_secret`) — used by the `vngcloud/vngcloud` Terraform provider for vServer / vDB (e.g. the managed PostgreSQL in `deployments/postgres`), authenticating against `iamapis.vngcloud.vn`.

So a Terraform module that provisions buckets needs S3 keys, while one that provisions a managed DB needs the vIAM client id/secret — don't reuse one for the other.

Related: [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]].

## Related

- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider]]
- [[not vngcloud]]

%% ai-graph-start %%

**Related notes:**
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]
- [[vStorage project is a paid prerequisite Terraform cannot create]]
- [[VNG Cloud IaC = Terraform provider (no first-party CLI); vStorageregistry via S3+docker]]
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]

%% ai-graph-end %%