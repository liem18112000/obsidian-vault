---
title: "RESTEASY003880 means the JAX-RS Providers thread-local is missing off-request-thread"
created: 2026-09-09
aliases: ["RESTEASY003880", "Unable to find contextual data of type javax.ws.rs.ext.Providers"]
type: concept
status: seedling
source: "luz-docs-batch incident 2026-09; LUZ-132205"
tags: [resteasy, jax-rs, wildfly, multipart, gotcha, kepler-luz]
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
