---
ai_hash: fcdb4dc0ea7dfb8c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.835
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48361668681/Tap+to+pay+implementation
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Tap to pay implementation
topic: programming
type: source
updated: 2025-02-28
---

# Tap to pay implementation

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-02-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48361668681/Tap+to+pay+implementation)
> Relevance 0.835 · topic `programming`

![[48361668681-image-20250225-063607.png]]



## Implementation flow (reference: <a href="https://docs.adyen.com/point-of-sale/ipp-mobile/tap-to-pay-android/" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.adyen.com/point-of-sale/ipp-mobile/tap-to-pay-android/</a>):

- **Development phase:**

  1.  Backend:

      1.  Create **client key** in current payment credential (required when using tap to pay)

      2.  Create API to create sessions (<a href="https://docs.adyen.com/point-of-sale/ipp-mobile/tap-to-pay-android/integration-ttp/#make-a-sessions-request" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.adyen.com/point-of-sale/ipp-mobile/tap-to-pay-android/integration-ttp/#make-a-sessions-request</a> )

  2.  POS app:

      - Upgrade Kotlin version to 2.1.0 (<a href="https://docs.adyen.com/point-of-sale/firmware-release-notes/#releaseNote=2025-02-17-android-sdk-on-mobile-2.0.0" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.adyen.com/point-of-sale/firmware-release-notes/#releaseNote=2025-02-17-android-sdk-on-mobile-2.0.0</a> )

      - Configure maven repositories for debugging SDK (need API credential with **"Allow SDK download for POS developers"** role) → **need a testing API credential stored in POS app to authenticate to the repo**

      - Implement `AuthenticationProvider` and `MerchantAuthenticationService` to establish a session automatically when make transaction.

      - Implement payment request

      - Implement refund request

      - Implement request turn on NFC reader when using Tap to pay

- **Going live phase:**

  1.  Backend:

      - Get the live endpoint with prefix in **Developers** \> **API URLs** \> **Prefix**. (example: `https://{PREFIX}-checkout-live.adyenpayments.com/checkout/possdk/v68/sessions`)

  2.  POS app:

      - Change maven repositories to get release SDK (<a href="https://docs.adyen.com/point-of-sale/ipp-mobile/tap-to-pay-android/integration-ttp/#going-live" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.adyen.com/point-of-sale/ipp-mobile/tap-to-pay-android/integration-ttp/#going-live</a>) → **need a live API credential stored in POS app to authenticate to the repo**

%% ai-graph-start %%

**Related notes:**
- [[Plan for the implementation]]

%% ai-graph-end %%