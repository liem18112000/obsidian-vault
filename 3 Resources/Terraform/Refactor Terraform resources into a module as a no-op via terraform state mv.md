---
ai_hash: 6f9d02d49d312242
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities: []
source: session 2026-08-31 test-agent tf unify
status: seedling
tags:
- terraform
- refactoring
- iac
- state-mv
- module
title: Refactor Terraform resources into a module as a no-op via terraform state mv
type: howto
---

# Refactor Terraform resources into a module as a no-op via terraform state mv

To DRY up repeated Terraform resources into a reusable module **without destroying/recreating live infrastructure**, move each existing resource in state to its new module address, then prove the refactor is inert:

1. Back up the state file first.
2. Write the module + the module calls; `terraform init` to register it, `terraform validate`.
3. For every resource, `terraform state mv '<old.addr>' 'module.<name>.<addr>'`. A resource with no `count` (addr `foo.bar`) moving into a module whose resource uses `count` becomes `module.x.foo.bar[0]` — the index is part of the new address.
4. `terraform plan` MUST report **"No changes"**. Any create/replace means an address wasn't moved or the module renders different config; any in-place update means an attribute mismatch to chase (see [[Cloud Run v2 env blocks are order-sensitive in Terraform]]).

No `apply` is needed for the refactor — moving blocks between files or into a module changes only addresses (and state), never the real resources. Only the module-nesting changes addresses; relocating a plain resource block between .tf files does not. Worked on the test-agent deployments: 4 Cloud Run services collapsed into one module, plan clean.

## Related

- [[Cloud Run v2 env blocks are order-sensitive in Terraform]]

%% ai-graph-start %%

**Related notes:**
- [[Cloud Run v2 env blocks are order-sensitive in Terraform]]
- [[Cloud Run v2 multi-container sidecar in Terraform]]
- [[Single-to-multi container Cloud Run update fails in-place; use terraform -replace]]
- [[Renaming a Terraform for_eachmap key needs terraform state mv or it destroys+recreates]]
- [[Deploy a unique image tag to force a Cloud Run rollout via terraform]]

%% ai-graph-end %%