---
ai_hash: 39be875b593dc234
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-09
entities: []
source: session 2026-09-09 (dagster-scaling-uat-vserver.md)
status: seedling
tags:
- vng-cloud
- terraform
- infra
- gotcha
- leo-customer360
title: VNG Cloud HCM03-1C offers only the s-general flavor family
type: lesson
---

# VNG Cloud HCM03-1C offers only the s-general flavor family

The VNG Cloud availability zone **HCM03-1C** exposes only the `s-general-*` VM flavor family. The `s2-general-*` flavors (the ones with the published rate card, ~283,800 VND per 1 vCPU+2 GB block, and a memory-optimized 1:4 option) live in a **different, sold-out AZ (HCM03-1A)**. You cannot provision them for this account.

**Why it bites:** name-based Terraform lookups (flavor_zone_name, image_name, volume_type_zone_name) resolve to the sold-out 1A zone and fail. So `deployments/server/overlays/uat.tfvars` pins `flavor_zone_id`, `image_id`, and `root_disk_type_id` **directly** to the HCM03-1C values instead.

**Implication for capacity planning:** any Dagster / vServer sizing for this account must use `s-general` flavors (strict 1:2 vCPU:RAM) — there is **no memory-optimized 1:4 flavor**, so a `4 vCPU / 16 GB` (1:4) workload provisions 8 vCPU to get 16 GB and bills as 8 blocks, not 4 (a "memory tax"). Do not quote the `s2-general` rate card as if it were available here.

See [[UAT vServer Dagster topology split webserver+daemon on one s-general box|UAT vServer Dagster topology: split webserver+daemon on one s-general box]].

## Related

- [[UAT vServer Dagster topology split webserver+daemon on one s-general box|UAT vServer Dagster topology: split webserver+daemon on one s-general box]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vServer flavor zones are per-AZ under one shared name; the flavor's flavorZoneId field IS the AZ]]
- [[VNG vServer OS images are not associated with the s2-general flavor zone (image data-source trap)]]
- [[VNG vServer default catalog endpoints return the DISABLED default AZ; use zoneId=AZ and pin UUIDs]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]
- [[GreenNode vDB package family + IOPS tier are per-zone (HCM03-1A vs 1C)]]

%% ai-graph-end %%