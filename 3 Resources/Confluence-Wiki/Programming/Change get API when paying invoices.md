---
ai_hash: d6774c68759f6832
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.762
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47212954432/Change+get+API+when+paying+invoices
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Change get API when paying invoices
topic: programming
type: source
updated: 2022-11-18
---

# Change get API when paying invoices

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-11-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47212954432/Change+get+API+when+paying+invoices)
> Relevance 0.762 · topic `programming`

# 1. User story

When paying invoices there are thumbnails and the list of documents in the overview:

User Story detail: <a href="https://axonivy.atlassian.net/browse/LUZ-88517" class="external-link" rel="nofollow">https://axonivy.atlassian.net/browse/LUZ-88517</a>

# 2. Code changes

We did introduce a new **DocumentBookingPaymentService**.java to get the DocumentDisplay with thumbnail file that could be shown later on the payment due page.

<a href="https://bitbucket.org/axonivy-prod/luz_components/pull-requests/8396/implement-change-get-api-when-paying" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_components/pull-requests/8396/implement-change-get-api-when-paying</a>

And a quick changes in **BankAccountConverter**.java of luz_finance to adapt the new implementation of retrieving data from DB (neither luz_docs or File Manager).

<a href="https://bitbucket.org/axonivy-prod/luz_finance/pull-requests/6314/change-get-api-when-paying-invoices" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_finance/pull-requests/6314/change-get-api-when-paying-invoices</a>

%% ai-graph-start %%

**Related notes:**
- [[Impact of code changes on common components]]
- [[Get article thumbnail API - related modules]]
- [[Merging process]]
- [[Invoice Run & luz-docs Archive Improvements]]
- [[Programming]]

%% ai-graph-end %%