---
ai_hash: c2e7204ce85ab872
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48631021734/Load+test+get+document+id+API
space: TK
status: reference
tags:
- confluence
- programming
- space/tk
title: Load test get document id API
topic: programming
type: source
updated: 2025-08-22
---

# Load test get document id API

> [!info] Imported from Confluence
> Space **TK** · updated 2025-08-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48631021734/Load+test+get+document+id+API)
> Relevance 0.786 · topic `programming`

<div>

|  |  |  |  |  |  |
|----|----|----|----|----|----|
| **TenantId** | **Amount of documents** | **API** | **Elapse ms per request** | **Total elapse ms (100 requests)** | **Total elapse ms (1000 requests)** |
| ea1190d1-2148-4c13-82f0-3101f0f7ca53 | 371k | GET luz_docs/api/{tenant_id}/documents/{document_id} | 800ms ~ 900ms | ~1m30' | ~14m |
| 00a04daf-f2b3-41d5-8c12-2d1b4c48a36a | 493k | GET luz_docs/api/{tenant_id}/documents/{document_id} | 800ms ~ 900ms | ~1m30' | ~14m |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Measure the time-consuming of patch update document API in luz_docs]]
- [[luz-docs-statistic-get-latest-endpoint]]
- [[WIP Analyze subfolder search API (542ms)]]
- [[Measure API luz-docs]]
- [[luz-docs documentscount is ~130s on an 800k tenant — the 16-shard fan-out, not counting, is the bottleneck]]

%% ai-graph-end %%