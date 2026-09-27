---
ai_hash: f3f144df7602f354
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- luz_docs_import
- Java 17
- javax stack
- modern idioms
- pom.xml
- maven.compiler.source
- maven.compiler.target
- Java EE
- javax.ejb
- javax.ws.rs
- javax.json
- MicroProfile REST clients
- WildFly
- var
- records
- instanceof pattern matching
- String.formatted()
- List.of
- text blocks
- Java 8
- compiler target
- maven.compiler.release
- javax
- jakarta
- EE namespace
- JDK language level
- JSON
- JsonObject
- JsonObjectBuilder
- JsonValue
- JsonString
- Jackson
- Gson
- Models
- Lombok
- '@Getter'
- '@Setter'
- '@Builder'
- DocsImportAsyncService.createDocument()
- document metadata
source: LUZ-158230 impl 2026-08-04
status: seedling
tags:
- luz-docs-import
- java
- kepler
- gotcha
title: luz_docs_import targets Java 17 (javax stack, modern idioms allowed)
type: lesson
---

# luz_docs_import targets Java 17 (javax stack, modern idioms allowed)

The luz_docs_import service targets **Java 17** (`pom.xml`: `maven.compiler.source/target = 17`), even though it runs on the Java EE `javax.*` stack (javax.ejb, javax.ws.rs, javax.json, MicroProfile REST clients on WildFly). So modern language idioms ARE available and are used in the codebase: `var`, records, `instanceof` pattern matching, `String.formatted()`, `List.of`, text blocks.

Correction: an earlier note claimed this repo was Java 8 — that was wrong (inferred before checking the pom). Always confirm the compiler target in pom.xml (`maven.compiler.target` / `maven.compiler.release`) rather than inferring the Java level from the `javax.*` imports — javax vs jakarta indicates the EE namespace, NOT the JDK language level.

JSON is javax.json (JsonObject/JsonObjectBuilder/JsonValue/JsonString), not Jackson/Gson. Models use Lombok (@Getter/@Setter/@Builder).

Related: [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]

## Related

- [[luz_docs_import adds document metadata only in DocsImportAsyncService.createDocument()]]

%% ai-graph-start %%

**Related notes:**
- [[DocumentFile.java - Three-Repo Class Map]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[Align the Google Cloud stack in a luz WildFly WAR via libraries-bom]]
- [[luz_docs_import idempotent re-import replaces view-controller search-based file dedup]]
- [[luz-docs-import createDocument is not idempotent — server-generated id, @Retry can duplicate on lost response]]

**Relations:**
- luz_docs_import — *targets* — Java 17
- luz_docs_import — *runs on* — javax stack
- luz_docs_import — *uses* — modern idioms
- pom.xml — *configures* — Java 17
- pom.xml — *defines* — maven.compiler.source
- pom.xml — *defines* — maven.compiler.target
- javax stack — *is* — Java EE
- javax stack — *includes* — javax.ejb
- javax stack — *includes* — javax.ws.rs
- javax stack — *includes* — javax.json
- luz_docs_import — *runs on* — WildFly
- luz_docs_import — *uses* — MicroProfile REST clients
- MicroProfile REST clients — *runs on* — WildFly
- modern idioms — *include* — var
- modern idioms — *include* — records
- modern idioms — *include* — instanceof pattern matching
- modern idioms — *include* — String.formatted()
- modern idioms — *include* — List.of
- modern idioms — *include* — text blocks
- luz_docs_import — *was incorrectly associated with* — Java 8
- compiler target — *is defined in* — pom.xml
- pom.xml — *defines* — maven.compiler.release
- javax — *indicates* — EE namespace
- jakarta — *indicates* — EE namespace
- EE namespace — *is not* — JDK language level
- luz_docs_import — *uses* — JSON
- JSON — *is implemented by* — javax.json
- javax.json — *includes* — JsonObject
- javax.json — *includes* — JsonObjectBuilder
- javax.json — *includes* — JsonValue
- javax.json — *includes* — JsonString
- luz_docs_import — *does not use* — Jackson
- luz_docs_import — *does not use* — Gson
- Models — *use* — Lombok
- Lombok — *provides* — @Getter
- Lombok — *provides* — @Setter
- Lombok — *provides* — @Builder
- luz_docs_import — *is related to* — DocsImportAsyncService.createDocument()
- DocsImportAsyncService.createDocument() — *handles* — document metadata

%% ai-graph-end %%