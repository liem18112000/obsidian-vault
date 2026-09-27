---
ai_hash: a3f25973c67a34fb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-29
entities:
- Payrexx v1.0 charge API
- ISO 8583 code
- Payrexx
- api.klarapay.ch/v1.0
- POST /v1.0/Transaction/{id}
- ClientResponseFilter
- luz-online-payment Payrexx client
- LUZ-157476
- Transaction.additionalProperties
- PayrexxResponse.additionalProperties
- '@JsonAnySetter'
- KlaraTransactionRequest.declineCode
- luz_store
- FailureCategory
- message prose
- CARD_EXPIRED
- declineCode plumbing
- Jackson
- Payrexx BACK-OFFICE transaction detail view
- status field
- message field
- API response
- decline code
- code-based taxonomy
source: session 2026-07-29; raw response filter on dev
status: seedling
tags:
- luz-157476
- payrexx
- decline-code
- finding
title: Payrexx v1.0 charge API returns only status+message on failure — no ISO 8583
  code
type: observation
---

# Payrexx v1.0 charge API returns only status+message on failure — no ISO 8583 code

Empirically confirmed on dev (2026-07-29) against real Payrexx (api.klarapay.ch/v1.0): a failed charge via `POST /v1.0/Transaction/{id}` returns ONLY:

```
{"status":"error","message":"An error occurred: Your card has expired."}
```

No `data`, no code field — nothing else. Captured via a temporary ClientResponseFilter that logs the raw body on the luz-online-payment Payrexx client.

Consequences for LUZ-157476:
- The ISO 8583 two-digit decline code Payrexx support mentioned exists only in their BACK-OFFICE transaction detail view, NOT in this API response. So `Transaction.additionalProperties` / `PayrexxResponse.additionalProperties` (@JsonAnySetter capture) are legitimately empty — there is nothing to capture.
- Therefore `KlaraTransactionRequest.declineCode` stays null on this path, and luz_store CANNOT key `FailureCategory` off a code today — it must keep mapping from the free-text `message` prose (e.g. "Your card has expired." -> CARD_EXPIRED via the `expir` keyword).
- The declineCode plumbing built in luz-online-payment is still correct and future-proof: if Payrexx later exposes the code (a new field, an expand param, or a different endpoint), only the candidate-key list needs updating.

Open follow-up: ask Payrexx whether the code is retrievable via any API field / query param / other endpoint at all; if not, the code-based taxonomy is blocked upstream.

Related: [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]], [[Capture an unknown-named JSON field with Jackson @JsonAnySetter]]

## Related

- [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]]
- [[Capture an unknown-named JSON field with Jackson @JsonAnySetter]]

%% ai-graph-start %%

**Related notes:**
- [[Enumerate real Payrexx decline codes via chargeTransactionId lookup, not via service responses]]
- [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]]
- [[luz_online_payment silently drops Payrexx decline codes]]
- [[LUZ-157476 decline taxonomy maps codes at luz_online_payment boundary]]
- [[KlaraPay DTOs are code-blind - lenient Jackson drops any Payrexx decline code]]

**Relations:**
- Payrexx v1.0 charge API — *returns* — status field
- Payrexx v1.0 charge API — *returns* — message field
- Payrexx v1.0 charge API — *does not return* — ISO 8583 code
- Payrexx v1.0 charge API — *accessed via* — POST /v1.0/Transaction/{id}
- Payrexx v1.0 charge API — *confirmed against* — api.klarapay.ch/v1.0
- ISO 8583 code — *exists in* — Payrexx BACK-OFFICE transaction detail view
- ISO 8583 code — *not present in* — API response
- Transaction.additionalProperties — *is* — empty
- PayrexxResponse.additionalProperties — *is* — empty
- @JsonAnySetter — *used for* — capture
- KlaraTransactionRequest.declineCode — *remains* — null
- luz_store — *cannot key* — FailureCategory
- FailureCategory — *off* — decline code
- luz_store — *maps* — message prose
- message prose — *to* — FailureCategory
- message prose — *example* — Your card has expired.
- Your card has expired. — *maps to* — CARD_EXPIRED
- declineCode plumbing — *is* — correct and future-proof
- declineCode plumbing — *is in* — luz-online-payment Payrexx client
- Payrexx — *may expose* — decline code
- code-based taxonomy — *is blocked by* — Payrexx API limitation
- LUZ-157476 — *related to* — decline-code flow luz-online-payment forwards, luz_store maps
- LUZ-157476 — *related to* — Capture an unknown-named JSON field with Jackson @JsonAnySetter
- luz-online-payment Payrexx client — *uses* — ClientResponseFilter
- Capture an unknown-named JSON field with Jackson @JsonAnySetter — *uses* — Jackson

%% ai-graph-end %%