---
ai_hash: 351a2d4a9b9ce805
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-23
entities:
- Vault/OpenBao
- VNG KMS
- auto-unseal
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
- local disk
- DR
- customer360 secret/config flow on VNG
- GitHub Actions
- .env
- tfstate-on-vStorage
- deployments/configs/vault-openbao-deep-dive.md
- HSM
- storage-backend
- backend
- unsupported seal
- Vault/OpenBao on VNG Cloud
- native cloud-KMS auto-unseal
- hands-off prod
- unattended reboots/recovery
- one bootstrap vault
- vStorage as storage-backend
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

Also: use **Integrated Storage (Raft)** on local disk as the backend (snapshot to vStorage for DR), NOT vStorage-as-storage-backend. Context: [[customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage]]. Full analysis: deployments/configs/vault-openbao-deep-dive.md.

## Related

- [[customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud has KMS (keys only) but no managed secrets manager or parameter store]]
- [[customer360 secretconfig flow on VNG GitHub Actions + .env + tfstate-on-vStorage]]
- [[OpenBao = MPL-2.0 Linux-Foundation fork of Vault 1.14, drop-in compatible; Vault is BUSLIBM]]
- [[Manage VNG Cloud vStorage buckets with the AWS Terraform provider, not vngcloud]]
- [[Configure vStorage S3 backend creds in each component .env so deploy scripts self-auth]]

**Relations:**
- Vault/OpenBao — *cannot auto-unseal with* — VNG KMS
- Vault/OpenBao — *supports auto-unseal with* — AliCloud KMS
- Vault/OpenBao — *supports auto-unseal with* — AWS KMS
- Vault/OpenBao — *supports auto-unseal with* — Azure Key Vault
- Vault/OpenBao — *supports auto-unseal with* — GCP Cloud KMS
- Vault/OpenBao — *supports auto-unseal with* — OCI KMS
- Vault/OpenBao — *supports auto-unseal with* — Transit
- Vault/OpenBao — *supports auto-unseal with* — PKCS#11 HSM
- VNG KMS — *is an* — unsupported seal
- VNG KMS — *is not* — AWS-KMS-API compatible
- vStorage — *is* — S3-API compatible
- awskms seal — *cannot be pointed at* — VNG KMS
- Vault/OpenBao on VNG Cloud — *lacks* — native cloud-KMS auto-unseal
- Shamir — *is an unseal method* — Shamir
- Transit — *is an auto-unseal method* — Transit
- Shamir — *blocks* — unattended reboots/recovery
- Transit — *is recommended for* — hands-off prod
- Transit — *requires* — one bootstrap vault
- PKCS#11 HSM — *requires* — HSM
- PKCS#11 HSM — *is not practical on* — VNG Cloud
- Integrated Storage (Raft) — *is recommended as* — backend
- Integrated Storage (Raft) — *stores data on* — local disk
- Integrated Storage (Raft) — *snapshots to* — vStorage
- vStorage — *is used for* — DR
- vStorage — *is not recommended as* — storage-backend
- customer360 secret/config flow on VNG — *provides context for* — vStorage as storage-backend
- GitHub Actions — *is part of* — customer360 secret/config flow on VNG
- .env — *is part of* — customer360 secret/config flow on VNG
- tfstate-on-vStorage — *is part of* — customer360 secret/config flow on VNG
- deployments/configs/vault-openbao-deep-dive.md — *is a* — full analysis
- deployments/configs/vault-openbao-deep-dive.md — *related to* — Vault/OpenBao
- customer360 secret/config flow on VNG — *related to* — Vault/OpenBao

%% ai-graph-end %%