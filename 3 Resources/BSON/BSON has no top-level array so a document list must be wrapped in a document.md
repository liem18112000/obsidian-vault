---
ai_hash: d37e1558af81b404
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24
status: seedling
tags:
- bson
- mongodb
- gotcha
title: BSON has no top-level array so a document list must be wrapped in a document
type: lesson
---

# BSON has no top-level array so a document list must be wrapped in a document

BSON has no top-level array type; the root of a BSON payload is always a document. To ship a List<Document> over an application/bson endpoint, wrap it in a single BSON document, e.g. { "items": [ {...}, {...} ] }, and unwrap the "items" field on read.

A mongo-java-driver DocumentCodec round-trips this cleanly: nested BSON arrays decode to java.util.List and embedded documents decode to org.bson.Document, so wrapper.get("items") yields a List<Document>.

## Related

- [[Serving a custom application/bson media type in JAX-RS via MessageBodyReader and Writer]]

%% ai-graph-start %%

**Related notes:**
- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]
- [[Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[End-to-end BSON API testing with the Node bson package]]
- [[OpenAPI @RequestBody mediaType is documentation-only; JAX-RS @Consumes controls content negotiation]]

%% ai-graph-end %%