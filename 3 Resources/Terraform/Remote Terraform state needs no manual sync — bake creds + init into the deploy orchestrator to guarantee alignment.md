---
ai_hash: 0978275389ddb008
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-21
entities: []
source: session 2026-08-21, deployments/lib/tfstate.sh
status: seedling
tags:
- terraform
- remote-state
- ci-cd
- operations
- leo-customer360
title: Remote Terraform state needs no manual sync — bake creds + init into the deploy
  orchestrator to guarantee alignment
type: lesson
---

# Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment

With a **remote** Terraform backend you never "sync state down" — Terraform reads/writes it live from the bucket on every command, so local and CI runs operate on the same state by construction. What a machine actually needs is only: (1) `terraform init` ONCE per module per checkout (binds `.terraform/` to the remote backend; `.terraform/` is gitignored and machine-local; this is NOT `-migrate-state`, which is a one-time migration), and (2) the backend credentials in the environment (e.g. `AWS_ACCESS_KEY_ID`/`AWS_SECRET_ACCESS_KEY`).

Gotcha: switching from a local to a remote backend means commands that previously needed NO creds (`terraform output`, `workspace select`) now must authenticate — scripts that read outputs silently break locally until creds are exported.

Fix to make it foolproof: have the deploy orchestrators preflight (a) load the backend creds into the env from existing config and (b) run `terraform init -input=false` on the remote-backend modules — both idempotent. Then a local `./deploy-all.sh` can never drift onto a stale local `terraform.tfstate.d/` copy. In leo-customer360 this is `deployments/lib/tfstate.sh` (`ensure_vstorage_creds` + `ensure_remote_init`) sourced by `deploy-all.sh`. Also: the old local `terraform.tfstate.d/` files become stale/ignored after migration, and vStorage has no lock table so there is no state locking (avoid concurrent applies). Related: [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]].

## Related

- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]

%% ai-graph-start %%

**Related notes:**
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Terraform S3 backend on a non-AWS store (vStorageMinIO) needs skip-checks + path-style]]
- [[CI-driven CD cannot resolve local gitignored Terraform state — needs remote backend or IPs via secrets]]

%% ai-graph-end %%