---
ai_hash: aedc46bbfd25f06a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities:
- BSON endpoint
- JSON
- MongoDB dates
- native Date
- type fidelity
- luz_jsonstore v1 endpoints
- org.bson.Document
- Document.toJson()
- string
- MongoDB
- BSON
- BSON datetime
- java.util.Date
- native Mongo Date
- _id
- native ObjectId
- v1 setDocId()
- hex string
- v2
- JsonStoreMongoDbResourceV2
- mdb/v2/{tenant-id}
- JsonStoreMongoDbService
- '"dates as string -> Mongo Date" migration branch'
- JAX-RS
- MessageBodyReader
- MessageBodyWriter
- top-level array
- document list
- document
source: session 2026-08-24
status: seedling
tags:
- mongodb
- bson
- luz-jsonstore
- date
title: Use a BSON endpoint instead of JSON to store MongoDB dates as native Date
type: lesson
---

# Use a BSON endpoint instead of JSON to store MongoDB dates as native Date

Transporting documents as JSON loses type fidelity. The luz_jsonstore v1 endpoints serialized org.bson.Document via Document.toJson(), so a date field arrived as a string and was stored in MongoDB as a string. Switching the wire format to BSON fixes this end-to-end: a BSON datetime decodes to java.util.Date and is stored as a native Mongo Date.

The same fidelity argument applies to _id: BSON carries a native ObjectId, so the v1 setDocId() step that rewrote _id into a hex string is intentionally dropped in v2 (JsonStoreMongoDbResourceV2 at path mdb/v2/{tenant-id}, reusing the same JsonStoreMongoDbService). This is the core rationale behind the "dates as string -> Mongo Date" migration branch.

## Related

- [[Serving a custom application/bson media type in JAX-RS via MessageBodyReader and Writer]]
- [[BSON has no top-level array so a document list must be wrapped in a document]]

%% ai-graph-start %%

**Related notes:**
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[Serving a custom applicationbson media type in JAX-RS via MessageBodyReader and Writer]]
- [[A JAX-RS body param typed org.bson.Document is JSON-deserialized, not a BSON wire format]]
- [[luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug]]
- [[End-to-end BSON API testing with the Node bson package]]

**Relations:**
- BSON endpoint — *stores* — MongoDB dates as native Date
- JSON — *loses* — type fidelity
- luz_jsonstore v1 endpoints — *serialized* — org.bson.Document
- org.bson.Document — *serialized via* — Document.toJson()
- Document.toJson() — *resulted in* — string
- string — *stored in* — MongoDB
- BSON — *fixes* — type fidelity
- BSON datetime — *decodes to* — java.util.Date
- java.util.Date — *stored as* — native Mongo Date
- BSON — *carries* — native ObjectId
- native ObjectId — *for* — _id
- v1 setDocId() — *rewrote* — _id
- _id — *rewritten to* — hex string
- v2 — *drops* — v1 setDocId()
- JsonStoreMongoDbResourceV2 — *has path* — mdb/v2/{tenant-id}
- JsonStoreMongoDbResourceV2 — *reuses* — JsonStoreMongoDbService
- Dropping v1 setDocId() — *is rationale for* — "dates as string -> Mongo Date" migration branch
- BSON — *lacks* — top-level array
- document list — *must be wrapped in* — document
- JAX-RS — *serves media type with* — MessageBodyReader
- JAX-RS — *serves media type with* — MessageBodyWriter

%% ai-graph-end %%