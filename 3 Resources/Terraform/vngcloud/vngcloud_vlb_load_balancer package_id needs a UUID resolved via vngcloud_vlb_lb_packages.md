---
ai_hash: 0fc2aff01cb4fe0c
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
- gotcha
title: vngcloud_vlb_load_balancer package_id needs a UUID resolved via vngcloud_vlb_lb_packages
type: lesson
---

# vngcloud_vlb_load_balancer package_id needs a UUID resolved via vngcloud_vlb_lb_packages

The `vngcloud_vlb_load_balancer` Terraform resource wants `package_id` as the package **UUID**, not the console display name like `NLB_Small`. There is a data source, `vngcloud_vlb_lb_packages`, but it takes only `project_id` and returns **every** package for the project as a `packages` list of `{uuid, name, package_type, lb_type, connection_number, data_transfer, mode}` — it has **no name filter**. So you match your package by `name` yourself in a Terraform `local` and read its `.uuid`.

```hcl
data "vngcloud_vlb_lb_packages" "all" { project_id = var.project_id }

locals {
  matched       = [for p in data.vngcloud_vlb_lb_packages.all.packages : p if p.name == var.package_name]
  lb_package_id = length(local.matched) > 0 ? local.matched[0].uuid : ""
}
```

**Gotcha:** a typo in `package_name` yields no match and an **empty** `package_id`, which surfaces later as a confusing provider-side error rather than a clear "not found". Guard it with a `lifecycle { precondition { condition = local.lb_package_id != "" ... } }` so it fails early with an actionable message. (Same shape as the vDB deploy, where the package/volume data sources return an empty id on no-match.)

Exported LB attributes worth wiring to outputs: `id`, `status`, `address` (its IP), `private_subnet_cidr`. See [[VNG Cloud vLB Layer 4 = NLB, Layer 7 = ALB|VNG Cloud vLB: Layer 4 = NLB, Layer 7 = ALB]].

## Related

- [[VNG Cloud vLB Layer 4 = NLB, Layer 7 = ALB]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vLB packages are per-AZ too pin the zoneId= package uuid (full lbp- prefix) or the create rejects it]]
- [[VNG Cloud vLB Layer 4 = NLB, Layer 7 = ALB]]
- [[vngcloud vDB packagevolume data source returns empty id on no-match (guard with a precondition)]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]
- [[VNG vLB replace the listener when its pool is ForceNew-replaced]]

%% ai-graph-end %%