---
title: "Vault/OpenBao cannot auto-unseal with VNG KMS (unsupported seal) — use Shamir or Transit"
created: 2026-09-23
type: observation
status: seedling
source: "web research + deep-dive 2026-09-23"
tags: [openbao, vault, vng-cloud, auto-unseal, secrets, gotcha]
---

# Vault/OpenBao cannot auto-unseal with VNG KMS (unsupported seal) — use Shamir or Transit

Vault/OpenBao **auto-unseal** supports only these seals: AliCloud KMS, AWS KMS, Azure Key Vault, GCP Cloud KMS, OCI KMS, **Transit** (another vault), and **PKCS#11 HSM** (Enterprise). **VNG Cloud KMS is NOT on that list**, and — unlike vStorage which is S3-*API*-compatible — VNG KMS is not AWS-KMS-*API*-compatible, so the `awskms` seal cannot be pointed at it.

Implication for running a self-hosted vault on VNG Cloud: no native cloud-KMS auto-unseal. Choose either **Shamir** (manual N-of-M unseal on every restart — simple, but blocks unattended reboots/recovery) or **Transit auto-unseal** (a small second OpenBao holds the transit key; the main cluster auto-unseals against it — recommended for hands-off prod, at the cost of one bootstrap vault). PKCS#11 needs a real HSM (not practical on VNG).

Also: use **Integrated Storage (Raft)** on local disk as the backend (snapshot to vStorage for DR), NOT vStorage-as-storage-backend. Context: [[customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage]]. Full analysis: deployments/configs/vault-openbao-deep-dive.md.

## Related

- [[customer360 secret/config flow on VNG: GitHub Actions + .env + tfstate-on-vStorage]]
