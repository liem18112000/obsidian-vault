---
ai_hash: 5f8f8c768ddc6ab0
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-24
entities:
- Payrexx
- KlaraTransactionRequest
- luz_online_payment
- chargeTransactionId
- INVOICE_CREDIT_CARD_TRANSACTION
- Payrexx/KlaraPay merchant backoffice
- ISO 8583 decline code
- KlaraPay
- KlaraPay API (v1.0)
- ClientResponseFilter
- PayrexxTransactionRestClient
- Jackson
- webhook resource
- Payrexx public docs
- KlaraPay DTOs
- decline-code field
- API transaction object
- Payrexx decline codes
- service responses
- raw body
source: LUZ-157476 session 2026-07-24
status: seedling
tags:
- payrexx
- klarapay
- luz-store
- debugging
title: Enumerate real Payrexx decline codes via chargeTransactionId lookup, not via
  service responses
type: howto
---

# Enumerate real Payrexx decline codes via chargeTransactionId lookup, not via service responses

The KlaraTransactionRequest response from luz_online_payment structurally cannot reveal Payrexx decline codes (no code field), and the deserialized Payrexx response drops unknown fields. To enumerate codes that actually occur: (1) zero-code path — take chargeTransactionId values from failed INVOICE_CREDIT_CARD_TRANSACTION rows (errorMessage non-null) and look those transactions up in the Payrexx/KlaraPay merchant backoffice, which displays the ISO 8583 decline code per transaction; (2) no-deploy path — GET https://api.klarapay.ch/v1.0/Transaction/{id} directly with the service's instance/apiKey credentials and inspect raw JSON to learn whether the v1.0 API carries a code field at all; (3) deploy path — ClientResponseFilter on PayrexxTransactionRestClient logging the raw body pre-Jackson, plus the webhook resource.

Expectation: Payrexx public docs show no decline-code field on the API transaction object — codes likely live only in the backoffice UI, so the API mapping input probably stays prose.

## Related
- [[KlaraPay DTOs are code-blind - lenient Jackson drops any Payrexx decline code]]
- [[Payrexx ISO 8583 decline code to meaning reference table]]

%% ai-graph-start %%

**Related notes:**
- [[KlaraPay DTOs are code-blind - lenient Jackson drops any Payrexx decline code]]
- [[Payrexx v1.0 charge API returns only status+message on failure — no ISO 8583 code]]
- [[luz_online_payment silently drops Payrexx decline codes]]
- [[KlaraPay V2 Java classes still call Payrexx API v1.0 on the consumer flow]]
- [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]]

**Relations:**
- KlaraTransactionRequest — *from* — luz_online_payment
- KlaraTransactionRequest — *lacks* — code field
- KlaraTransactionRequest — *cannot reveal* — Payrexx decline codes
- Payrexx response — *deserialized_by* — Jackson
- Jackson — *drops* — unknown fields
- chargeTransactionId — *from* — INVOICE_CREDIT_CARD_TRANSACTION
- Payrexx/KlaraPay merchant backoffice — *displays* — ISO 8583 decline code
- KlaraPay API (v1.0) — *endpoint* — https://api.klarapay.ch/v1.0/Transaction/{id}
- KlaraPay API (v1.0) — *may_contain* — decline-code field
- ClientResponseFilter — *applied_to* — PayrexxTransactionRestClient
- ClientResponseFilter — *logs* — raw body
- Payrexx public docs — *show_no* — decline-code field
- decline-code field — *on* — API transaction object
- KlaraPay DTOs — *are* — code-blind
- Jackson — *drops* — Payrexx decline code
- ISO 8583 decline code — *has* — meaning reference table
- Payrexx decline codes — *not_available_via* — service responses
- Payrexx decline codes — *available_via* — chargeTransactionId lookup
- webhook resource — *used_for* — logging

%% ai-graph-end %%