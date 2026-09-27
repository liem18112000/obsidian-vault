---
ai_hash: d21dba119aae4db9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-31
entities: []
source: session 2026-08-31
status: seedling
tags:
- terraform
- cloud-run
- gcp
- gotcha
- dynamic-blocks
title: Cloud Run v2 env blocks are order-sensitive in Terraform
type: lesson
---

# Cloud Run v2 env blocks are order-sensitive in Terraform

In `google_cloud_run_v2_service`, container `env` is an **ordered list of blocks**, so Terraform diffs it by position — reordering the same env vars shows as a change and breaks a "no-op" refactor.

When replacing inline `env {}` blocks with a module that renders them via `dynamic "env"`, the rendered ORDER must match what is already in state. Gotchas:
- `dynamic "env" { for_each = <map> }` emits in **sorted key order**, not definition order. If the original used a map, replicate that alphabetical order.
- Pass env as an **ordered `list(object(...))`** (with `optional()` for the literal-vs-secret split) so the caller controls exact order, rather than a map.
- Only emit optional attributes (`cpu_idle`, `startup_cpu_boost`, `timeout`, `session_affinity`) when they were actually set originally — pass `null` otherwise so Terraform leaves them computed and no phantom diff appears.

Get all this right and `terraform plan` = No changes. See [[Refactor Terraform resources into a module as a no-op via terraform state mv]].

## Related

- [[Refactor Terraform resources into a module as a no-op via terraform state mv]]

%% ai-graph-start %%

**Related notes:**
- [[Refactor Terraform resources into a module as a no-op via terraform state mv]]
- [[Terraform-managed Cloud Run set env flags in TF, not gcloud run update]]
- [[Single-to-multi container Cloud Run update fails in-place; use terraform -replace]]
- [[Cloud Run v2 multi-container sidecar in Terraform]]
- [[Cloud Run won't redeploy on a latest digest change — apply by immutable digest]]

%% ai-graph-end %%