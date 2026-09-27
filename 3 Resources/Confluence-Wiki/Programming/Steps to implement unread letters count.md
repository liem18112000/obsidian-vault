---
title: "Steps to implement unread letters count"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47255126194/Steps+to+implement+unread+letters+count
space: "TP2020"
topic: programming
relevance: 0.878
depth: 3
updated: 2023-01-10
attachments: 3
tags:
  - confluence
  - programming
  - space/tp2020
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
