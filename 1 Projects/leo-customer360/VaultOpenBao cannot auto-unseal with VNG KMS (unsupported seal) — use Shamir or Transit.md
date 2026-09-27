---
ai_hash: 43792a220919f343
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Vault
- OpenBao
- VNG KMS
- Shamir
- Transit
- AliCloud KMS
- AWS KMS
- Azure Key Vault
- GCP Cloud KMS
- OCI KMS
- PKCS#11 HSM
- VNG Cloud
- vStorage
- S3-API
- AWS-KMS-API
- awskms seal
- Integrated Storage (Raft)
- 'customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage'
- deployments/configs/vault-openbao-deep-dive.md
- HSM
- DR
- hands-off prod
- bootstrap vault
- auto-unseal
- backend
- unattended reboots/recovery
source: web research + deep-dive 2026-09-23
status: seedling
tags:
- openbao
- vault
- vng-cloud
- auto-unseal
- secrets
- gotcha
title: Vault/OpenBao cannot auto-unseal with VNG KMS (unsupported seal) — use Shamir
  or Transit
type: observation
---

# Vault/OpenBao cannot auto-unseal with VNG KMS (unsupported seal) — use Shamir or Transit

Vault/OpenBao **auto-unseal** supports only these seals: AliCloud KMS, AWS KMS, Azure Key Vault, GCP Cloud KMS, OCI KMS, **Transit** (another vault), and **PKCS#11 HSM** (Enterprise). **VNG Cloud KMS is NOT on that list**, and — unlike vStorage which is S3-*API*-compatible — VNG KMS is not AWS-KMS-*API*-compatible, so the `awskms` seal cannot be pointed at it.

Implication for running a self-hosted vault on VNG Cloud: no native cloud-KMS auto-unseal. Choose either **Shamir** (manual N-of-M unseal on every restart — simple, but blocks unattended reboots/recovery) or **Transit auto-unseal** (a small second OpenBao holds the transit key; the main cluster auto-unseals against it — recommended for hands-off prod, at the cost of one bootstrap vault). PKCS#11 needs a real HSM (not practical on VNG).

Also: use **Integrated Storage (Raft)** on local disk as the backend (snapshot to vStorage for DR), NOT vStorage-as-storage-backend. Context: [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage|customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage]]. Full analysis: deployments/configs/vault-openbao-deep-dive.md.

## Related

- [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage|customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store]]
- [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage]]
- [[OpenBao = MPL-2.0 Linux-Foundation fork of Vault 1.14, drop-in compatible; Vault is BUSLIBM]]
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[How to unseal Vault Unseal]]

**Relations:**
- Vault — *cannot auto-unseal with* — VNG KMS
- OpenBao — *cannot auto-unseal with* — VNG KMS
- VNG KMS — *is an* — unsupported seal
- Vault — *can use for auto-unseal* — Shamir
- OpenBao — *can use for auto-unseal* — Shamir
- Vault — *can use for auto-unseal* — Transit
- OpenBao — *can use for auto-unseal* — Transit
- auto-unseal — *supports* — AliCloud KMS
- auto-unseal — *supports* — AWS KMS
- auto-unseal — *supports* — Azure Key Vault
- auto-unseal — *supports* — GCP Cloud KMS
- auto-unseal — *supports* — OCI KMS
- auto-unseal — *supports* — Transit
- auto-unseal — *supports* — PKCS#11 HSM
- VNG KMS — *is not supported by* — auto-unseal
- VNG KMS — *is not* — AWS-KMS-API-compatible
- vStorage — *is* — S3-API-compatible
- awskms seal — *cannot be pointed at* — VNG KMS
- Vault — *on VNG Cloud lacks native* — auto-unseal
- Shamir — *is a* — manual unseal method
- Shamir — *blocks* — unattended reboots/recovery
- Transit — *is an* — auto-unseal method
- Transit — *uses* — bootstrap vault
- Transit — *is recommended for* — hands-off prod
- PKCS#11 HSM — *needs a* — HSM
- PKCS#11 HSM — *is not practical on* — VNG Cloud
- Integrated Storage (Raft) — *is recommended as* — backend
- Integrated Storage (Raft) — *snapshots to* — vStorage
- vStorage — *is not recommended as* — backend
- customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage — *provides context for* — backend
- deployments/configs/vault-openbao-deep-dive.md — *contains analysis of* — Vault
- deployments/configs/vault-openbao-deep-dive.md — *contains analysis of* — OpenBao

%% ai-graph-end %%