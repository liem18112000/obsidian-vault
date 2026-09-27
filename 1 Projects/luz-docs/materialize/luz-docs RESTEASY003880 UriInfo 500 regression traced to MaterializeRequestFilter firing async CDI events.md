---
ai_hash: 7a8968c146427596
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-13
entities:
- luz-docs
- RESTEASY003880
- UriInfo
- HTTP 500
- MaterializeRequestFilter
- async CDI events
- regression
- dev integration-test run
- POST /documents
- POST /folders
- org.jboss.resteasy.spi.LoggableFailure
- javax.ws.rs.core.UriInfo
- javax.ws.rs.ext.Providers
- materialize-pattern merge
- ContainerRequestFilter
- resource method
- MaterializeFolderParentChangeService
- MaterializeFolderRenameService
- retryEvent.fireAsync
- RESTEasy's thread-local context
- ContextParameterInjector$GenericDelegatingProxy
- Providers
- DocumentUtil.getMetadata
- request thread
- SynchronousDispatcher
- ContainerResponseFilter
- RESTEasy context
- MP context propagation
- test repo
- server fix
- JAX-RS @Context proxies
- Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies
  (RESTEASY003880)
- luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata
  AttributeError
- base_metadata AttributeError
source: dev IT run 2026-06-12 + server log analysis
status: seedling
tags:
- luz-docs
- materialize
- resteasy
- sprint-158
- regression
title: luz-docs RESTEASY003880 UriInfo 500 regression traced to MaterializeRequestFilter
  firing async CDI events
type: observation
---

# luz-docs RESTEASY003880 UriInfo 500 regression traced to MaterializeRequestFilter firing async CDI events

On the 2026-06-12 dev integration-test run, the dominant failure cluster (~10-14 scenarios) was `POST /documents` and `POST /folders` intermittently returning HTTP 500 with `org.jboss.resteasy.spi.LoggableFailure: RESTEASY003880: Unable to find contextual data of type: javax.ws.rs.core.UriInfo` (and a sibling `...type: javax.ws.rs.ext.Providers`).

Root cause (server-side, luz_docs, introduced by the materialize-pattern merge): `MaterializeRequestFilter` is a `@RequestScoped @Provider ContainerRequestFilter` that runs on the request thread *before* the resource method, on every materialize-allowlisted tenant. It calls `MaterializeFolderParentChangeService.onRetry()` / `MaterializeFolderRenameService.onRetry()`, which do `retryEvent.fireAsync(event, NotificationOptions.ofExecutor(managedExecutorService))`. Firing that context-propagating async CDI event perturbs RESTEasys thread-local context, so when the resource method then calls `uriInfo.getAbsolutePathBuilder()` (`FolderResource.createFolder:95`, `DocumentResource.createDocument:275`) the lazy `@Context UriInfo` proxy (`ContextParameterInjector$GenericDelegatingProxy`) finds no context and throws RESTEASY003880. Same mechanism breaks `Providers` during body (de)serialization in `DocumentUtil.getMetadata`.

Confirmed from the server stack trace: the failure is on the normal `default task-N` request thread via `SynchronousDispatcher` — NOT an async/FaultTolerance thread — which is what proves the request-thread context was wiped mid-request rather than the work simply running off-thread.

Intermittent because the filter only fires for allowlisted tenants and the async dispatch races the request thread.

Fix lives in luz_docs (server), not the integration-test repo — e.g. move the retry seeding to a ContainerResponseFilter (after the resource method consumed UriInfo), snapshot/restore the RESTEasy context around the fireAsync, or decouple the seeding from MP context propagation. As of this session the user chose to fix the test repo only, so the server fix is pending.

General principle: [[Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies (RESTEASY003880)]]. Downstream test symptom: [[luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata AttributeError]].

## Related

- [[Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies (RESTEASY003880)]]
- [[luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata AttributeError]]

%% ai-graph-start %%

**Related notes:**
- [[Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies (RESTEASY003880)]]
- [[luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata AttributeError]]
- [[RESTEASY003880 means the JAX-RS Providers thread-local is missing off-request-thread]]
- [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]]
- [[ManagedExecutorService.execute loses CDI request context]]

**Relations:**
- luz-docs — *experiences* — regression
- regression — *is* — RESTEASY003880
- regression — *involves* — UriInfo
- regression — *results in* — HTTP 500
- regression — *traced to* — MaterializeRequestFilter
- MaterializeRequestFilter — *fires* — async CDI events
- dev integration-test run — *observed* — HTTP 500
- HTTP 500 — *affects* — POST /documents
- HTTP 500 — *affects* — POST /folders
- HTTP 500 — *accompanied by* — org.jboss.resteasy.spi.LoggableFailure
- org.jboss.resteasy.spi.LoggableFailure — *cites* — RESTEASY003880
- RESTEASY003880 — *mentions* — javax.ws.rs.core.UriInfo
- RESTEASY003880 — *mentions* — javax.ws.rs.ext.Providers
- MaterializeRequestFilter — *introduced by* — materialize-pattern merge
- MaterializeRequestFilter — *is a* — ContainerRequestFilter
- MaterializeRequestFilter — *runs before* — resource method
- MaterializeRequestFilter — *calls* — MaterializeFolderParentChangeService
- MaterializeRequestFilter — *calls* — MaterializeFolderRenameService
- MaterializeFolderParentChangeService — *performs* — retryEvent.fireAsync
- MaterializeFolderRenameService — *performs* — retryEvent.fireAsync
- retryEvent.fireAsync — *perturbs* — RESTEasy's thread-local context
- resource method — *calls* — UriInfo
- UriInfo — *is a* — ContextParameterInjector$GenericDelegatingProxy
- ContextParameterInjector$GenericDelegatingProxy — *throws* — RESTEASY003880
- async CDI events — *break* — Providers
- Providers — *used in* — DocumentUtil.getMetadata
- failure — *occurs on* — request thread
- request thread — *via* — SynchronousDispatcher
- request thread — *context wiped by* — async CDI events
- regression — *is intermittent due to* — async dispatch races request thread
- luz-docs — *requires* — server fix
- server fix — *option* — ContainerResponseFilter
- server fix — *option* — snapshot/restore RESTEasy context
- server fix — *option* — decouple seeding from MP context propagation
- user — *fixed* — test repo
- server fix — *is* — pending
- Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies (RESTEASY003880) — *is a* — General principle
- General principle — *explains* — RESTEASY003880
- luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata AttributeError — *is a* — Downstream test symptom
- Downstream test symptom — *results from* — HTTP 500
- luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata AttributeError — *surfaces as* — base_metadata AttributeError
- Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies (RESTEASY003880) — *related to* — luz-docs IT create-document step swallows server 500 and surfaces as cryptic base_metadata AttributeError

%% ai-graph-end %%