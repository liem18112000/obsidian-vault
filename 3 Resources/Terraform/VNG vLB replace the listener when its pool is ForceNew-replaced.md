---
ai_hash: efdad42f96af50cf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 load_balancer 2026-08-19
status: seedling
tags:
- terraform
- vngcloud
- load-balancer
- replace_triggered_by
- forcenew
- gotcha
title: 'VNG vLB: replace the listener when its pool is ForceNew-replaced'
type: lesson
---

# VNG vLB: replace the listener when its pool is ForceNew-replaced

On VNG Cloud vLB (and similar L4 LBs), changing a pool attribute that is ForceNew
(health_check_protocol HTTP<->TCP, member port, protocol) makes Terraform REPLACE the
pool. But the API rejects deleting a pool still referenced by a listener
(default_pool_id) -> `error deleting Pool ...: Pool id ... is used in listener`.
Terraforms default replace order destroys the OLD pool first (before updating the
listener), so it fails.

Fix: force the listener to be replaced whenever its pool is replaced, so Terraform
destroys the listener FIRST (dependents are destroyed before dependencies), which frees
the pool. On the listener resource:

    lifecycle {
      replace_triggered_by = [vngcloud_vlb_pool.this[each.key].id]
    }

(each.key works inside a for_each listener; .id changes only on pool replacement, so it
triggers precisely then.) create_before_destroy on the pool is NOT a clean alternative
here because pool names must be unique per LB, so a same-named second pool would clash.

Discovered in leo-customer360 deployments/load_balancer when a backend switched health
checks HTTP->TCP. Related: [[Per-AZ catalog trap on VNG Cloud Terraform provider]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vLB pool in-place update fails ('Stickiness cannot be specified for non-HTTP pools') — use terraform -replace]]
- [[Recovering an orphaned VNG vLB resource after a Terraform state-save failure]]
- [[VNG vServer name is in-place updatable; decouple server name from the for_each map key to rename without recreate]]
- [[Terraform for_each rename reusing a unique port can transiently clash on apply]]
- [[vngcloud_vlb_load_balancer package_id needs a UUID resolved via vngcloud_vlb_lb_packages]]

%% ai-graph-end %%