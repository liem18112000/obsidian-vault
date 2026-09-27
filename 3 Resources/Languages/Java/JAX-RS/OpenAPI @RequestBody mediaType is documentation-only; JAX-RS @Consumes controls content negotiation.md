---
ai_hash: 8034b52189a12384
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-29
entities: []
source: luz_jsonstore session 2026-07-29
status: seedling
tags:
- jax-rs
- jakarta-ee
- openapi
- microprofile
- content-negotiation
- gotcha
title: OpenAPI @RequestBody mediaType is documentation-only; JAX-RS @Consumes controls
  content negotiation
type: lesson
---

# OpenAPI @RequestBody mediaType is documentation-only; JAX-RS @Consumes controls content negotiation

The MicroProfile OpenAPI `@RequestBody(content = @Content(mediaType = ...))` annotation only shapes the generated OpenAPI/Swagger spec — it has **zero effect on runtime content negotiation**. What a JAX-RS endpoint actually accepts and returns is decided solely by `@Consumes`/`@Produces`.

Consequences worth remembering:
- A class-level `@Consumes(APPLICATION_JSON)` applies to **every** method in the resource unless a method overrides it with its own `@Consumes`. A request whose `Content-Type` is not in that list is rejected with **415 Unsupported Media Type** before the method body ever runs.
- So changing the `@RequestBody` `mediaType` to advertise a new type does nothing on its own — you must also add it to `@Consumes`.

Concrete example: in luz_jsonstore's `JsonStoreMongoDbResource`, every method's `@RequestBody` says `mediaType = APPLICATION_JSON`, but the real gatekeeper is the class-level `@Consumes(MediaType.APPLICATION_JSON)` (no method-level overrides exist), so all endpoints are JSON-only.

Related: [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]

## Related

- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]

%% ai-graph-start %%

**Related notes:**
- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]
- [[Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[BSON has no top-level array so a document list must be wrapped in a document]]
- [[JAX-RS inherits routing annotations from interfaces but not custom security annotations]]

%% ai-graph-end %%