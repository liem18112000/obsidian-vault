---
ai_hash: 26d7499633e6fdd7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-19
entities: []
source: leo-customer360 load_balancer, 2026-08
status: seedling
tags:
- terraform
- for_each
- load-balancer
- gotcha
- vngcloud
title: Terraform for_each rename reusing a unique port can transiently clash on apply
type: lesson
---

# Terraform for_each rename reusing a unique port can transiently clash on apply

When a Terraform `for_each` resource is keyed by a name and you RENAME the key (e.g. LB listener `"api"` -> `"caddy_http"`) while it keeps a **globally-unique** attribute like a listen port, the plan is destroy-old + create-new as two independent instances. Terraform does not guarantee destroy-before-create across different instances, so the provider can try to create the new listener on `:80` while the old one still holds `:80` -> API rejects it ("port ... in use").

**Fix / workaround:** simply re-run `apply` — the first (partial) run destroys the old instance, the second creates the new one cleanly. Or force ordering with `create_before_destroy=false` isn't enough here (different instances); a `-target` two-step or a lifecycle `replace_triggered_by` on a shared dependency can serialize it. New ports (e.g. `:443`) never conflict, only the reused one.

Related: [[Caddy handle_path strips the path prefix, handle keeps it]]

%% ai-graph-start %%

**Related notes:**
- [[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates]]
- [[Single-to-multi container Cloud Run update fails in-place; use terraform -replace]]
- [[VNG vLB replace the listener when its pool is ForceNew-replaced]]
- [[VNG vServer name is in-place updatable; decouple server name from the for_each map key to rename without recreate]]
- [[VNG vLB pool in-place update fails ('Stickiness cannot be specified for non-HTTP pools') — use terraform -replace]]

%% ai-graph-end %%