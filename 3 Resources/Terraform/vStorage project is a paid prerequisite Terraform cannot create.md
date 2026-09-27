---
ai_hash: 6531d795cd5fcfb7
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
- object-storage
- gotcha
- prerequisite
title: vStorage project is a paid prerequisite Terraform cannot create
type: lesson
---

# vStorage project is a paid prerequisite Terraform cannot create

VNG Cloud vStorage object storage has a three-level hierarchy, and the top level is a **paid prerequisite** you must set up before any Terraform runs:

```
vStorage PROJECT   ← console: region + quota/package + billing period + auto-renew, then checkout
   └── S3 key       ← created INSIDE the project; scopes access_key/secret_key to it
        └── bucket(s)  ← managed over the S3 API (Terraform via hashicorp/aws)
```

**Terraform cannot create the project.** Two reasons: the `vngcloud/vngcloud` provider does not expose vStorage at all, and project creation is a *billing* action (you pick the storage quota/package and pay) that is not part of the S3 API the `aws` provider speaks. So the project is a manual console step at `https://vstorage.console.vngcloud.vn` → Create a Project.

Consequences:
- The **storage quota (and its cost) lives on the project**, not in Terraform. A Terraform `estimated_storage_tb`-style cost figure is only a forecast/output — it provisions nothing.
- The **S3 key is what binds a Terraform module to one specific project**; Terraform then only creates/manages buckets *inside* it. A project can hold one or more buckets.
- Analogous to how the managed-DB (vDB) module needs a vIAM service account created first — the control-plane identity/resource is a manual prereq, Terraform manages what sits on top.

Related: [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]], [[vStorage S3 keys differ from vIAM client credentials used by vDB]].

## Related

- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider]]
- [[not vngcloud]]
- [[vStorage S3 keys differ from vIAM client credentials used by vDB]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud vStorage is S3-compatible object storage]]
- [[vStorage API project creation needs a billing order (payment method or POC wallet)]]
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[vStorage REST control-plane API endpoints and vIAM bearer auth]]
- [[vStorage has no Terraform resource so manage buckets via the AWS S3 provider]]

%% ai-graph-end %%