---
ai_hash: e33ad009734468fc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-06
entities: []
source: session 2026-09-06 docs-chatbot UAT deploy
status: seedling
tags:
- terraform
- infra
- ops
- drift
- gotcha
title: Use terraform -target to land one change without reconciling unrelated drift
type: howto
---

# Use terraform -target to land one change without reconciling unrelated drift

When `terraform plan` on a live module shows changes you did NOT intend alongside the one you did (pre-existing config drift — e.g. VM name relabels the state hasn't caught up on), do NOT run a blanket `apply`: it sweeps in that unrelated drift. Instead scope the apply to just your resource with `-target=<address>`:

```bash
terraform plan   # read the plan: which address is the change you want?
terraform apply -target='aws_security_group_rule.extra["8000-10.0.0.5/32"]'
```

Quote the whole address in single quotes — resource addresses with map keys contain `["..."]` (brackets + double-quotes) the shell would otherwise mangle. Terraform still applies the target's dependencies, so it stays consistent. This is the safe way to add one firewall rule / one resource to a drifted module without a big reconciliation you didn't sign up for. Re-run a `-target` plan afterwards; "No changes" proves the resource is now in state.

Caveat: -target is meant for surgical fixes, not routine workflow — the drift you skipped is still there and someone must reconcile it later (or a future untargeted apply will). In LEO Customer360 the deploy wrapper exposes this as `TARGET=... ./deploy.sh <env> apply`. Relates to [[LEO Customer360 VNG topology: co-located services use localhost, cross-box hops need explicit extra_ingress]].

## Related

- [[LEO Customer360 VNG topology: co-located services use localhost]]
- [[cross-box hops need explicit extra_ingress]]

%% ai-graph-start %%

**Related notes:**
- [[LEO Customer360 VNG topology co-located services use localhost, cross-box hops need explicit extra_ingress]]
- [[Remote Terraform state needs no manual sync — bake creds + init into the deploy orchestrator to guarantee alignment]]
- [[leo-customer360 CD deploys app containers to vServers only, never the vDBvLBvStorage Terraform]]
- [[VNG vLB pool in-place update fails ('Stickiness cannot be specified for non-HTTP pools') — use terraform -replace]]
- [[CRLF in a tfvars user_data heredoc makes Terraform force-replace VNG vServers]]

%% ai-graph-end %%