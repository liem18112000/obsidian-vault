---
ai_hash: 399541308cacd93f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: c360 python code review 2026-09-05
status: seedling
tags:
- authorization
- idor
- multi-tenancy
- security
title: Derive tenant identity from the verified token, never from request input
type: lesson
---

# Derive tenant identity from the verified token, never from request input

Taking the tenant (or user) identity from request input, a query param, request body, or header, is an IDOR: the caller can name someone else's tenant. This holds even when it is "only for dev mode", because dev defaults ship.

Always derive the security principal (tenant_id, user_id, roles) from the verified token/session. If an endpoint also accepts a tenant argument (e.g. for filtering), compare it to the token's tenant and reject mismatches (403). Never pass a request-supplied tenant into the RLS session variable.

## Related

- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]

%% ai-graph-start %%

**Related notes:**
- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]
- [[Response caches must include the authenticated tenant in the key]]
- [[Postgres session SET vs transaction-local set_config for RLS context]]
- [[Postgres RLS is silently bypassed by superuser connections]]
- [[Postgres Row-Level Security is bypassed by superusers so the app needs a non-superuser role]]

%% ai-graph-end %%