---
ai_hash: 46fd64c7a0efd07b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
aliases:
- RESTEASY003880
- Unable to find contextual data of type javax.ws.rs.ext.Providers
created: 2026-09-09
entities: []
source: luz-docs-batch incident 2026-09; LUZ-132205
status: seedling
tags:
- resteasy
- jax-rs
- wildfly
- multipart
- gotcha
- kepler-luz
title: RESTEASY003880 means the JAX-RS Providers thread-local is missing off-request-thread
type: concept
---

# RESTEASY003880 means the JAX-RS Providers thread-local is missing off-request-thread

RESTEasy 4.x resolves the JAX-RS `Providers` (needed to find a `MessageBodyReader`) through a `@Context` proxy — `ContextParameterInjector$GenericDelegatingProxy` → `ResteasyContext.getContextData(Providers.class)` — which reads a **thread-local stack that is only pushed for the duration of the active JAX-RS request thread**.

So `InputPart.getBody(...)` / `getBodyAsString()` on a multipart part works fine *during* the request, but throws:

```
org.jboss.resteasy.spi.LoggableFailure: RESTEASY003880: Unable to find contextual data of type: javax.ws.rs.ext.Providers
```

whenever the part body is read **off the request thread** — inside an async `ManagedExecutorService` / `CompletableFuture` task, a `@ObservesAsync` CDI observer, a background/batch worker, or simply after the request context has been popped. The stack is empty on that thread, so the proxy has nothing to return.

**Tell it apart from other failures:** the message says *contextual data* (a context lookup), not a missing provider or a consumed stream. A stack top of `MultipartInputImpl$PartImpl.getBodyAsString` / `getBody` confirms it.

Seen in Kepler/Luz (WildFly, `resteasy-multipart-provider` 4.7.7.Final): luz-eletter (ticket LUZ-132205) and luz-docs-batch (Sept 2026, doc creation down on DEV/TEST/PROD).

The fix is a code-structure change, not a config toggle — see [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]].

## Related

- [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]]

%% ai-graph-start %%

**Related notes:**
- [[Read RESTEasy multipart parts eagerly in-request; never pass InputPart around]]
- [[luz-docs RESTEASY003880 UriInfo 500 regression traced to MaterializeRequestFilter firing async CDI events]]
- [[Context-propagating fireAsync before the resource method wipes JAX-RS @Context proxies (RESTEASY003880)]]
- [[ManagedExecutorService.execute loses CDI request context]]
- [[RESTEasy multipart repeated field name yields a List, get(0) silently drops extras]]

%% ai-graph-end %%