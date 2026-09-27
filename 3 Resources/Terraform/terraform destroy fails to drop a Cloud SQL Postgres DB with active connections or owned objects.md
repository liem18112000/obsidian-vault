---
ai_hash: 699a36d0bbe682cc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08 test-agent-v1 teardown
status: seedling
tags:
- terraform
- gcp
- cloud-sql
- postgres
- destroy
- gotcha
title: terraform destroy fails to drop a Cloud SQL Postgres DB with active connections
  or owned objects
type: lesson
---

# terraform destroy fails to drop a Cloud SQL Postgres DB with active connections or owned objects

When a Terraform stack models a Cloud SQL Postgres instance as THREE resources — `google_sql_database_instance`, `google_sql_database`, and `google_sql_user` — a `terraform destroy` deletes the database and user FIRST (via SQL `DROP DATABASE` / `DROP ROLE`), then the instance. Those two SQL drops fail when:

- the database "is being accessed by other users" (any live connection — e.g. a Cloud Run service whose connections have not fully drained yet), and
- the role "cannot be dropped because N objects depend on it" (the login role owns tables/objects in the DB).

Result: `terraform destroy` exits 1 with the instance left behind, even though it already removed everything else (services, buckets, secrets, SA).

Fix: delete the INSTANCE directly — `gcloud sql instances delete <name> --project <p> --quiet` — which cascades away the database, user, and all data in one operation, no per-object DROP needed. (Then the leftover terraform state entries for the db/user/instance are moot since the whole stack is being decommissioned; `terraform state rm` them if you keep the state.)

Verified 2026-09-08 tearing down test-agent-v1 (instance kga-taskstore) in klara-nonprod.

%% ai-graph-start %%

**Related notes:**
- [[Stale terraform state != live cloud — verify with gcloud before deleting a retired deployment]]
- [[Cloud Run v2 deletion_protection defaults true — set false and apply before destroy]]
- [[Wipe test-agent-v2 memory and taskstore via test-agent-v2tools]]
- [[Cloud SQL Postgres point-in-time recovery requires automated backups enabled]]
- [[Managed DB provisioners create the server but not in-database objects]]

%% ai-graph-end %%