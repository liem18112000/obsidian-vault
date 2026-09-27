---
ai_hash: a0c62a45f9d2f984
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17
status: seedling
tags:
- greennode
- vngcloud
- vdb
- api
- debugging
- terraform
title: Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary
type: howto
---

# Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary

When a cloud API is undocumented/gated (VNG Cloud/GreenNode vDB catalog endpoints returned 403 for every guessed path), **extract the real endpoint path strings from the Terraform provider binary**:

```bash
EXE=$(find .terraform -iname 'terraform-provider-vngcloud*.exe' | head -1)
grep -aoE '/(vdb-[a-z]+)/v[0-9][A-Za-z0-9/_{}.-]*' "$EXE" | sort -u
```

`grep -a` treats the binary as text; Go embeds route templates as plain string literals, so the real paths fall right out. This beat reading the Go source (GitHub 429'd) and TF_LOG=TRACE (the provider doesn't log request URLs/bodies).

**Recovered vDB RELATIONAL (standalone) catalog endpoints** (base `https://vdb-gateway.vngcloud.vn`, Bearer token, header `portal-user-id: <userId>`):
- flavors/packages: `GET /vdb-relational/v1/database-instances/flavors`
- volume types:     `GET /vdb-relational/v1/database-instances/volume/types?zoneId=<zone>`
- zones:            `GET /vdb-relational/v1/database-instances/zones`
(Cluster equivalents live under `/vdb-postgresql/v1/cluster/...`.)

**HCM03-1C standalone catalog** (the account's only vServer-enabled AZ): packages use `db.s-general-<v>x<r>` (e.g. db.s-general-8x16); volume types are `ssd-iops<N>-HCM03-1C` with N in {200,400,800,1000,1200,1600,3000,3200} — **max 3200 IOPS** (no 10000 tier). Contrast HCM03-1A which had `db.s2-general-*` + `Gen2-NVMe2-IOPS10000`. Catalog names are per-AZ. See [[GreenNode vDB create constraints instance name 6-20 chars, password start-with-letter, package family s2-general|GreenNode vDB create constraints: instance name 6-20 chars, password start-with-letter, package family s2-general]].

## Related

- [[GreenNode vDB create constraints: instance name 6-20 chars]]
- [[password start-with-letter]]
- [[package family s2-general]]

%% ai-graph-start %%

**Related notes:**
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]
- [[VNG Cloud Terraform provider vDB service-to-resource mapping]]
- [[10000-IOPS standalone vDB PostgreSQL needs a vServer-enabled zone that offers Gen2-NVMe2-IOPS10000 (HCM03-1A)]]
- [[VNG Cloud vServer discovering account catalog names via the vserver-gateway API]]
- [[GreenNode vDB package family + IOPS tier are per-zone (HCM03-1A vs 1C)]]

%% ai-graph-end %%