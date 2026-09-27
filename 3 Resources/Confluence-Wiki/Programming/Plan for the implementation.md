---
title: "Plan for the implementation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48124559379/Plan+for+the+implementation
space: "Helios"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2024-10-30
attachments: 0
tags:
  - confluence
  - programming
  - space/helios
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
