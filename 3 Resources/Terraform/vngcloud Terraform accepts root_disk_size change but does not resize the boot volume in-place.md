---
ai_hash: 47d4044aa422e989
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-10
entities: []
source: customer360 UAT 2026-09-10
status: seedling
tags:
- terraform
- vngcloud
- disk
- drift
- gotcha
- infra
title: vngcloud Terraform accepts root_disk_size change but does not resize the boot
  volume in-place
type: lesson
---

# vngcloud Terraform accepts root_disk_size change but does not resize the boot volume in-place

On the vngcloud (VNG Cloud / vServer) Terraform provider, changing `root_disk_size` on an existing `vngcloud_vserver_server` plans + applies as an in-place update and Terraform writes the new value into STATE -- but the actual boot volume is NOT resized. After `terraform apply` + reboot, `lsblk` still shows the old size (e.g. sda=20G) while `terraform state show` reports the new size (50). This is **silent drift**: state says 50, reality is 20, and no future plan re-flags it because state==config.

By contrast, the FLAVOR change (vCPU/RAM) on the same resource DOES apply in-place correctly.

**Implications / options:**
- Do not trust a root_disk_size bump alone to grow a live box. To actually grow the root disk you must resize the underlying volume out-of-band (VNG console/API) then `growpart /dev/sda 1 && resize2fs /dev/sda1`, or recreate the server.
- After discovering the drift, either revert the tfvars value to the real size (to stop the config lying) or annotate it, and consider attaching a separate data volume instead of growing root.

Seen on customer360 UAT 2026-09-10 resizing the backend/Dagster box s-general-2x4 -> s-general-4x8 (CPU/RAM took; disk stayed 20G).

## Related
[[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates|Renaming a Terraform for_each/map key needs terraform state mv or it destroys+recreates]]

## Related

- [[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates|Renaming a Terraform for_each/map key needs terraform state mv or it destroys+recreates]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vServer flavor resize (s-general-1x2 - 2x4) is an in-place terraform change (0 destroy) but reboots the box]]
- [[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates]]
- [[vDB volume_type cannot be changed on a live instance (no change-type API; not ForceNew so TF won't recreate)]]
- [[VNG vServer name is in-place updatable; decouple server name from the for_each map key to rename without recreate]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-end %%