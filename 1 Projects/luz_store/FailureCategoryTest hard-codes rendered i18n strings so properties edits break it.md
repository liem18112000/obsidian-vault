---
ai_hash: 3a89190c37fa9013
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-03
entities:
- FailureCategoryTest
- ch.klara.luz.store.onlinepayment.model
- i18n strings
- Locale
- DeliveryChannel
- assertEquals expectations
- src/main/resources/message/invoice/text_{de,fr,it,en}.properties
- unit tests
- surefire test phase
- Cloud Build
- klara-infra
- luz-store
- mvn deploy
- container image
- master branch
- LUZ-157476
- 9f751294a
- 'PR #1845'
- DE messages
- informal du
- formal Sie
- master build
- buildMessageGermanLocaleRendersGermanDeclined
- buildMessageGermanLocaleRendersGermanExpired
- buildMessageGermanLocaleRendersGermanNoPaymentMethod
- mt-receive/LUZ-157476-fix-failure-category-test
- invoice text_*.properties value
- mvn -o test -Dtest=FailureCategoryTest
- EN assertions
- DE/FR/IT assertions
- German tests
- templates
- placeholders
- brand
- card number
- payment request
- payrexx-iso8583-decline-code
- payment-failure taxonomy
source: session 2026-09-03 LUZ-157476
status: seedling
tags:
- luz_store
- i18n
- testing
- gotcha
- LUZ-157476
- cloud-build
title: FailureCategoryTest hard-codes rendered i18n strings so properties edits break
  it
type: lesson
---

# FailureCategoryTest hard-codes rendered i18n strings so properties edits break it

FailureCategoryTest (`ch.klara.luz.store.onlinepayment.model`) asserts on the **fully-rendered** customer-facing payment-failure strings — per `Locale` and per `DeliveryChannel` — via hard-coded `assertEquals` expectations. Because the expected text is the resolved message, **any** edit to a value in `src/main/resources/message/invoice/text_{de,fr,it,en}.properties` silently breaks these unit tests until the matching assertion is updated in the same commit.

**Why it bites:** the strings live in two places (the `.properties` file and the test literal) with no compile-time link. A translator-style reword compiles fine and only fails at the surefire `test` phase — and in the Cloud Build (`klara-infra`, trigger `luz-store`) `mvn deploy` runs tests first, so a red test means the container image is **never published** and master ships nothing.

**Concrete incident (LUZ-157476):** commit `9f751294a` rewrote the DE messages from informal *du* to formal *Sie* form and merged to master (PR #1845). That turned the master build red with 3 failures — `buildMessageGermanLocaleRendersGermanDeclined` / `...EmailRendersGermanExpired` / `...RendersGermanNoPaymentMethod` (test lines 138/148/158). Fix branch: `mt-receive/LUZ-157476-fix-failure-category-test`.

**How to apply:** whenever you touch an invoice `text_*.properties` value, `grep FailureCategoryTest` for the affected message and update its expected literal in the same commit. Verify locally with `mvn -o test -Dtest=FailureCategoryTest` before pushing. Note the EN and DE/FR/IT assertions are independent — changing only DE (leaving EN unchanged on master) breaks only the German tests.

**Placeholder gotcha:** the templates embed `{0} {1}` (brand, card number). When the request carries no payment, both render empty, so `"...Karte {0} {1} konnte..."` collapses to `"...Karte   konnte..."` with the extra literal spaces still present. The expected string in the test must keep those spaces verbatim or the assertion fails on whitespace alone.

Related: [[payrexx-iso8583-decline-code]], LUZ-157476 payment-failure taxonomy.

## Related

- [[payrexx-iso8583-decline-code]]

%% ai-graph-start %%

**Related notes:**
- [[Invoice run v2 shows charge failures via verbatim message copy at controller line 628]]
- [[LUZ-157476 decline taxonomy maps codes at luz_online_payment boundary]]
- [[KlaraTransactionRequest.message content is Payrexx prose or runtime exception text, never a mapped constant]]
- [[Observed Payrexx prose vocabulary in dev is only three messages]]
- [[LUZ-157476 maps failure categories in luz_store only, overriding the boundary recommendation]]

**Relations:**
- FailureCategoryTest — *is_located_in* — ch.klara.luz.store.onlinepayment.model
- FailureCategoryTest — *asserts_on* — i18n strings
- i18n strings — *are_rendered_per* — Locale
- i18n strings — *are_rendered_per* — DeliveryChannel
- FailureCategoryTest — *uses* — assertEquals expectations
- assertEquals expectations — *hard_code* — i18n strings
- src/main/resources/message/invoice/text_{de,fr,it,en}.properties — *defines* — i18n strings
- edit_to — *breaks* — FailureCategoryTest
- src/main/resources/message/invoice/text_{de,fr,it,en}.properties — *is_edited_by* — edit_to
- i18n strings — *are_stored_in* — src/main/resources/message/invoice/text_{de,fr,it,en}.properties
- i18n strings — *are_hard_coded_in* — assertEquals expectations
- no_compile-time_link_between — *and* — assertEquals expectations
- src/main/resources/message/invoice/text_{de,fr,it,en}.properties — *is_part_of* — no_compile-time_link_between
- translator-style reword — *fails_at* — surefire test phase
- Cloud Build — *uses* — klara-infra
- Cloud Build — *uses* — luz-store
- Cloud Build — *runs* — mvn deploy
- mvn deploy — *executes* — unit tests
- red test — *prevents* — container image
- red test — *prevents* — master branch
- LUZ-157476 — *is_an_incident* — true
- 9f751294a — *is_a_commit* — true
- 9f751294a — *rewrote* — DE messages
- DE messages — *changed_from* — informal du
- DE messages — *changed_to* — formal Sie
- 9f751294a — *merged_via* — PR #1845
- 9f751294a — *caused* — master build
- master build — *failure* — true
- master build — *failed_with* — buildMessageGermanLocaleRendersGermanDeclined
- master build — *failed_with* — buildMessageGermanLocaleRendersGermanExpired
- master build — *failed_with* — buildMessageGermanLocaleRendersGermanNoPaymentMethod
- mt-receive/LUZ-157476-fix-failure-category-test — *is_a_fix_branch_for* — LUZ-157476
- invoice text_*.properties value — *requires_update_in* — FailureCategoryTest
- update_in — *should_be_in* — same commit
- FailureCategoryTest — *is_updated_by* — update_in
- mvn -o test -Dtest=FailureCategoryTest — *verifies* — changes
- EN assertions — *are_independent_of* — DE/FR/IT assertions
- changing DE messages — *breaks* — German tests
- templates — *embed* — placeholders
- placeholders — *represent* — brand
- placeholders — *represent* — card number
- no payment request — *causes* — placeholders
- placeholders — *to_render_empty_when* — no payment request
- expected string — *in* — FailureCategoryTest
- expected string — *must_preserve* — whitespace
- FailureCategoryTest — *is_related_to* — payrexx-iso8583-decline-code
- FailureCategoryTest — *is_related_to* — LUZ-157476
- LUZ-157476 — *is_related_to* — payment-failure taxonomy
- payrexx-iso8583-decline-code — *is_related_to* — FailureCategoryTest

%% ai-graph-end %%