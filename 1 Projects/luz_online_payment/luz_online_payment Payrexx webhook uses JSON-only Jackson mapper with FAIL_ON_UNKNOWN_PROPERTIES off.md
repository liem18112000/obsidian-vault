---
ai_hash: cf624a634c8851e2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-31
entities:
- luz_online_payment
- Payrexx webhook
- Jackson mapper
- FAIL_ON_UNKNOWN_PROPERTIES
- POST /transactions/notify
- PayrexxNotifyTransactionResource
- RESTEasy 4.7.7
- Jackson JAX-RS provider
- ObjectMapper
- RESTContextResolver
- ObjectMapperFactory
- Transaction
- '@JsonAnySetter'
- application/json
- application/x-www-form-urlencoded
- 415 Unsupported Media Type
- MerchantService.java:115
- type("json")
- metadata
- Object
- LinkedHashMap
- Payrexx decline code lives at transaction.metadata.decline_code
- Payrexx notify webhook dispatches to two consumers, neither forwards decline code
source: LUZ-157476 investigation 2026-07
status: seedling
tags:
- payrexx
- webhook
- resteasy
- jackson
- luz-online-payment
- luz-157476
title: luz_online_payment Payrexx webhook uses JSON-only Jackson mapper with FAIL_ON_UNKNOWN_PROPERTIES
  off
type: observation
---

# luz_online_payment Payrexx webhook uses JSON-only Jackson mapper with FAIL_ON_UNKNOWN_PROPERTIES off

The `POST /transactions/notify` webhook endpoint (`PayrexxNotifyTransactionResource`) binds the body via RESTEasy 4.7.7 + the Jackson JAX-RS provider, using the custom `ObjectMapper` from `RESTContextResolver` (`@Provider @Singleton`) built in `ObjectMapperFactory`. That mapper sets `FAIL_ON_UNKNOWN_PROPERTIES = false`, which is why `Transaction`s `@JsonAnySetter` catch-all works and why adding new response fields never breaks binding.

Consequence 1 (decisive): the Jackson provider is chosen only for `application/json`. If Payrexx POSTs `application/x-www-form-urlencoded`, RESTEasy returns **415 Unsupported Media Type** and the handler never runs. The webhook is registered with `type("json")` in `MerchantService.java:115`, so JSON is expected — but confirm against a real dev webhook.

Consequence 2: unknown top-level keys are silently ignored unless captured by `@JsonAnySetter`; nested known fields (like `metadata`) bind to their declared type (`Object` -> LinkedHashMap).

Related: [[Payrexx decline code lives at transaction.metadata.decline_code]], [[Payrexx notify webhook dispatches to two consumers, neither forwards decline code]].

## Related

- [[Payrexx decline code lives at transaction.metadata.decline_code]]
- [[Payrexx notify webhook dispatches to two consumers, neither forwards decline code]]

%% ai-graph-start %%

**Related notes:**
- [[luz_online_payment notify webhook silently 400-rejects ~43% of Payrexx webhooks on dev]]
- [[Payrexx notify webhook dispatches to two consumers, neither forwards decline code]]
- [[DeclineCodes resolver misses nested metadata.decline_code]]
- [[KlaraPay DTOs are code-blind - lenient Jackson drops any Payrexx decline code]]
- [[TransactionStatus.from returns null on unknown Payrexx status causing silent NotNull 400]]

**Relations:**
- luz_online_payment — *uses* — Payrexx webhook
- Payrexx webhook — *uses* — Jackson mapper
- Jackson mapper — *is* — JSON-only
- Jackson mapper — *sets* — FAIL_ON_UNKNOWN_PROPERTIES
- FAIL_ON_UNKNOWN_PROPERTIES — *is set to* — false
- POST /transactions/notify — *is a* — webhook endpoint
- PayrexxNotifyTransactionResource — *handles* — POST /transactions/notify
- PayrexxNotifyTransactionResource — *binds body via* — RESTEasy 4.7.7
- PayrexxNotifyTransactionResource — *binds body via* — Jackson JAX-RS provider
- Jackson JAX-RS provider — *uses* — ObjectMapper
- ObjectMapper — *is from* — RESTContextResolver
- RESTContextResolver — *builds* — ObjectMapper
- ObjectMapper — *built in* — ObjectMapperFactory
- ObjectMapper — *sets* — FAIL_ON_UNKNOWN_PROPERTIES = false
- FAIL_ON_UNKNOWN_PROPERTIES = false — *enables* — @JsonAnySetter
- @JsonAnySetter — *handles* — unknown response fields
- Jackson JAX-RS provider — *chosen for* — application/json
- RESTEasy 4.7.7 — *returns* — 415 Unsupported Media Type
- 415 Unsupported Media Type — *occurs if POSTed with* — application/x-www-form-urlencoded
- Payrexx webhook — *is registered with* — type("json")
- type("json") — *is in* — MerchantService.java:115
- unknown top-level keys — *are* — ignored
- unknown top-level keys — *are captured by* — @JsonAnySetter
- nested known fields — *bind to* — declared type
- metadata — *is a* — nested known field
- metadata — *binds to* — Object
- Object — *is a* — LinkedHashMap
- Payrexx webhook — *related to* — Payrexx decline code lives at transaction.metadata.decline_code
- Payrexx webhook — *related to* — Payrexx notify webhook dispatches to two consumers, neither forwards decline code

%% ai-graph-end %%