---
title: "Measure the time-consuming of patch update document API in luz_docs"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47363489990/Measure+the+time-consuming+of+patch+update+document+API+in+luz_docs
space: "TP2020"
topic: programming
relevance: 0.706
depth: 2.5
updated: 2023-04-27
attachments: 3
tags:
  - confluence
  - programming
  - space/tp2020
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
