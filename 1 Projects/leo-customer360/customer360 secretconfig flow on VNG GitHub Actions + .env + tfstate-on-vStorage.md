---
title: "customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage"
created: 2026-09-23
type: observation
status: seedling
source: "deployments/ inspection 2026-09-23"
tags: [leo-customer360, secrets, vng-cloud, terraform, ci-cd, deployment]
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
