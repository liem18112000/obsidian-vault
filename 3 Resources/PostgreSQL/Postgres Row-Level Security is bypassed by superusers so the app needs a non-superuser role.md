---
ai_hash: edd9cab53dda9315
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-03
entities: []
source: session 2026-08-03
status: seedling
tags:
- postgresql
- rls
- security
- multitenancy
- terraform
- gotcha
title: Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser
  role
type: lesson
---

# Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser role

PostgreSQL **Row-Level Security is silently bypassed for superusers** (and for the table owner unless `FORCE ROW LEVEL SECURITY` is set). So an app that connects as the `postgres` superuser gets **zero tenant isolation** even with RLS policies enabled — a dangerous false sense of security. The app must connect as a **dedicated non-superuser LOGIN role** (e.g. `customer360_app`) granted schema `USAGE` + table CRUD (and `ALTER DEFAULT PRIVILEGES` for future tables).

**Corollary for managed databases / IaC:** a managed-DB Terraform provider provisions the *server* but not what is *inside* it. Extensions, the RLS app role, schema DDL, and seed data all need a **separate, idempotent post-provision migration step** — `CREATE EXTENSION IF NOT EXISTS`, a `DO`-block that creates the role only `IF NOT EXISTS (SELECT FROM pg_roles ...)`, and `ON CONFLICT` seeds. Run it from CI/CD or a Kubernetes Job (or a gated `null_resource` local-exec), not from the DB resource itself.

Note: `psql` variable substitution (`:'var'`) does **not** expand inside dollar-quoted `$$ ... $$` blocks, so render role/identifier values into the SQL at template time instead.

## Related
- [[vDB PostgreSQL supports PostGIS and pgvector plus the fuzzy-match extensions]]
- [[LEO Customer360 GreenNode Terraform infrastructure]]

%% ai-graph-start %%

**Related notes:**
- [[Postgres RLS is silently bypassed by superuser connections]]
- [[FORCE RLS breaks seeding as a non-superuser unless app.tenant_id is set]]
- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]
- [[Managed DB provisioners create the server but not in-database objects]]
- [[Inject a Postgres session GUC into an unmodifiable script via PGOPTIONS]]

%% ai-graph-end %%