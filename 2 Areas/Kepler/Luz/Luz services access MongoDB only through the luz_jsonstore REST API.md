---
ai_hash: 5441778b67a5b321
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-11
entities:
- Luz/KLARA microservices
- MongoDB
- luz_jsonstore REST API
- HTTP
- MicroProfile REST client interface
- MongoDBClient
- '@RegisterRestClient'
- javax.json JsonObjects
- Mongo driver
- Tenant isolation
- Auth
- Bearer token
- Tenant-id path segment
- Resilience
- '@Retry'
- ServiceUnavailableException
- JAX-RS Response
- 'Connection: close'
- luz_docs_statistic two-token model service-tenant vs per-tenant cache token
- Mongo operators
- Request body
- Connection leaks
- Service wrappers
- MongoDB connection
- Clients
- Data access
source: luz_docs_statistic repo analysis, session 2026-06-11
status: seedling
tags:
- luz
- mongodb
- jsonstore
- architecture
- microprofile
title: Luz services access MongoDB only through the luz_jsonstore REST API
type: concept
---

# Luz services access MongoDB only through the luz_jsonstore REST API

Luz/KLARA microservices never open a direct MongoDB connection. Every read/write/aggregate goes over HTTP to the luz_jsonstore service (`/luz_jsonstore/api/mdb/{tenant-id}/{collection}`) through a MicroProfile REST client interface (e.g. `MongoDBClient` with `@RegisterRestClient(configKey = "LUZ_JSONSTORE_MDB_HOST_PORT")`).

Consequences:
- Mongo operators ($set, $facet, $match…) are built as `javax.json` JsonObjects and shipped as the request body — there is no Mongo driver in the service.
- Tenant isolation and auth are enforced by jsonstore via the bearer token + tenant-id path segment, so the *token you use determines whose data you touch* (see [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]]).
- Resilience is done client-side, e.g. `@Retry(retryOn = ServiceUnavailableException.class)` around each call.
- Service wrappers must close the JAX-RS `Response` (try-with-resources) to avoid connection leaks; clients send `Connection: close`.

## Related

- [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_statistic two-token model service-tenant vs per-tenant cache token]]
- [[Run local luz-jsonstore against a real tenant GKE Mongo via port-forwards]]
- [[Cache one MongoClient per tenant and close it on eviction]]
- [[Luz performance env cluster topology]]
- [[01 Overview]]

**Relations:**
- Luz/KLARA microservices — *access* — MongoDB
- Luz/KLARA microservices — *accesses via* — luz_jsonstore REST API
- Luz/KLARA microservices — *do not open direct* — MongoDB connection
- Luz/KLARA microservices — *send operations over* — HTTP
- Luz/KLARA microservices — *use* — MicroProfile REST client interface
- MicroProfile REST client interface — *example* — MongoDBClient
- MongoDBClient — *uses annotation* — @RegisterRestClient
- Mongo operators — *are built as* — javax.json JsonObjects
- Mongo operators — *are shipped as* — Request body
- Luz/KLARA microservices — *do not contain* — Mongo driver
- luz_jsonstore REST API — *enforces* — Tenant isolation
- luz_jsonstore REST API — *enforces* — Auth
- Bearer token — *determines* — Data access
- Tenant-id path segment — *determines* — Data access
- Resilience — *is handled client-side* — Luz/KLARA microservices
- @Retry — *is used for* — Resilience
- @Retry — *handles* — ServiceUnavailableException
- Service wrappers — *must close* — JAX-RS Response
- Closing JAX-RS Response — *prevents* — Connection leaks
- Clients — *send header* — Connection: close
- luz_jsonstore REST API — *is related to* — luz_docs_statistic two-token model service-tenant vs per-tenant cache token
- Bearer token — *is related to* — luz_docs_statistic two-token model service-tenant vs per-tenant cache token
- Tenant-id path segment — *is related to* — luz_docs_statistic two-token model service-tenant vs per-tenant cache token

%% ai-graph-end %%