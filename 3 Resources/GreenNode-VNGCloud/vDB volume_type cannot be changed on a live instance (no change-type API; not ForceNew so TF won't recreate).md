---
ai_hash: 92f6959c168b2b97
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18
status: seedling
tags:
- greennode
- vngcloud
- vdb
- terraform
- volume
- gotcha
title: vDB volume_type cannot be changed on a live instance (no change-type API; not
  ForceNew so TF won't recreate)
type: lesson
---

# vDB volume_type cannot be changed on a live instance (no change-type API; not ForceNew so TF won't recreate)

GreenNode/VNG Cloud vDB standalone (`vngcloud_vdb_relational_database`): the **volume TYPE (IOPS tier) cannot be changed on an existing instance**. Corroborated two ways:
- Provider binary exposes only `.../{instanceId}/resize-storage` (volume SIZE up) and `.../{instanceId}/resize-instance` (flavor/package) — **no change-volume-type endpoint**.
- In the TF schema `volume_type` is **NOT ForceNew** (only engine_*, name, subnet_id, username, db_name, zone_id, backup_id, is_poc are). So Terraform won't destroy+recreate to change it either — it attempts an unsupported in-place update -> apply error / no-op / drift.

**Gotcha:** because it's not ForceNew, `terraform plan` may show `~ volume_type update in-place` for a type change that apply cannot actually perform — don't trust that plan line for volume_type.

**To change volume type:** recreate the instance (destroy+create) or restore a backup into a new instance with the desired type. `volume_size` (grow-only) and the flavor/package CAN be changed in place.

Zone reality: HCM03-1C standalone volume types are `ssd-iops200..3200` (max 3200 IOPS); higher IOPS is cluster-only. See [[GreenNode vDB create constraints instance name 6-20 chars, password start-with-letter, package family s2-general|GreenNode vDB create constraints: instance name 6-20 chars, password start-with-letter, package family s2-general]].

## Related

- [[GreenNode vDB create constraints: instance name 6-20 chars]]
- [[password start-with-letter]]
- [[package family s2-general]]

%% ai-graph-start %%

**Related notes:**
- [[GreenNode vDB package family + IOPS tier are per-zone (HCM03-1A vs 1C)]]
- [[10000-IOPS standalone vDB PostgreSQL needs a vServer-enabled zone that offers Gen2-NVMe2-IOPS10000 (HCM03-1A)]]
- [[Recover undocumented vDB API endpoints by grep-ing the vngcloud provider binary]]
- [[VNG vServer default catalog endpoints return the DISABLED default AZ; use zoneId=AZ and pin UUIDs]]
- [[VNG vServer flavor resize (s-general-1x2 - 2x4) is an in-place terraform change (0 destroy) but reboots the box]]

%% ai-graph-end %%