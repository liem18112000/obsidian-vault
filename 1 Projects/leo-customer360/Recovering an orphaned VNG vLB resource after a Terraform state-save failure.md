---
ai_hash: '2548873979549096'
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- Terraform
- VNG vLB resource
- state-save failure
- orphaned resource
- GreenNode
- leocdp360 load_balancer module
- S3
- vStorage
- backend
- DNS blip
- vStorage endpoint
- hcm04.vstorage.vngcloud.vn
- provider
- errored.tfstate
- module directory
- Python
- terraform state push
- secgroup rule
- service account
- IAM
- IAM_PERMISSION_DENIED
- API enumeration
- operator
- GreenNode console
- LB detail page
- vLB pool
- projectId
- loadBalancerId
- poolId
- terraform import
- -var-file
- overlays/uat.tfvars
- vngcloud_vlb_pool.this["jaeger"]
- listener
- VNG token endpoint
- iamapis.vngcloud.vn/accounts-api/v2/auth/token
- client id
- client secret
- HTTP Basic auth
- IAM policy
- vLB reads
- Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do
  monitoring/LB
source: session 2026-08-21
status: seedling
tags:
- leo-customer360
- terraform
- vngcloud
- vlb
- state-recovery
- import
- gotcha
title: Recovering an orphaned VNG vLB resource after a Terraform state-save failure
type: lesson
---

# Recovering an orphaned VNG vLB resource after a Terraform state-save failure

How to recover when a Terraform apply against the VNG/GreenNode vLB **creates resources but fails to persist state** (leocdp360 load_balancer module, S3/vStorage backend).

## The failure
A transient DNS blip on the vStorage endpoint (`lookup hcm04.vstorage.vngcloud.vn: no such host`) during the state UPLOAD after resources were created -> provider 'crashed' (`Plugin did not respond`). Real VNG resources exist but aren't in state -> drift. A blind retry fails with **`Duplicated Pool name`** (create-again conflict).

## Recovery steps
1. Terraform writes **`errored.tfstate`** in the module dir on a backend-save failure — it holds the resources whose creation COMPLETED. Inspect it (`python -c` over the json) to see what got recorded.
2. When the backend is reachable again: `terraform state push errored.tfstate` — reconciles the recorded resources (here: the secgroup rule) so they aren't recreated. (Serial must be > remote; it is, being post-apply.)
3. Anything created but NOT in errored.tfstate (creation was mid-flight at crash) is a true orphan -> **import it**.
4. **Getting the orphan's id:** the service account can CREATE vLB resources but is **IAM-denied on LIST/GET** (`IAM_PERMISSION_DENIED`), so you can't enumerate via API — the operator reads the id (`pool-...`) from the **GreenNode console** (LB detail page).
5. **Import format for a vLB pool: `{projectId}:{loadBalancerId}:{poolId}`** (the provider error tells you). E.g. `terraform import -var-file=overlays/uat.tfvars 'vngcloud_vlb_pool.this["jaeger"]' pro-...:lb-...:pool-...`. Pass the `-var-file` so the for_each key exists in config.
6. `./deploy.sh <env> apply` — plan is now just the remaining resource (the listener); applies clean.

## Gotcha
The VNG token endpoint (`iamapis.vngcloud.vn/accounts-api/v2/auth/token`) wants `grant_type=client_credentials` with the client id/secret as **HTTP Basic auth** (not in the body). But even with a valid token, this service account's IAM policy blocks vLB reads — so API enumeration is a dead end; use the console for ids.

## Related
[[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB|Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

## Related

- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB|Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vLB pool in-place update fails ('Stickiness cannot be specified for non-HTTP pools') — use terraform -replace]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]
- [[VNG vLB replace the listener when its pool is ForceNew-replaced]]

**Relations:**
- Terraform — *manages* — VNG vLB resource
- Terraform — *experiences* — state-save failure
- state-save failure — *results_in* — orphaned resource
- VNG vLB resource — *is_part_of* — GreenNode
- leocdp360 load_balancer module — *manages* — VNG vLB resource
- leocdp360 load_balancer module — *uses_as_backend* — S3
- leocdp360 load_balancer module — *uses_as_backend* — vStorage
- DNS blip — *affects* — vStorage endpoint
- vStorage endpoint — *is_located_at* — hcm04.vstorage.vngcloud.vn
- DNS blip — *causes* — provider
- provider — *crashed_with_message* — Plugin did not respond
- provider — *crash_results_in* — Duplicated Pool name error
- Terraform — *writes* — errored.tfstate
- errored.tfstate — *is_located_in* — module directory
- errored.tfstate — *contains* — secgroup rule
- Python — *inspects* — errored.tfstate
- terraform state push — *uses* — errored.tfstate
- terraform state push — *reconciles* — secgroup rule
- orphaned resource — *is_not_in* — errored.tfstate
- orphaned resource — *requires* — terraform import
- service account — *can* — CREATE VNG vLB resource
- service account — *is_subject_to* — IAM
- IAM — *denies_on* — LIST/GET
- LIST/GET — *results_in* — IAM_PERMISSION_DENIED
- IAM_PERMISSION_DENIED — *blocks* — API enumeration
- operator — *retrieves_id_from* — GreenNode console
- GreenNode console — *contains* — LB detail page
- vLB pool — *has_import_format* — {projectId}:{loadBalancerId}:{poolId}
- terraform import — *uses* — -var-file
- -var-file — *specifies* — overlays/uat.tfvars
- terraform import — *targets* — vngcloud_vlb_pool.this["jaeger"]
- vngcloud_vlb_pool.this["jaeger"] — *is_a* — vLB pool
- deploy.sh — *executes* — apply
- apply — *targets* — listener
- listener — *is_a* — VNG vLB resource
- VNG token endpoint — *is_located_at* — iamapis.vngcloud.vn/accounts-api/v2/auth/token
- VNG token endpoint — *requires_via* — HTTP Basic auth
- HTTP Basic auth — *includes* — client id
- HTTP Basic auth — *includes* — client secret
- IAM policy — *blocks* — vLB reads
- IAM policy — *blocks* — API enumeration
- GreenNode console — *provides* — ids
- Note — *mentions* — Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB

%% ai-graph-end %%