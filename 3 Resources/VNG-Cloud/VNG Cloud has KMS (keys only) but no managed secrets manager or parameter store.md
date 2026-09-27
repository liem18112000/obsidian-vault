---
title: "VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store"
created: 2026-09-23
type: observation
status: seedling
source: "docs.greennode.ai + VNG docs, verified 2026-09-23"
tags: [vng-cloud, greennode, secrets, kms, config-management, leo-customer360]
---

# VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store

VNG Cloud (docs now redirect to docs.greennode.ai) does **not** offer a managed secrets manager or a config/parameter-store service. Verified via the KMS docs page + the full `llms.txt` doc index (Sep 2026).

- **Has:** KMS (Key Management System) — Customer-Managed Keys, symmetric/asymmetric, key-material import. This manages *encryption keys only*, NOT arbitrary secrets/config values. Also vServer SSH key pairs + Security Groups (access control, not secrets).
- **No equivalent of:** AWS Secrets Manager / GCP Secret Manager (secret storage), or AWS SSM Parameter Store (config vars).

Implication for the customer360 stack: keep handling config+secrets via `server/.env` on the vServer with values injected by GitHub Actions secrets in CD (the current pattern). For stronger handling on VNG, all options are self-run: VKS + Kubernetes Secrets (optionally envelope-encrypted at rest via a KMS provider -> VNG KMS), self-hosted HashiCorp Vault/OpenBao, or SOPS/sealed-secrets in git.

Caveat: KMS existence is well-corroborated; the *absence* of a secrets service is "not in current public docs" (index reads were partial) — confirm in the VNG console/support before treating as final.
