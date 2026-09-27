---
ai_hash: 6ba0d02d550adefc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-26
entities: []
source: session 2026-08-26
status: seedling
tags:
- jax-rs
- resteasy
- routing
- rest
title: JAX-RS literal path segment outranks a template variable in request matching
type: concept
---

# JAX-RS literal path segment outranks a template variable in request matching

In JAX-RS (RESTEasy on WildFly), when two resource methods share an HTTP verb and their paths differ only in that one segment is a **literal** and the other is a **template variable**, the request-matching algorithm ranks the literal as more specific and picks it. So a literal sub-resource path can safely coexist with a catch-all `{id}` path on the same verb.

Concrete case in `JsonStoreMongoDbResourceV2` (class path `mdb/v2/{tenant-id}/{collection}`), all under `@PATCH`:
- `@PATCH` (no `@Path`) -> matches `.../{collection}` (collection root) = `updateMany`
- `@Path("update-bulk") @PATCH` -> matches `.../{collection}/update-bulk` = `updateBulk`
- `@Path("{doc-id}") @PATCH` -> matches `.../{collection}/{doc-id}` = `updateOne`

A `PATCH .../{collection}/update-bulk` could textually match both `update-bulk` and `{doc-id}`; the literal `update-bulk` wins, so no explicit disambiguation is needed. (Same reason `@Path("count")` coexists with `@Path("{doc-id}")` under different verbs.)

## Related

- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]

%% ai-graph-start %%

**Related notes:**
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[Implementing a @Path-annotated interface auto-registers the class as a JAX-RS server resource]]
- [[luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug]]
- [[JAX-RS inherits routing annotations from interfaces but not custom security annotations]]
- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]

%% ai-graph-end %%