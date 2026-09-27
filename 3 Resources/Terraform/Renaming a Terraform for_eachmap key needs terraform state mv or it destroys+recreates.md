---
title: "Renaming a Terraform for_each/map key needs terraform state mv or it destroys+recreates"
created: 2026-09-10
type: lesson
status: seedling
source: "customer360 UAT 2026-09-10"
tags: [terraform, state, for-each, gotcha, infra]
---

# Renaming a Terraform for_each/map key needs terraform state mv or it destroys+recreates

A `for_each`/map-keyed Terraform resource is addressed by its key, e.g. `vngcloud_vserver_server.this["1x2"]`. Renaming the key in the tfvars (`"1x2"` -> `"backend"`) changes the resource ADDRESS, so Terraform plans a **destroy of the old + create of the new** -- for a live VM that means a rebuilt box (new IP, data loss), not a rename.

**Fix:** migrate the state so config and state addresses match, THEN the resize/attribute change plans as a clean in-place update:

```
terraform state mv vngcloud_vserver_server.this["1x2"] vngcloud_vserver_server.this["backend"]
# (data sources are re-read each plan; moving them too just avoids a transient)
```

`state mv` edits only Terraforms state file, not the live infra, and is reversible (mv back). After it, `terraform plan` showed `0 to add, 1 to change, 0 to destroy`. Declarative alternative: a `moved { from = ... to = ... }` block applied at plan time. Also: renaming a shared map key means updating every reference (deploy scripts default key, monitoring agent key lists, admin helpers), and doing the same `state mv` in each workspace (uat, prod) that uses the key. Seen on customer360 UAT 2026-09-10 (renamed backend server key "1x2" -> "backend" while resizing 2x4 -> 4x8).

## Related
[[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]

## Related

- [[Hung STARTED Dagster runs holding all max_concurrent_runs slots freeze the whole queue]]
