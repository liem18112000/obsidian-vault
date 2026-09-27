---
ai_hash: c36c29dd331ef331
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-05
entities: []
source: c360 python code review 2026-09-05
status: seedling
tags:
- caching
- multi-tenancy
- security
- redis
- gotcha
title: Response caches must include the authenticated tenant in the key
type: lesson
---

# Response caches must include the authenticated tenant in the key

A response cache keyed only on request parameters sits *in front of* the database, so a cache HIT skips the DB (and therefore skips RLS) entirely. If the authenticated tenant is not part of the key, tenant A populates an entry and tenant B's identical request reads A's data.

Rule: fold the auth context (tenant id, and user id where results are user-scoped) into every cache key for tenant-scoped routes, or do not cache those routes. The tenant usually lives on the request/session context, not in the handler's kwargs, so a generic "cache by primitive kwargs" decorator will miss it.

## Related

- [[Postgres RLS should be defense-in-depth]]
- [[not the sole tenant boundary]]

%% ai-graph-start %%

**Related notes:**
- [[Postgres RLS should be defense-in-depth, not the sole tenant boundary]]
- [[Derive tenant identity from the verified token, never from request input]]
- [[Postgres session SET vs transaction-local set_config for RLS context]]
- [[Postgres RLS is silently bypassed by superuser connections]]
- [[Set Postgres RLS session GUC via the raw DBAPI connection, not Session.execute]]

%% ai-graph-end %%