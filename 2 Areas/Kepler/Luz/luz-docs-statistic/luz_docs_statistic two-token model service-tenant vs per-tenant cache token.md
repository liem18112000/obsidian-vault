---
ai_hash: 51a57a2caf178fda
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities:
- luz_docs_statistic
- two-token model
- Service-tenant token
- Per-tenant token
- client_credentials
- luzsec
- JwtService
- documentstatistics collection
- luz_cache
- luz_docs_api_<tenantId>
- luz_docs
- $facet aggregation
- documents collection
- jsonstore
- tenant
- MongoDB
- luz_jsonstore REST API
- 1-minute EJB timer
- PubSub
- distributed cache
- Luz services
- service tenant
- impersonation
source: luz_docs_statistic repo analysis, session 2026-06-11
status: seedling
tags:
- luz
- jwt
- luz-cache
- security
- luz-docs-statistic
title: 'luz_docs_statistic two-token model: service-tenant vs per-tenant cache token'
type: model
---

# luz_docs_statistic two-token model: service-tenant vs per-tenant cache token

luz_docs_statistic uses two different bearer tokens depending on *whose* data it touches:

1. **Service-tenant token** — obtained via client_credentials from luzsec, cached as a static singleton in `JwtService` and refreshed on expiry. Used for everything under the service tenant: reads/writes on the `documentstatistics` collection and calls to luz_cache.
2. **Per-tenant token** — fetched from luz_cache under key `luz_docs_api_<tenantId>` (placed there earlier by luz_docs). Used to run the $facet aggregation on that tenant's own `documents` collection, because jsonstore scopes data access by the token's tenant.

Gotcha: if the cache entry is missing/expired, that tenant's statistic update fails for the round (the aggregate call can't authenticate as the tenant). The cache key is deleted in a `finally` after each tenant is processed, so it is effectively a one-shot handoff from luz_docs.

Why: the service tenant has no right to read tenant documents directly; impersonation is done by borrowing the tenant token that luz_docs parked in the distributed cache. See [[Luz services access MongoDB only through the luz_jsonstore REST API]].

## Related

- [[Luz services access MongoDB only through the luz_jsonstore REST API]]
- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_statistic updates stats via 1-minute EJB timer over PubSub and $facet aggregation]]
- [[Luz services access MongoDB only through the luz_jsonstore REST API]]
- [[luz-docs-statistic-get-latest-endpoint]]
- [[totalFolders needs a second aggregate because a $facet pipeline is bound to one collection]]
- [[New architecture for documentStatistic]]

**Relations:**
- luz_docs_statistic — *uses* — two-token model
- two-token model — *comprises* — Service-tenant token
- two-token model — *comprises* — Per-tenant token
- Service-tenant token — *obtained_via* — client_credentials
- client_credentials — *from* — luzsec
- Service-tenant token — *cached_in* — JwtService
- Service-tenant token — *used_for* — documentstatistics collection
- Service-tenant token — *used_for* — luz_cache
- Per-tenant token — *fetched_from* — luz_cache
- Per-tenant token — *identified_by_key* — luz_docs_api_<tenantId>
- luz_docs — *places* — Per-tenant token
- Per-tenant token — *used_for* — $facet aggregation
- $facet aggregation — *on* — documents collection
- jsonstore — *scopes_data_access_by* — tenant
- luz_docs_statistic — *updates_stats_via* — 1-minute EJB timer
- luz_docs_statistic — *updates_stats_via* — PubSub
- luz_docs_statistic — *updates_stats_via* — $facet aggregation
- Luz services — *access* — MongoDB
- Luz services — *access_via* — luz_jsonstore REST API
- luz_jsonstore REST API — *provides_access_to* — MongoDB
- service tenant — *has_no_right_to_read* — documents collection
- Per-tenant token — *enables* — impersonation
- Per-tenant token — *parked_in* — distributed cache
- Per-tenant token — *parked_by* — luz_docs
- luz_cache — *missing_or_expired_entry_causes* — statistic update fails
- luz_cache — *key_deleted_after_processing* — tenant
- luz_docs_statistic — *interacts_with* — luz_cache
- luz_docs_statistic — *interacts_with* — jsonstore
- luz_docs_statistic — *processes* — tenant
- luz_docs — *interacts_with* — luz_cache
- luz_docs — *provides* — Per-tenant token

%% ai-graph-end %%