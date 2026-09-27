---
title: "VNG Cloud HCM03-1C offers only the s-general flavor family"
created: 2026-09-09
type: lesson
status: seedling
source: "session 2026-09-09 (dagster-scaling-uat-vserver.md)"
tags: [vng-cloud, terraform, infra, gotcha, leo-customer360]
---

# VNG Cloud HCM03-1C offers only the s-general flavor family

The VNG Cloud availability zone **HCM03-1C** exposes only the `s-general-*` VM flavor family. The `s2-general-*` flavors (the ones with the published rate card, ~283,800 VND per 1 vCPU+2 GB block, and a memory-optimized 1:4 option) live in a **different, sold-out AZ (HCM03-1A)**. You cannot provision them for this account.

**Why it bites:** name-based Terraform lookups (flavor_zone_name, image_name, volume_type_zone_name) resolve to the sold-out 1A zone and fail. So `deployments/server/overlays/uat.tfvars` pins `flavor_zone_id`, `image_id`, and `root_disk_type_id` **directly** to the HCM03-1C values instead.

**Implication for capacity planning:** any Dagster / vServer sizing for this account must use `s-general` flavors (strict 1:2 vCPU:RAM) — there is **no memory-optimized 1:4 flavor**, so a `4 vCPU / 16 GB` (1:4) workload provisions 8 vCPU to get 16 GB and bills as 8 blocks, not 4 (a "memory tax"). Do not quote the `s2-general` rate card as if it were available here.

See [[UAT vServer Dagster topology: split webserver+daemon on one s-general box]].

## Related

- [[UAT vServer Dagster topology: split webserver+daemon on one s-general box]]
