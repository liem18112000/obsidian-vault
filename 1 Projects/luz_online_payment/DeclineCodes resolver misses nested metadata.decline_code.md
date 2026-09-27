---
ai_hash: 67b0031aa48f480e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-31
entities:
- DeclineCodes resolver
- transaction.metadata.decline_code
- luz_online_payment
- Transaction.resolveDeclineCode()
- DeclineCodes.resolve()
- declineCode (field)
- additionalProperties
- metadata (field)
- '@JsonAnySetter'
- Jackson
- Payrexx
- webhook path
- Transaction.metadata
- Object (type)
- LinkedHashMap
- Metadata.java
- paypalBillingAgreementId
- LUZ-157476 commit
- synchronous path
- TransactionTask
- ConsumerServiceClientErrorException
- NotifiedTransactionService
- consumer
- luz_store
- webhook Content-Type
- type("json")
- MerchantService.java:115
- PayrexxNotifyTransactionResource
- APPLICATION_JSON
- form-urlencoded
- Payrexx delivers decline code only via webhook, not sync response (Note)
- Payrexx decline code lives at transaction.metadata.decline_code (Note)
- decline code (concept)
- JSON endpoint
source: LUZ-157476 code review 2026-07
status: seedling
tags:
- payrexx
- luz-157476
- decline-code
- luz-online-payment
- gotcha
title: DeclineCodes resolver misses nested metadata.decline_code
type: lesson
---

# DeclineCodes resolver misses nested metadata.decline_code

In `luz_online_payment`, `Transaction.resolveDeclineCode()` delegates to `DeclineCodes.resolve(declineCode, additionalProperties)`, which only inspects the top-level `declineCode` field plus top-level unknown keys captured by `@JsonAnySetter` into `additionalProperties`. Because `metadata` is a **known** field (`private Object metadata`), Jackson binds it directly and its nested contents never reach `additionalProperties`. Payrexx puts the code at `transaction.metadata.decline_code`, so **the current resolver misses it** even on the webhook path.

Fix: extend the resolver to also look inside the `metadata` map — `Transaction.metadata` is typed `Object` and deserializes to a `LinkedHashMap`, so read `((Map)metadata).get("decline_code")` (via the candidate-key list) with null/type guards (metadata can arrive as an empty array in other responses, which is why it is typed `Object`). `Metadata.java` models only `paypalBillingAgreementId`, so it is not usable for this.

Also: the LUZ-157476 commit wired `declineCode` onto the **synchronous** path (`TransactionTask` -> `ConsumerServiceClientErrorException`), which never carries the code. The plumbing must move to the webhook path (`NotifiedTransactionService` -> consumer) and forward to luz_store. And verify the webhook Content-Type: registered `type("json")` in `MerchantService.java:115` and `PayrexxNotifyTransactionResource` consumes `APPLICATION_JSON`, but the sample was form-urlencoded — a form-encoded POST would 415 the JSON endpoint.

Background: [[Payrexx delivers decline code only via webhook, not sync response]], [[Payrexx decline code lives at transaction.metadata.decline_code]].

## Related

- [[Payrexx delivers decline code only via webhook, not sync response]]
- [[Payrexx decline code lives at transaction.metadata.decline_code]]

%% ai-graph-start %%

**Related notes:**
- [[Payrexx notify webhook dispatches to two consumers, neither forwards decline code]]
- [[luz_online_payment silently drops Payrexx decline codes]]
- [[Payrexx delivers decline code only via webhook, not sync response]]
- [[luz_online_payment notify webhook silently 400-rejects ~43% of Payrexx webhooks on dev]]
- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]

**Relations:**
- DeclineCodes resolver — *misses* — transaction.metadata.decline_code
- Transaction.resolveDeclineCode() — *delegates to* — DeclineCodes.resolve()
- DeclineCodes.resolve() — *inspects* — declineCode (field)
- DeclineCodes.resolve() — *inspects* — additionalProperties
- metadata (field) — *is a* — known field
- Jackson — *binds* — metadata (field)
- metadata (field) — *nested contents never reach* — additionalProperties
- Payrexx — *puts* — decline code (concept)
- decline code (concept) — *at* — transaction.metadata.decline_code
- DeclineCodes resolver — *misses* — transaction.metadata.decline_code
- Fix — *extends* — DeclineCodes resolver
- DeclineCodes resolver — *should look inside* — metadata (field)
- Transaction.metadata — *is typed as* — Object (type)
- Transaction.metadata — *deserializes to* — LinkedHashMap
- Metadata.java — *models* — paypalBillingAgreementId
- Metadata.java — *is not usable for* — decline code (concept)
- LUZ-157476 commit — *wired* — declineCode (field)
- declineCode (field) — *wired onto* — synchronous path
- synchronous path — *never carries* — declineCode (field)
- plumbing — *must move to* — webhook path
- webhook path — *forwards to* — luz_store
- MerchantService.java:115 — *registers* — type("json")
- PayrexxNotifyTransactionResource — *consumes* — APPLICATION_JSON
- sample — *was* — form-urlencoded
- form-urlencoded — *would 415* — JSON endpoint
- Payrexx delivers decline code only via webhook, not sync response (Note) — *provides background for* — DeclineCodes resolver
- Payrexx decline code lives at transaction.metadata.decline_code (Note) — *provides background for* — DeclineCodes resolver
- Payrexx delivers decline code only via webhook, not sync response (Note) — *is related to* — DeclineCodes resolver
- Payrexx decline code lives at transaction.metadata.decline_code (Note) — *is related to* — DeclineCodes resolver

%% ai-graph-end %%