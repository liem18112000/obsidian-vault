---
title: "OpenBao = MPL-2.0 Linux-Foundation fork of Vault 1.14, drop-in compatible; Vault is BUSL/IBM"
created: 2026-09-23
type: term
status: seedling
source: "web research 2026-09-23"
tags: [openbao, vault, secrets, licensing, busl, mpl]
---

# OpenBao = MPL-2.0 Linux-Foundation fork of Vault 1.14, drop-in compatible; Vault is BUSL/IBM

OpenBao is a community fork of HashiCorp Vault, created from Vault **1.14.0** (the last MPL-2.0 release) after HashiCorp relicensed Vault to **BUSL-1.1** in Aug 2023. As of early 2026 OpenBao is at **2.5.0** (Feb 2026), under **Linux Foundation** governance (IBM engineers contribute), and is **API/CLI/secrets-engine/auth-method drop-in compatible** with Vault.

Licensing nuance: BUSL-1.1 permits internal production use — it only restricts offering a *competing hosted secrets-management service*. So Vault BUSL is legally usable for internal use; HashiCorp Vault is now an **IBM product** (acquisition completed early 2025). The reason to prefer OpenBao for a fresh self-hosted deployment is governance/openness (MPL-2.0, LF), not a legal blocker.

Community/OpenBao core covers kv-v2, database (dynamic + static-role rotation), transit, pki, ssh, AppRole/OIDC/Kubernetes auth, Raft HA, audit. Enterprise-only (paid) features you forgo: namespaces, HSM seal, DR/perf replication, control groups.

Verified Sep 2026 across openlogic / digitalis / bespinian / openbao.org.
