---
ai_hash: 3ff62485cd9bffa4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-17
entities: []
source: session 2026-08-17 leo-customer360 load_balancer deploy
status: seedling
tags:
- terraform
- vngcloud
- greennode
- load-balancer
- terminology
title: 'VNG Cloud vLB: Layer 4 = NLB, Layer 7 = ALB'
type: term
---

# VNG Cloud vLB: Layer 4 = NLB, Layer 7 = ALB

In VNG Cloud / GreenNode vLB, the load-balancer **type** is a two-value distinction that shows up under two different names depending on where you look:

- **`type = "Layer 4"`** in the `vngcloud_vlb_load_balancer` resource = a **Network Load Balancer (NLB)**; the packages data source reports this as **`lb_type = "L4"`**. Package names look like `NLB_Small`, `NLB_Medium`, `NLB_Large`.
- **`type = "Layer 7"`** = an **Application Load Balancer (ALB)**; `lb_type = "L7"`. Package names look like `ALB_*`.
- A package can also report `lb_type = "ALL"`, meaning it can back both L4 and L7.

So "create an NLB_Small load balancer" maps to `type = "Layer 4"` + `package_name = "NLB_Small"`. The resource `type` string is spelled with a space ("Layer 4", not "L4" or "Layer4"); the `L4`/`L7`/`ALL` codes only appear on the data-source `packages[].lb_type` attribute.

`scheme` is a separate axis: `"Internet"` (public IP) vs `"Internal"` (private-only within the VPC).

See [[vngcloud_vlb_load_balancer package_id needs a UUID resolved via vngcloud_vlb_lb_packages]].

## Related

- [[vngcloud_vlb_load_balancer package_id needs a UUID resolved via vngcloud_vlb_lb_packages]]

%% ai-graph-start %%

**Related notes:**
- [[vngcloud_vlb_load_balancer package_id needs a UUID resolved via vngcloud_vlb_lb_packages]]
- [[VNG vLB packages are per-AZ too pin the zoneId= package uuid (full lbp- prefix) or the create rejects it]]
- [[VNG vLB pool in-place update fails ('Stickiness cannot be specified for non-HTTP pools') — use terraform -replace]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-end %%