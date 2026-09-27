---
ai_hash: 7f3f3cdc7bbd3c03
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- LEO customer360
- VNG Cloud
- GitHub Actions
- .env files
- KMS
- managed secrets manager
- parameter store
- GitHub Actions secrets
- cd.yml
- deploy-*.sh
- deployments/server/.env
- vIAM
- service-account creds
- TF_VAR_client_id
- TF_VAR_secret
- provider.tf
- app runtime keys
- SMTP
- smtp.uat.env
- smtp.*.local.env
- Terraform remote state
- VNG vStorage
- S3 bucket
- leocdp360-tfstate
- hcm04.vstorage.vngcloud.vn
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- terraform.tfvars
- '*.auto.tfvars'
- '*.tfstate*'
- overlays/*.tfvars
- SOPS
- age
- K8s Secrets
- KMS envelope encryption
- VKS migration
- deployments/docs/vks-migration-technical-analysis.md
- deployments/configs/vng-secrets-management.md
- LEO_OPENAI_API_KEY
- AGENT_API_TOKEN
- DB_PASSWORD
- REDIS_PASSWORD
- BREVO_SMTP_*
- KEYCLOAK_*
- VSTORAGE_ACCESS/SECRET_KEY
- DEPLOY_SSH_KEY
- PORTAINER_ADMIN_PASSWORD
- TELEGRAM_*
- DOCS_*
- deploy.sh
- .gitignore
- plaintext secrets in remote state
- duplicated secrets
- no rotation/audit
- harden vStorage tfstate bucket
- private access
- KMS at-rest
- secrets
source: deployments/ inspection 2026-09-23
status: seedling
tags:
- leo-customer360
- secrets
- vng-cloud
- terraform
- ci-cd
- deployment
title: 'customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage'
type: observation
---

# customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage

How LEO customer360 handles config + secrets on VNG Cloud (no managed secrets service exists — see [[VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store]]). Three stores:

1. **GitHub Actions secrets** = de-facto source of truth (~20: LEO_OPENAI_API_KEY, AGENT_API_TOKEN, DB_PASSWORD, REDIS_PASSWORD, BREVO_SMTP_*, KEYCLOAK_*, VSTORAGE_ACCESS/SECRET_KEY, DEPLOY_SSH_KEY, PORTAINER_ADMIN_PASSWORD, TELEGRAM_*, DOCS_*). `cd.yml` injects them; `deploy-*.sh` render on-server `.env`.
2. **Git-ignored `.env` files**: `deployments/server/.env` holds vIAM service-account creds as `TF_VAR_client_id/secret` (deploy.sh auto-loads -> provider.tf) + app runtime keys. SMTP split: `smtp.uat.env` committed/non-secret vs `smtp.*.local.env` git-ignored/secret.
3. **Terraform remote state** on VNG **vStorage** S3 bucket `leocdp360-tfstate` (hcm04.vstorage.vngcloud.vn), workspace-per-env, via AWS_ACCESS_KEY_ID/SECRET. ⚠ **state can hold secrets in plaintext** (module .gitignore says so) -> the bucket is a secret-bearing asset.

Gitignore discipline: `terraform.tfvars`, `*.auto.tfvars`, `.env`, `smtp.*.local.env`, `*.tfstate*` ignored; `overlays/*.tfvars` committed (non-secret).

Key gaps: plaintext secrets in remote state; same secret duplicated across all three stores (3 rotation points); no rotation/audit. Recommended path: harden the vStorage tfstate bucket (private + KMS at-rest) first, add SOPS+age for at-rest secrets, then move runtime secrets to K8s Secrets + KMS envelope encryption when the VKS migration lands (`deployments/docs/vks-migration-technical-analysis.md`). Full report: `deployments/configs/vng-secrets-management.md`.

## Related

- [[VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store]]

%% ai-graph-start %%

**Related notes:**
- [[Running leo-customer360 deploys locally needs vStorage backend creds; CI can't do monitoringLB]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]
- [[VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[Terraform S3 remote backend for VNG vStorage (S3-compatible) config recipe]]

**Relations:**
- LEO customer360 — *handles config + secrets on* — VNG Cloud
- VNG Cloud — *has* — KMS
- VNG Cloud — *lacks* — managed secrets manager
- VNG Cloud — *lacks* — parameter store
- LEO customer360 — *uses* — GitHub Actions secrets
- LEO customer360 — *uses* — .env files
- LEO customer360 — *uses* — Terraform remote state
- GitHub Actions secrets — *is source of truth for* — LEO_OPENAI_API_KEY
- GitHub Actions secrets — *is source of truth for* — AGENT_API_TOKEN
- GitHub Actions secrets — *is source of truth for* — DB_PASSWORD
- GitHub Actions secrets — *is source of truth for* — REDIS_PASSWORD
- GitHub Actions secrets — *is source of truth for* — BREVO_SMTP_*
- GitHub Actions secrets — *is source of truth for* — KEYCLOAK_*
- GitHub Actions secrets — *is source of truth for* — VSTORAGE_ACCESS/SECRET_KEY
- GitHub Actions secrets — *is source of truth for* — DEPLOY_SSH_KEY
- GitHub Actions secrets — *is source of truth for* — PORTAINER_ADMIN_PASSWORD
- GitHub Actions secrets — *is source of truth for* — TELEGRAM_*
- GitHub Actions secrets — *is source of truth for* — DOCS_*
- cd.yml — *injects* — GitHub Actions secrets
- deploy-*.sh — *renders* — .env files
- deployments/server/.env — *holds* — vIAM service-account creds
- vIAM service-account creds — *include* — TF_VAR_client_id
- vIAM service-account creds — *include* — TF_VAR_secret
- deploy.sh — *auto-loads* — deployments/server/.env
- deployments/server/.env — *configures* — provider.tf
- deployments/server/.env — *holds* — app runtime keys
- SMTP — *uses* — smtp.uat.env
- SMTP — *uses* — smtp.*.local.env
- smtp.uat.env — *is* — committed
- smtp.uat.env — *is* — non-secret
- smtp.*.local.env — *is* — git-ignored
- smtp.*.local.env — *is* — secret
- Terraform remote state — *is stored on* — VNG vStorage
- VNG vStorage — *is an* — S3 bucket
- S3 bucket — *is named* — leocdp360-tfstate
- leocdp360-tfstate — *is located at* — hcm04.vstorage.vngcloud.vn
- Terraform remote state — *uses* — AWS_ACCESS_KEY_ID
- Terraform remote state — *uses* — AWS_SECRET_ACCESS_KEY
- Terraform remote state — *can hold* — plaintext secrets in remote state
- leocdp360-tfstate — *is a* — secret-bearing asset
- .gitignore — *ignores* — terraform.tfvars
- .gitignore — *ignores* — *.auto.tfvars
- .gitignore — *ignores* — .env files
- .gitignore — *ignores* — smtp.*.local.env
- .gitignore — *ignores* — *.tfstate*
- overlays/*.tfvars — *is* — committed
- overlays/*.tfvars — *is* — non-secret
- plaintext secrets in remote state — *is a gap for* — LEO customer360
- duplicated secrets — *is a gap for* — LEO customer360
- no rotation/audit — *is a gap for* — LEO customer360
- LEO customer360 — *recommends* — harden vStorage tfstate bucket
- harden vStorage tfstate bucket — *includes* — private access
- harden vStorage tfstate bucket — *includes* — KMS at-rest
- LEO customer360 — *recommends* — add SOPS
- LEO customer360 — *recommends* — add age
- SOPS — *is for* — secrets
- age — *is for* — secrets
- LEO customer360 — *recommends* — move runtime secrets to K8s Secrets
- K8s Secrets — *uses* — KMS envelope encryption
- move runtime secrets to K8s Secrets — *depends on* — VKS migration
- VKS migration — *is documented in* — deployments/docs/vks-migration-technical-analysis.md
- deployments/configs/vng-secrets-management.md — *is a* — full report

%% ai-graph-end %%