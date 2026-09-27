---
ai_hash: 25fbfcdf1bdaa4e7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.878
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47255126194/Steps+to+implement+unread+letters+count
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Steps to implement unread letters count
topic: programming
type: source
updated: 2023-01-10
---

# Steps to implement unread letters count

> [!info] Imported from Confluence
> Space **TP2020** · updated 2023-01-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47255126194/Steps+to+implement+unread+letters+count)
> Relevance 0.878 · topic `programming`

## I. In luz_components module.

Handle showing the counter when refreshing the web page.

Checkout this branch: <a href="https://bitbucket.org/axonivy-prod/luz_components/branch/pioneer/LUZ-90036/research-counter-badge" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_components/branch/pioneer/LUZ-90036/research-counter-badge</a>

Note: When testing the local, you need to change the variables to value: **designer**


![[47255126194-image-20230110-063658.png]]



## II. In luz_epost_business_web.

Need to call the java script function named `getBadgeDigitalLetterbox()` when implementing this feature

Checkout this branch (need to trigger the above script manually): <a href="https://bitbucket.org/axonivy-prod/luz_epost_business_web/branch/pioneer/LUZ-90036/research-counter-badge" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_epost_business_web/branch/pioneer/LUZ-90036/research-counter-badge</a>

## III. In luz_docs_view_controller.

The query from team Optimus is not querying isStored field.

Checkout this branch to add query for isStored field: <a href="https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/LUZ-90036/research-counter-badge" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/LUZ-90036/research-counter-badge</a>

%% ai-graph-start %%

**Related notes:**
- [[Copy 4. Architecture for delivering eLetter after email verified]]
- [[eArchive performance — luz-epost-business-web calls the count API on every search]]
- [[Copy 5. How to extend modify ONE API delivery API Research]]
- [[5. How to extend modify ONE API delivery API Research]]
- [[4. Architecture for delivering eLetter after email verified]]

%% ai-graph-end %%