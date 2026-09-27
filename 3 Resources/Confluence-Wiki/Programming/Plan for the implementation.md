---
ai_hash: 8887841aa9a7d125
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48124559379/Plan+for+the+implementation
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Plan for the implementation
topic: programming
type: source
updated: 2024-10-30
---

# Plan for the implementation

> [!info] Imported from Confluence
> Space **Helios** · updated 2024-10-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48124559379/Plan+for+the+implementation)
> Relevance 0.738 · topic `programming`

## Solution

- Payment will trigger from the POS app directly to Adyen’s side

- Webhooks will be implemented on local devices and use our API to update the display’s URL to the terminal setting on Adyen’s side

## Steps to implement

### Backend

1.  Prepare at least 2 API keys and store them on the backend side (1 for payment with minimum permission, 1 for updating the settings of the terminal on Adyen’s side)

2.  Implement API to update webhooks URL on Adyen’s side

3.  Investigate about managing one API key for each tenant

### POS app

1.  Build webhooks on the local device to receive notifications and show them on the UI

2.  Integrate API updating webhooks URL

3.  Handle error exceptions (Ex: when Adyen returns error token expired, the network broken during payment …)

4.  Handle cancel payment

5.  Handle logging payment process

6.  Adapt merchant receipt (if necessary)

%% ai-graph-start %%

**Related notes:**
- [[Tap to pay implementation]]
- [[Send creditcard notification from luz-public-api-adapter-messaging]]
- [[Adyen Migration script]]
- [[Compare AI models gpt-5.4(medium) vs opus4.6(high)]]

%% ai-graph-end %%