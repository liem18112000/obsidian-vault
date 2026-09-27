---
ai_hash: 33456ded1f9ae0e4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-18
entities: []
source: session 2026-08-18 leo-customer360 deployments/server
status: seedling
tags:
- terraform
- vngcloud
- vserver
- for-each
- gotcha
title: VNG vServer name is in-place updatable; decouple server name from the for_each
  map key to rename without recreate
type: lesson
---

# VNG vServer name is in-place updatable; decouple server name from the for_each map key to rename without recreate

To rename a VNG Cloud vServer via Terraform WITHOUT recreating it (and destroying whatever runs on it):

1. The `vngcloud_vserver_server` **`name` is updatable in-place** (NOT ForceNew) — changing it plans as "will be updated in-place / 1 to change, 0 to destroy", same instance id. Verified by `terraform plan` before applying (always plan a rename first to confirm in-place vs replace).

2. When servers are created with `for_each = var.servers` and `name = "${prefix}-${each.key}"`, the display name is tied to the MAP KEY — and changing a for_each key DOES destroy+recreate (the key is the resource identity). So decouple them: add an optional `name` to the servers object and compute `name = "${prefix}-${coalesce(each.value.name, each.key)}"`. Now you can rename a server (set its `name`) while keeping its map key stable -> in-place rename, no churn.

Example: keep key "1x2" but set name="backend" -> server renamed to c360-api-uat-backend in place; the running container survived.

## Related
[[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-start %%

**Related notes:**
- [[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates]]
- [[VNG vServer flavor resize (s-general-1x2 - 2x4) is an in-place terraform change (0 destroy) but reboots the box]]
- [[vngcloud Terraform accepts root_disk_size change but does not resize the boot volume in-place]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]
- [[VNG vLB replace the listener when its pool is ForceNew-replaced]]

%% ai-graph-end %%