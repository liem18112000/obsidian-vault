---
title: "Load test get document id API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48631021734/Load+test+get+document+id+API
space: "TK"
topic: programming
relevance: 0.786
depth: 3
updated: 2025-08-22
attachments: 0
tags:
  - confluence
  - programming
  - space/tk
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
