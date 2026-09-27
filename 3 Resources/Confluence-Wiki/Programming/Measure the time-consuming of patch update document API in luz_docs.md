---
ai_hash: 810107b095d87b94
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 2.5
entities: []
relevance: 0.706
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47363489990/Measure+the+time-consuming+of+patch+update+document+API+in+luz_docs
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Measure the time-consuming of patch update document API in luz_docs
topic: programming
type: source
updated: 2023-04-27
---

# Measure the time-consuming of patch update document API in luz_docs

> [!info] Imported from Confluence
> Space **TP2020** · updated 2023-04-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47363489990/Measure+the+time-consuming+of+patch+update+document+API+in+luz_docs)
> Relevance 0.706 · topic `programming`

## 1. Time-consuming analyzing

ENV: DEV-VN

Company: Performance Test Company

API information: `PATCH` <a href="http://localhost:12080/luz_docs/api/:tenant_id/documents/:letter-id" class="external-link" rel="nofollow">/luz_docs/api/:tenant_id/documents/:letter-id</a>

GCP LOG: <a href="https://cloudlogging.app.goo.gl/4afEPwieajMcfv9H9" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/4afEPwieajMcfv9H9</a>


![[47363489990-image-20230427-041532.png]]



→ The time-consuming take around **11 seconds** to update metadata for 20 documents

## 2. Problems

We need to **update multiple documents at once with different information**.

Currently, we need to loop to update metadata for each document.

→ This is inefficient and time-consuming

%% ai-graph-start %%

**Related notes:**
- [[Load test get document id API]]
- [[Refactor Enricher process - update PATCH]]
- [[Security Classes updating measurement]]
- [[Measure API luz-docs]]
- [[Research on bulk removal of access class]]

%% ai-graph-end %%