---
ai_hash: c90a48e2da46b3ef
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-24
entities:
- luz_online_payment
- Payrexx
- Transaction.java
- ObjectMapperFactory
- ObjectMapper
- ISO 8583 decline code
- LUZ-157476
- luz_store
- decline taxonomy
- structured decline code
- free-text message prose
- decline code fields
- FAIL_ON_UNKNOWN_PROPERTIES = false
- deserialization
- decline codes
- Payrexx card declines
source: session 2026-07-24, code investigation
status: seedling
tags:
- luz-online-payment
- payrexx
- jackson
- gotcha
- LUZ-157476
title: luz_online_payment silently drops Payrexx decline codes
type: lesson
---

# luz_online_payment silently drops Payrexx decline codes

luz_online_payment captures **no structured decline code** from Payrexx. The consumer response DTO `Transaction.java` declares no `code` / `errorCode` / `declineCode` / `reason` field, and the REST-client ObjectMapper is configured `FAIL_ON_UNKNOWN_PROPERTIES = false` (in `ObjectMapperFactory`).

Consequence: if Payrexx sends an ISO 8583 decline code on the wire, it is **silently dropped** during deserialization — no error, no log. The only decline detail that survives is the free-text `message` prose.

**Implication for the taxonomy work (LUZ-157476):** before a code->category mapping can be frozen, you must first confirm whether Payrexx even sends a code. The place to prove it is `Transaction.java` — add a candidate field, capture a declined sandbox payload, and observe. The lenient mapper is currently hiding the answer.

Repo: luz_online_payment.

## Related

- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[LUZ-157476 decline taxonomy maps codes at luz_online_payment boundary]]

%% ai-graph-start %%

**Related notes:**
- [[LUZ-157476 decline taxonomy maps codes at luz_online_payment boundary]]
- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[Payrexx v1.0 charge API returns only status+message on failure — no ISO 8583 code]]
- [[LUZ-157476 decline-code flow luz-online-payment forwards, luz_store maps]]
- [[KlaraPay DTOs are code-blind - lenient Jackson drops any Payrexx decline code]]

**Relations:**
- luz_online_payment — *silently drops* — Payrexx decline codes
- luz_online_payment — *captures no* — structured decline code
- structured decline code — *from* — Payrexx
- Transaction.java — *declares no* — decline code fields
- ObjectMapper — *is configured in* — ObjectMapperFactory
- ObjectMapper — *has setting* — FAIL_ON_UNKNOWN_PROPERTIES = false
- ISO 8583 decline code — *is dropped during* — deserialization
- deserialization — *is affected by* — FAIL_ON_UNKNOWN_PROPERTIES = false
- free-text message prose — *is the only surviving detail for* — decline
- LUZ-157476 — *is related to* — decline taxonomy
- Payrexx — *sends* — ISO 8583 decline code
- Transaction.java — *is the place to confirm* — Payrexx sends a code
- luz_online_payment — *is the repository* — luz_online_payment
- Payrexx card declines — *reach* — luz_store
- LUZ-157476 — *maps* — decline codes
- decline codes — *are mapped at* — luz_online_payment boundary

%% ai-graph-end %%