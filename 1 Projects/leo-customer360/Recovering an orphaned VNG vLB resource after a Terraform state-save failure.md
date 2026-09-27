---
ai_hash: 8ddcfabf5c4a31ed
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities:
- VNG vLB resource
- Terraform
- Terraform state-save failure
- VNG/GreenNode vLB
- leocdp360 load_balancer module
- S3/vStorage backend
- DNS blip
- vStorage endpoint
- hcm04.vstorage.vngcloud.vn
- Plugin did not respond
- Duplicated Pool name
- errored.tfstate
- python -c
- terraform state push
- secgroup rule
- Orphaned resource
- Service account
- IAM-denied
- API enumeration
- GreenNode console
- vLB pool ID
- terraform import
- -var-file
- overlays/uat.tfvars
- vngcloud_vlb_pool.this["jaeger"]
- deploy.sh
- Listener resource
- VNG token endpoint
- iamapis.vngcloud.vn/accounts-api/v2/auth/token
- HTTP Basic auth
- IAM policy
- vLB reads
- CI
- Monitoring
- Load Balancer (LB)
- Running leo-customer360 deploys locally
- vStorage backend creds
- Drift
- client id/secret
- grant_type=client_credentials
- projectId
- loadBalancerId
- poolId
- completed resources
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
[[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

## Related

- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoring/LB]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vLB pool in-place update fails ('Stickiness cannot be specified for non-HTTP pools') — use terraform -replace]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]
- [[VNG vLB replace the listener when its pool is ForceNew-replaced]]

**Relations:**
- VNG vLB resource — *experiences* — Terraform state-save failure
- Terraform — *applies against* — VNG/GreenNode vLB
- Terraform — *uses* — leocdp360 load_balancer module
- Terraform — *uses* — S3/vStorage backend
- Terraform state-save failure — *caused by* — DNS blip
- DNS blip — *occurs on* — vStorage endpoint
- vStorage endpoint — *is* — hcm04.vstorage.vngcloud.vn
- DNS blip — *leads to* — Plugin did not respond
- Plugin did not respond — *causes* — Drift
- Drift — *implies* — resources exist but aren't in state
- blind retry — *fails due to* — Duplicated Pool name
- Terraform — *writes* — errored.tfstate
- errored.tfstate — *contains* — completed resources
- python -c — *inspects* — errored.tfstate
- terraform state push — *uses* — errored.tfstate
- terraform state push — *reconciles* — completed resources
- completed resources — *include* — secgroup rule
- Orphaned resource — *is* — created but NOT in errored.tfstate
- terraform import — *recovers* — Orphaned resource
- Service account — *can* — CREATE VNG vLB resource
- Service account — *is subject to* — IAM-denied
- IAM-denied — *applies to* — LIST/GET
- IAM-denied — *prevents* — API enumeration
- Operator — *obtains* — vLB pool ID
- Operator — *uses* — GreenNode console
- GreenNode console — *provides* — vLB pool ID
- vLB pool ID — *format includes* — projectId
- vLB pool ID — *format includes* — loadBalancerId
- vLB pool ID — *format includes* — poolId
- terraform import — *requires* — vLB pool ID
- terraform import — *uses* — -var-file
- -var-file — *specifies* — overlays/uat.tfvars
- terraform import — *targets* — vngcloud_vlb_pool.this["jaeger"]
- deploy.sh — *executes* — apply
- apply — *configures* — Listener resource
- VNG token endpoint — *is* — iamapis.vngcloud.vn/accounts-api/v2/auth/token
- VNG token endpoint — *requires* — grant_type=client_credentials
- VNG token endpoint — *authenticates with* — HTTP Basic auth
- HTTP Basic auth — *uses* — client id/secret
- Service account's IAM policy — *blocks* — vLB reads
- vLB reads — *are blocked by* — IAM policy
- API enumeration — *is a* — dead end
- GreenNode console — *is used for* — vLB pool ID
- Running leo-customer360 deploys locally — *needs* — vStorage backend creds
- CI — *cannot perform* — Monitoring
- CI — *cannot perform* — Load Balancer (LB)
- leocdp360 load_balancer module — *is used in* — Running leo-customer360 deploys locally
- Load Balancer (LB) — *is a type of* — VNG vLB resource

%% ai-graph-end %%