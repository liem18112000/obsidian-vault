---
ai_hash: 5ceee707825495f4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-16
entities: []
source: session 2026-08-16 prod backup wiring
status: seedling
tags:
- terraform
- iac
- gotcha
- vngcloud
title: 'Terraform Optional+Computed attribute: default the variable to null to leave
  it as-is'
type: lesson
---

# Terraform Optional+Computed attribute: default the variable to null to leave it as-is

When a Terraform provider marks an argument **Optional + Computed** (e.g. `backup_policy_id` / `backup_location_id` on `vngcloud_vdb_postgresql_cluster`), the provider fills in a value if you omit it, and does not reconcile drift afterward.

To wire such an argument through module layers cleanly, default the pass-through variable to **`null`**, not `""`. Passing `null` makes Terraform treat the argument as *unset*, so the provider keeps whatever value it computed / the console assigned. Passing an **empty string** is a real value and can attempt to CLEAR the attribute, producing spurious diffs or unintended changes.

This is also how you conditionally omit a **top-level** argument: `dynamic` blocks only work for nested blocks, so for a scalar arg you set it to a variable that is `null` when you want it absent. Pattern: `variable "x" { type = string; default = null }` then `x = var.x` in the resource.

Used to thread VNG Backup Center IDs into the LEO CDP prod PG cluster — see [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]].

## Related

- [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]]

%% ai-graph-start %%

**Related notes:**
- [[VNG vDB PostgreSQL cluster has no Terraform backup args (standalone-only)]]
- [[vngcloud_vdb_postgresql_cluster ignores backup_auto (cluster backups go via VNG Backup Center)]]
- [[vngcloud vDB packagevolume data source returns empty id on no-match (guard with a precondition)]]
- [[Terraform optional-resource toggle create-or-reuse via count + a local that picks the id]]
- [[VNG Cloud vServer Terraform catalog ids resolve via a zone-UUID lookup chain]]

%% ai-graph-end %%