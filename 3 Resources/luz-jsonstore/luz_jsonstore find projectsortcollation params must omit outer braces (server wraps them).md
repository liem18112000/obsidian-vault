---
ai_hash: f14ad92109711bd3
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-07
entities: []
source: luz_docs_import dedup 500 debug 2026-08-07
status: seedling
tags:
- luz-jsonstore
- mongodb
- rest-client
- gotcha
- idempotency
title: 'luz_jsonstore find: project/sort/collation params must omit outer braces (server
  wraps them)'
type: gotcha
---

# luz_jsonstore find: project/sort/collation params must omit outer braces (server wraps them)

luz_jsonstore's find/getMany endpoint (POST mdb/{tenant}/{collection}) treats its `project`, `sort`, and `collation` **query params** as the INNER body of a JSON object and wraps them in braces itself:

```java
Document projectFields = Document.parse("{" + project + "}");   // getMany, JsonStoreMongoDbService
Document sortDoc      = Document.parse("{" + sort + "}");
Collation collation   = new Gson().fromJson("{" + collationString + "}", Collation.class);
```

So the caller must pass the field list WITHOUT surrounding braces, e.g. project=`"successfulFiles":1,"skippedFiles":1` — NOT `{"successfulFiles":1,...}`. If you include the braces, the server builds `{{...}}`, `Document.parse` throws, and getMany's catch-all returns **HTTP 500 with an empty body** (no message). 

Contrast: the **filter** is sent as the POST request body (a raw JsonObject) and used directly by `collection.find(filter)` — it DOES need to be a complete `{...}` object. Only the query-param fragments (project/sort/collation) are the brace-less ones. Easy to get inconsistent because body and query use opposite conventions.

How it bit us (luz_docs_import IdempotentImportService): PROJECTION was `{"successfulFiles":1,"skippedFiles":1}`; every find 500'd; the best-effort catch swallowed it and returned an empty already-imported set, so re-uploading the same zip re-imported every file (dedup silently disabled). Fix: drop the outer braces from PROJECTION.

General lesson: a **best-effort catch that returns a neutral/empty value** turns a hard failure (500) into a silent feature regression — log loudly (we did: 'could not load prior imported paths') and check that log when a feature 'does nothing'. The empty 500 body came from json-store's getMany catch-all, which also hides the real cause — see the luz_jsonstore missing-ExceptionMapper issue.

Related: [[luz_docs_import]], [[Read-side fire-and-forget mutation pass the id and re-read in the async, don't mutate the object being serialized|Read-side fire-and-forget mutation: pass the id and re-read in the async, don't mutate the object being serialized]].

## Related

- [[luz_docs_import]]

%% ai-graph-start %%

**Related notes:**
- [[luz-jsonstore GET-by-id masks read exceptions as empty-body 400; 10MB max-post-size caps whole-doc $set writes]]
- [[luz-jsonstore find returns 200 empty string, not [], on zero matches]]
- [[luz-jsonstore intermittently returns 200 with empty body on folder finds]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[luz_jsonstore committed V2 updateOne count delete have latent BSON serialization bug]]

%% ai-graph-end %%