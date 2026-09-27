---
ai_hash: 6d0fac9f900b9475
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-05
entities:
- REST response
- boolean query param
- legacy-shaped DTO
- existing clients
- enrichment
- field type
- successfulFiles
- List<String>
- List<FileFailedInformation>
- dedicated view class
- from(enriched) mapper
- old contract
- JAX-RS
- resource method
- Object
- showWarning
- enriched model
- LegacyView.from(enriched)
- service
- full model
- view shaping
- resource
- presentation concern
- JSON-B
- Yasson
- runtime type
- nulls
- absent fields
- luz_docs_import
- master shape
- ImportJob
- per-file warning detail
- successful file
- non-error warning detail
- OpenAPI
- '@APIResponse'
- '@Schema'
- Enriched.class
- rich shape
- lean shape
- second schema
- Backward-compatible API evolution
- luz-docs document import
source: session 2026-08-05 luz_docs_import showWarning
status: seedling
tags:
- rest
- backward-compat
- jax-rs
- dto
- json-b
- luz-docs
title: Gate enriched REST response behind a boolean query param with a legacy-shaped
  DTO
type: lesson
---

# Gate enriched REST response behind a boolean query param with a legacy-shaped DTO

To evolve a REST response without breaking existing clients, add a boolean query param (default false) that gates the enrichment, and return a **separate legacy-shaped DTO** for the default case rather than mutating the enriched model.

**Why a separate DTO, not field-nulling:** if the enrichment *changed a field type* (e.g. `successfulFiles` went from `List<String>` to `List<FileFailedInformation>`), you cannot conditionally serialize the old shape from the new class — nulling a `detail` still yields `{filePath, detail:null}`, never a bare string. A dedicated view class with a `from(enriched)` mapper reproduces the old contract exactly (old field types + omitted new fields).

**Mechanics (JAX-RS):** change the resource method to return `Object` and pick `showWarning ? enriched : LegacyView.from(enriched)`. Keep the service returning the full model; do the view shaping at the resource (presentation concern). JSON-B/Yasson serializes by runtime type and omits nulls by default, so absent fields on the legacy DTO simply do not appear.

**Concrete case (luz_docs_import):** `GET import-jobs/{id}?showWarning=false` returns the master shape (successfulFiles as plain paths, no skippedFilesDetail/rejectedFiles); `?showWarning=true` returns the enriched ImportJob with per-file warning detail (e.g. an ignored `.metadata.json` sidecar). Domain nugget: a *successful* file can still carry a non-error warning detail.

Gotcha: OpenAPI `@APIResponse(schema=@Schema(implementation=Enriched.class))` documents only the rich shape; the default lean shape is undocumented unless you add a second schema.

## Related

- [[Backward-compatible API evolution]]
- [[luz-docs document import]]

%% ai-graph-start %%

**Related notes:**
- [[GET import-jobs id showWarning=false returns LegacyImportJob to keep luz_mylife_web working]]
- [[Verify a response-shape regression by tracing the downstream consumer, not the shape diff]]
- [[luz_jsonstore V2 BSON endpoints must be Document-in Document-out]]
- [[empty-object-not-null sentinel defeats Optional.ofNullable null-guards]]
- [[Add response fields tolerant-reader-first when producer and consumer deploy separately]]

**Relations:**
- REST response — *evolved for* — existing clients
- boolean query param — *gates* — enrichment
- legacy-shaped DTO — *used for* — default case
- legacy-shaped DTO — *avoids* — mutating the enriched model
- enrichment — *can change* — field type
- successfulFiles — *changes type from* — List<String>
- successfulFiles — *changes type to* — List<FileFailedInformation>
- dedicated view class — *reproduces* — old contract
- dedicated view class — *uses* — from(enriched) mapper
- JAX-RS — *defines* — resource method
- resource method — *returns* — Object
- resource method — *selects* — enriched model
- resource method — *selects* — LegacyView.from(enriched)
- resource method — *based on* — showWarning
- service — *returns* — full model
- view shaping — *occurs at* — resource
- view shaping — *is a* — presentation concern
- JSON-B — *serializes by* — runtime type
- Yasson — *serializes by* — runtime type
- JSON-B — *omits* — nulls
- Yasson — *omits* — nulls
- absent fields — *not present on* — legacy-shaped DTO
- luz_docs_import — *uses* — master shape
- luz_docs_import — *uses* — enriched ImportJob
- enriched ImportJob — *includes* — per-file warning detail
- successful file — *can have* — non-error warning detail
- OpenAPI — *documents* — rich shape
- @APIResponse — *used in* — OpenAPI
- @Schema — *used in* — OpenAPI
- Enriched.class — *represents* — rich shape
- lean shape — *requires* — second schema
- second schema — *for* — documentation
- Backward-compatible API evolution — *related to* — REST response
- luz-docs document import — *related to* — luz_docs_import

%% ai-graph-end %%