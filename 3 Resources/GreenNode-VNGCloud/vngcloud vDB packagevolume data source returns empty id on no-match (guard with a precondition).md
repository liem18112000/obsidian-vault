---
ai_hash: 45e9620964389147
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
- terraform
- gotcha
title: vngcloud vDB package/volume data source returns empty id on no-match (guard
  with a precondition)
type: lesson
---

# vngcloud vDB package/volume data source returns empty id on no-match (guard with a precondition)

On GreenNode/VNG Cloud vDB, the `vngcloud_vdb_database_package` and `vngcloud_vdb_database_volume_type` data sources **do not error when nothing matches** — they return successfully with an **empty/null `id`**. When that null feeds a required resource argument (`package_id = data.vngcloud_vdb_database_package.pg.id`), Terraform reports the misleading:

`Error: Missing required argument ... "package_id" is required, but no definition was found`

even though the line IS present. The real cause: `package_name` (or engine_version/zone) didn't match any real package.

**Tell-tale in plan output:** the matching data source prints `Read complete` WITHOUT an `[id=...]` suffix (a good match prints `[id=Gen2-NVMe2-IOPS10000]`).

**Fixes:**
1. Copy the EXACT `package_name` + `engine_version` from the console create-form dropdowns (they're account/region-specific and NOT in public docs). Name format seen: `db.s-general-<vCPU>x<GB>` (e.g. db.s-general-2x8) — but only the console lists which sizes actually exist.
2. Guard with a resource `lifecycle { precondition { condition = try(data...id, "") != "", error_message = "..." } }` so a bad name fails early with an actionable message instead of the cryptic 'Missing required argument'.

See [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider|Provision GreenNode/VNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]].

## Related

- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider|Provision GreenNode/VNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]

%% ai-graph-start %%

**Related notes:**
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]
- [[GreenNode vDB create constraints instance name 6-20 chars, password start-with-letter, package family s2-general]]
- [[Provision GreenNodeVNG Cloud vDB PostgreSQL with the vngcloud Terraform provider]]
- [[VNG Cloud Terraform provider maps managed Postgres and Redis to vdb resources]]
- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]

%% ai-graph-end %%