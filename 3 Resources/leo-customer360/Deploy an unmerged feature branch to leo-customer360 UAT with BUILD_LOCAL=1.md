---
title: "Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1"
created: 2026-09-10
type: howto
status: seedling
source: "leo-customer360 UAT deploy session 2026-09-10"
tags: [leo-customer360, uat, deploy, build-local, ghcr, dagster]
---

# Deploy an unmerged feature branch to leo-customer360 UAT with BUILD_LOCAL=1

To deploy an **unmerged feature branch** to the leo-customer360 UAT, you must set `BUILD_LOCAL=1`. Otherwise the app code that reaches UAT is `main`'s, not the branch's.

**Why:** `deployments/server/deploy-api.sh` and `deploy-backend.sh` default to `DEPLOY_MODE=ghcr`, which pulls the GHCR image tagged `latest` (or `image_tag` from the overlay). That image is built by CI **only on merge to main**, so it contains main's code. A feature branch has no GHCR image — pulling `latest` silently deploys the wrong code.

`BUILD_LOCAL=1` instead tars the current working tree, ships it over SSH, and builds the image **on the VM** — so the branch's code actually runs on UAT.

**The db-schema step is exempt:** `deployments/postgres/run-sql.sh` always ships the local `database-init/**` + `migrations/*.sql` files over SSH to psql, so it reflects the working tree regardless of `BUILD_LOCAL`. (It runs the full idempotent bootstrap: extensions -> database-schema.sql -> init-core-database.sql -> data-view-for-llm.sql -> forward migrations; `*.down.sql` are excluded.)

**Standard feature-branch UAT deploy:**
```bash
cd deployments
export BASTION="leocdp360@<api-box-floating-ip>"   # DB is private; psql runs on this bastion
export SSH_KEY="$HOME/.ssh/c360-api_ed25519"
export BUILD_LOCAL=1
bash deploy-all.sh uat --only db-schema -y   # migrations first
bash deploy-all.sh uat --only api -y         # ~11 min (builds on VM)
bash deploy-all.sh uat --only backend -y     # ~12 min (builds on VM)
```
`deploy-all.sh` sources `lib/tfstate.sh` to load vStorage creds and remote-init the terraform state, so `terraform output` (db host, server floating IPs) resolves against the same remote state CI uses. Server key for the Dagster box is `backend` (renamed from `1x2`).

Verifying the deployed routes afterward needs care — see [[FastAPI _IncludedRouter hides routes from app.routes introspection]].

## Related

- [[FastAPI _IncludedRouter hides routes from app.routes introspection]]
