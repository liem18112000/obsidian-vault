---
ai_hash: ca521a92f0c3db09
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- terraform
- iac
- state
- idempotency
- design-decision
title: Keep billed bootstrap resources out of Terraform state
type: lesson
---

# Keep billed bootstrap resources out of Terraform state

When a resource is a **prerequisite that is billed and holds data** (e.g. a VNG Cloud vStorage project with a paid quota, a cloud account, a DNS zone you don't own the lifecycle of), prefer a separate **idempotent bootstrap script** over putting it in the Terraform module that manages what sits on top.

Why: `terraform destroy` (or a botched `apply`/state drift) must never be able to delete a paid, data-bearing resource. Keeping it out of state removes that whole class of foot-gun. The bootstrap script should be idempotent — GET/list first, reuse if it already exists, only create when absent — so re-running is safe and it can run before every deploy.

The Terraform module then references the bootstrapped resource by a stable id/name it does not own. Example: `deployments/storage/scripts/create-project.sh` creates the vStorage project; the Terraform (via an S3 key scoped to that project) only manages buckets inside it.

Related: [[vStorage project is a paid prerequisite Terraform cannot create]], [[vStorage REST control-plane API: endpoints and vIAM bearer auth]].

## Related

- [[vStorage project is a paid prerequisite Terraform cannot create]]
- [[vStorage REST control-plane API: endpoints and vIAM bearer auth]]

%% ai-graph-start %%

**Related notes:**
- [[vStorage project is a paid prerequisite Terraform cannot create]]
- [[vStorage API project creation needs a billing order (payment method or POC wallet)]]
- [[Creating a vStorage bucket is free (data-plane); only the project quota + usage cost money]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[Split Terraform cluster-provisioning state separate from in-cluster workload state]]

%% ai-graph-end %%