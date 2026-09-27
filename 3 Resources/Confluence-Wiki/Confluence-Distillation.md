---
ai_hash: 2c79c48a398cd18c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
entities: []
tags:
- moc
- confluence
- distillation
title: Confluence Distillation
type: moc
---

# Confluence Distillation

> [!abstract] 91 atomic notes distilled from 534 imported source pages
> Source pages live in [[Confluence-Wiki]] and are **reference**, not atomic.
> This note tracks what has been turned into transferable knowledge and what is still queued.

## Distilled notes

### Backend Patterns

- [[Claim work across pods with an expiring lease column on the row]]
- [[Client-assigned idempotency keys with a unique constraint beat distributed locks]]
- [[Deleting a shared reference entity prefer the design whose cost stays constant per consumer|Deleting a shared reference entity: prefer the design whose cost stays constant per consumer]]
- [[Dual-write to a new datastore via a composite implementation of the existing interface]]
- [[Enumerate a dependency's exception surface and decide each one before integrating]]
- [[If upstream holds memory until you ack, your write latency is their OOM risk]]
- [[In-memory job throttles silently break when you scale to multiple replicas]]
- [[Let the owning service hold the report field mapping as config, not the reporting service in code]]
- [[Long exports acknowledge immediately, deliver by emailed link to object storage|Long exports: acknowledge immediately, deliver by emailed link to object storage]]
- [[Multi-channel delivery filter for eligibility, then send in priority order|Multi-channel delivery: filter for eligibility, then send in priority order]]
- [[Outbound sync needs a durable per-record status table and a retry scan]]
- [[Persist raw third-party results before mapping them to your domain shape]]
- [[Piggybacking background jobs on HTTP requests couples job load to traffic]]
- [[Split a batch against a cache and forward only the misses, tracking the residual]]

### Security

- [[Authorization has four named parts PAP, PDP, PEP, PIP|Authorization has four named parts: PAP, PDP, PEP, PIP]]
- [[E2EE covers message content only; metadata and server-held keys narrow it further]]
- [[Envelope encryption with Vault transit keeps Vault off the data path]]
- [[Exchange a partner IdP token by introspecting it, never by trusting it]]
- [[Folder access rights stored on the folder or derived from its contents|Folder access rights: stored on the folder or derived from its contents]]
- [[Grade identity assurance by what was proven, not a verified boolean]]
- [[Keycloak action tokens bridge an app session into a browser login]]
- [[OIDC federation with just-in-time provisioning hinges on the attribute join key]]
- [[Per-tenant encryption keys make GDPR deletion a key destruction]]
- [[RBAC is coarse-grained by role, ABAC is fine-grained by attribute]]
- [[Resource-level consent grants specific instances, not just scopes]]
- [[Shared and personal accounts make attribution impossible by construction]]
- [[Two-grade JWTs solve the multi-tenant bootstrap basic token discovers tenants|Two-grade JWTs solve the multi-tenant bootstrap: basic token discovers tenants]]

### API Design

- [[404 addresses a missing resource; an empty filter result is a successful query]]
- [[Bulk operations need per-item outcomes, not one status code]]
- [[Expose intermediate results of an async job, not just the final output]]
- [[Exposing a GUI flow as an API means replacing everything the screen did for the user]]
- [[Mutually exclusive API parameters should be rejected, not resolved by precedence]]
- [[Optional API config enums for one toggle, a set or config object for more|Optional API config: enums for one toggle, a set or config object for more]]
- [[PATCH removes the read-modify-write round trips that PUT-replace forces]]
- [[Reject input you cannot fully handle; never silently drop part of it]]
- [[Score async API designs on crash recovery and multi-instance, not latency]]
- [[Serve a URL and its QR code as two representations of one endpoint]]
- [[Throw error codes not sentences; localise at the request boundary]]

### AI-LLM

- [[A coding-agent prompt needs codebase anchors and stated house style]]
- [[Changing the embedding model forces a full index rebuild]]
- [[Construct agent permissions per spawn instead of negotiating them per prompt]]
- [[Extract reusable skills automatically from settled agent exchanges]]
- [[Harvest training labels as a side effect of the user's own goal]]
- [[Manufacture structured disagreement when real model independence is unavailable]]
- [[Token input-output ratio fingerprints whether an LLM caller is an agent or a feature]]
- [[Version the whole retrieval pipeline, not just the model]]
- [[Wrap the agent CLI rather than reimplementing the agent loop]]

### Architecture

- [[Borrow the Kubernetes resource shape for objects in a schemaless store]]
- [[Container-per-customer silo multi-tenancy trades cost for structural isolation]]
- [[Draw the tenant boundary at legal data ownership, and make it reassignable]]
- [[Embedding a search library means building its control plane yourself]]
- [[Put a public API adapter between external callers and internal services]]
- [[Server push choice is decided by proxy idle timeouts and pod affinity, not API elegance]]
- [[Share features as vertical slices with app-owned routes and an injected adapter]]
- [[Strike what every option shares to find the real architecture decision]]

### Performance

- [[A ten-request JVM benchmark measures warm-up, not throughput]]
- [[For authenticated pages pick the perf tool that can log in, not the prettiest report]]
- [[Forecast launch load per user journey, and rate each for sheddability]]
- [[N+1 hides at the service-call layer too, not just in the ORM]]
- [[Profiled sub-steps never sum to wall clock; report the residual]]
- [[Split page load into server, render and interaction before optimising]]

### Infrastructure

- [[Carve .well-known out of any catch-all proxy or certificate validation fails|Carve /.well-known out of any catch-all proxy or certificate validation fails]]
- [[Timeouts stack in series and the shortest wins; audit the whole chain]]
- [[Two IaC surfaces need an explicit naming contract at the seam]]

### Redis

- [[Redis Standard replicas are failover only; read scaling needs Cluster]]
- [[Redis TTL should express liveness and be refreshed by a heartbeat]]
- [[Store pod-level facts once, not copied into every user key]]

### CI-CD

- [[Released artifact versions are immutable, so hotfixes iterate as snapshots]]
- [[Route user-edited business rules through git and CI instead of a database]]

### Code-Quality

- [[A linter with ignoreFailures is reporting, not gating; ratchet it instead]]
- [[Fallow analyses a JS-TS repo as a graph and exposes it to agents over MCP]]

### Decision-Making

- [[Fix evaluation criteria before looking at candidates, and say which one you weight]]
- [[Verify an existing flag's data quality before designing on top of it]]

### MongoDB

- [[JSON has no date type so type information dies at the API boundary]]
- [[Omit query stages built from empty lists; empty and absent mean opposite things]]

### Testing

- [[Playwright for UI E2E, k6 for load split by specialization not overlap|Playwright for UI E2E, k6 for load: split by specialization not overlap]]
- [[Test the decisions that override the story description, not the description]]

### Audit-Logging

- [[Hibernate Envers generates audit tables from an annotation]]

### Cloud

- [[GCP Data Access logs are off by default, so data-plane calls are unattributable]]

### Cloud Run

- [[Cloud Run generates a per-project URL hash, breaking environment config parity]]

### Concurrency

- [[Blocking on CompletableFuture.get in a custom pool recreates the bottleneck]]

### Databases

- [[Choosing a DB migration library SQL changesets, startup vs command, maintenance|Choosing a DB migration library: SQL changesets, startup vs command, maintenance]]

### Debugging

- [[A nullable column with no constraint becomes an NPE in a downstream service]]

### Event-Driven

- [[Event records should carry new state keyed by entity id and stamped with event time]]

### Kubernetes

- [[Shift GKE traffic to Cloud Run with Istio weighted routing, not a cutover]]

### NextJS

- [[Next.js monkey-patches global fetch, and its response clone races your body read]]

### NodeJS

- [[Undrained fetch response bodies leak sockets in Node undici]]

### Observability

- [[Record origin and origin_href so a downstream row traces back to its cause]]

### PostgreSQL

- [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]]

### Refactoring

- [[Tag deferred migration sites with a greppable marker unique to that upgrade]]

### Search

- [[Lucene's write-once segments turn replication into a filename diff]]

## Queue — highest-value sources not yet distilled

> [!tip] How to continue
> Read the source note, extract each transferable idea as its own atomic note, tag it `confluence-distilled`,
> set `source:` to the Confluence page, and link back to the source note. Then re-run the generator.

| Relevance | Source page | Topic | Space |
|--:|---|---|---|
| 0.98 | [[Research Design architecture concept for the service to generate the Generic Interface File|Research: Design architecture concept for the service to generate the Generic Interface File]] | Architecture | HACKA |
| 0.97 | [[Proposal eArchived architecture direction for ePost web 2|Proposal: eArchived architecture direction for ePost web 2]] | Architecture | Helios |
| 0.95 | [[Java (and ecosystem) deep dive - Sprint 79, 80|Java (and ecosystem) deep dive - Sprint [79, 80]]] | Programming | TS |
| 0.94 | [[Use KLARA Swagger UI for REST API]] | Programming | LUZ |
| 0.93 | [[Rest API Vat Clearing]] | Programming | LUZ |
| 0.93 | [[[luz-storage] - Design: Security (Encryption & Decryption) with Vault]] | Security | LUZ |
| 0.93 | [[[CROSS-TEST] [LUZ-142507] Implement Analyze API Integration (Phase 1) | Part 2]] | Programming | TS |
| 0.93 | [[One API load test with locust]] | Programming | HACKA |
| 0.92 | [[Proposal An LLM Council for ePost – multi-lens deliberation in Claude Code|Proposal: An LLM Council for ePost – multi-lens deliberation in Claude Code]] | AI-ML | TK |
| 0.91 | [[Swiss banker Mock-API]] | Programming | HACKA |
| 0.91 | [[04_50_Setup Swagger UI for Finnova Rest API]] | Programming | GRAVITY |
| 0.91 | [[import invoices via python script]] | Programming | X4 |
| 0.91 | [[Testing Tool Comparison Playwright vs. k6|Testing Tool Comparison: Playwright vs. k6]] | Testing | LUZ |
| 0.91 | [[Refactor Enricher process - update PATCH]] | Programming | LUZ |
| 0.91 | [[Ionic angular|Ionic/angular]] | Programming | Helios |
| 0.90 | [[CI CD (Google Cloud Build & Google Cloud Deploy)|CI/CD (Google Cloud Build & Google Cloud Deploy)]] | Infrastructure | IO |
| 0.90 | [[Concept Intermediate data storage]] | Architecture | LUZ |
| 0.90 | [[Analyse your source code (Copy)]] | Programming | TK |
| 0.89 | [[Optimus ePost myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis|Optimus: ePost/myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis]] | Programming | FUT |
| 0.89 | [[Swagger with api explorer]] | Programming | Arrow |
| 0.89 | [[How to debug java code in Ivy Designer]] | Programming | PT |
| 0.89 | [[Reporting - Java class configuration]] | Programming | LUZ |
| 0.89 | [[06 - How to test a Rest API with authorization]] | Programming | GRAVITY |
| 0.89 | [[Git source code and Jenkins]] | Programming | Helios |
| 0.89 | [[LUZ-110826 Public API - Widget subscription by activation code]] | Programming | TS |
| 0.89 | [[OpenAPI UI (API on SwaggerUI)]] | Programming | LUZ |
| 0.89 | [[REST API for deleting EXPENSES documents]] | Programming | LUZ |
| 0.89 | [[Script to add hibernate_sequence for new Klara customers|Script to add \"hibernate_sequence\" for new Klara customers.]] | Programming | NEXT |
| 0.89 | [[Using ML models from Python in Java]] | Programming | AI |
| 0.89 | [[Prompt Architecture Code Review|Prompt: Architecture Code Review]] | Programming | FUT |
| 0.88 | [[Public API eletter token (00.02.08.00)|Public API/ eletter token (00.02.08.00)]] | Programming | LUZ |
| 0.88 | [[Deploy luz-epc-redis-service on GCP]] | Infrastructure | Helios |
| 0.88 | [[Error handling for delete and undo]] | Programming | Helios |
| 0.88 | [[Steps to implement unread letters count]] | Programming | TP2020 |
| 0.88 | [[[Analytics] Analyze API call when accessing eArchive]] | Programming | TP2020 |
| 0.88 | [[await fetch() vs FetchBuilder ()|await fetch() vs FetchBuilder<>()]] | Programming | TS |
| 0.87 | [[LUZ-92314 - [AI Data Feed] [Migration issue] - Investigate the cache mechanism from Postgresql]] | Infrastructure | TS |
| 0.87 | [[Code Review (AI-First model)]] | Programming | HACKA |
| 0.87 | [[Invoice API Java Client]] | Programming | AI |
| 0.87 | [[OCR API Java Client]] | Programming | AI |

%% ai-graph-start %%

**Related notes:**
- [[Confluence Export — What I Learned]]
- [[3 Resources]]
- [[Prompt Architecture Code Review]]
- [[Prompt Performance Code Review]]
- [[Programming]]

%% ai-graph-end %%