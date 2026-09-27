---
ai_hash: e53f0e9b14bd0de9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24
status: seedling
tags:
- jax-rs
- security
- gotcha
- refactoring
title: JAX-RS inherits routing annotations from interfaces but not custom security
  annotations
type: lesson
---

# JAX-RS inherits routing annotations from interfaces but not custom security annotations

When extracting a JAX-RS resource into an interface + implementation, be deliberate about which annotations move. Per the JAX-RS spec, routing annotations (@Path, @GET, @POST, and parameter annotations) declared on an interface ARE inherited by the implementing class. But custom security/CDI annotations are generally NOT inherited from an interface, because reflective getAnnotation does not traverse interfaces. A filter/interceptor that reads e.g. axonivy @PermissionAllowed off the concrete class would silently fail-open if the annotation lived only on the interface.

Safe pattern: keep the extracted interface signatures-only and leave every annotation on the concrete resource class. That is how JsonStoreMongoDbApi was extracted in luz_jsonstore (both the v1 and v2 resources implement it, each keeping its own annotations).

## Related

- [[Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer|Serving a custom application/bson media type in JAX-RS via MessageBodyReader and Writer]]

%% ai-graph-start %%

**Related notes:**
- [[Implementing a @Path-annotated interface auto-registers the class as a JAX-RS server resource]]
- [[Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer]]
- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]
- [[OpenAPI @RequestBody mediaType is documentation-only; JAX-RS @Consumes controls content negotiation]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]

%% ai-graph-end %%