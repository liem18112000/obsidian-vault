---
ai_hash: f9209808fc52019a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-07-24
entities:
- Invoice run v2
- Charge failures
- Message copying
- InvoiceRunServiceController
- luz_store
- UI Message column
- Invoice item message
- Klara transaction request message
- handleChargeCredit method
- In-flight charge-response message
- Localization
- Mapping
- InvoiceCreditCardTransaction.errorMessage
- Audit copy
- TransactionState.FAILED
- Message survival
- isWarningState method
- NOT_FINISHED transactions
- FAILED state
- REFUND_FAILED state
- ROLLBACKED state
- PDF_CREATED_WARNING state
- Message wiping
- Charging state
- CREDIT_CARD_CHARGED state
- DECLINED message
- Warning state
- Empty message
- UI
- Line 628
- Injection point
- Category-to-localized-string display
- LUZ-157476 Phase 4
- NPE risk
- handleChargeByCreditCard method
- Null check
- Payrexx card declines
- ERROR status
- DECLINED status
- Invoice charge-failure handling
- controller:627
- controller:638
- controller:674
- controller:782
- controller:853-856
source: LUZ-157476 session 2026-07-24
status: budding
tags:
- luz-store
- invoice-run
- payrexx
- message-mapping
title: Invoice run v2 shows charge failures via verbatim message copy at controller
  line 628
type: concept
---

# Invoice run v2 shows charge failures via verbatim message copy at controller line 628

In luz_store invoice run v2, the UI Message column for failed charges is populated by ONE line: invoiceItem.setMessage(klaraTransactionRequestAfterCharge.getMessage()) (InvoiceRunServiceController.java:628, inside handleChargeCredit) — the in-flight charge-response message copied verbatim, no localization or mapping. The persisted InvoiceCreditCardTransaction.errorMessage is an audit copy that nothing reads back for display.

TransactionState.FAILED controls message survival, not content: isWarningState (controller:782) checks NOT_FINISHED transactions for FAILED/REFUND_FAILED/ROLLBACKED — if any, the item ends as PDF_CREATED_WARNING and keeps the message; otherwise the message is wiped (controller:674). The charging state itself goes CREDIT_CARD_CHARGED unconditionally even on failure (controller:638).

Consequences: DECLINED (message=null) → warning state with EMPTY message in UI. Line 628 is the single injection point for category→localized-string display in LUZ-157476 Phase 4. Also an NPE risk: handleChargeByCreditCard can return null (controller:853-856) but controller:627 dereferences without null check.

## Related
- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[DECLINED status falls through invoice charge-failure handling in luz_store]]

%% ai-graph-start %%

**Related notes:**
- [[Observed Payrexx prose vocabulary in dev is only three messages]]
- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[DECLINED status falls through invoice charge-failure handling in luz_store]]
- [[KlaraTransactionRequest.message content is Payrexx prose or runtime exception text, never a mapped constant]]
- [[TECHNICAL_ERROR is not retried in-flight but is retry-eligible on invoice-item rerun]]

**Relations:**
- Invoice run v2 — *shows* — Charge failures
- Charge failures — *via* — Message copying
- Message copying — *at* — Line 628
- luz_store — *has* — Invoice run v2
- UI Message column — *populated by* — Invoice item message
- Invoice item message — *set from* — Klara transaction request message
- Klara transaction request message — *is* — In-flight charge-response message
- Line 628 — *is in* — InvoiceRunServiceController
- Line 628 — *is inside* — handleChargeCredit method
- In-flight charge-response message — *lacks* — Localization
- In-flight charge-response message — *lacks* — Mapping
- InvoiceCreditCardTransaction.errorMessage — *is an* — Audit copy
- Audit copy — *not read for* — UI
- TransactionState.FAILED — *controls* — Message survival
- isWarningState method — *checks* — NOT_FINISHED transactions
- NOT_FINISHED transactions — *for* — FAILED state
- NOT_FINISHED transactions — *for* — REFUND_FAILED state
- NOT_FINISHED transactions — *for* — ROLLBACKED state
- Invoice item — *ends as* — PDF_CREATED_WARNING state
- PDF_CREATED_WARNING state — *keeps* — Invoice item message
- Invoice item message — *wiped at* — controller:674
- Charging state — *becomes* — CREDIT_CARD_CHARGED state
- CREDIT_CARD_CHARGED state — *set at* — controller:638
- DECLINED message — *leads to* — Warning state
- Warning state — *shows* — Empty message
- Empty message — *in* — UI
- Line 628 — *is the* — Injection point
- Injection point — *for* — Category-to-localized-string display
- Category-to-localized-string display — *is part of* — LUZ-157476 Phase 4
- NPE risk — *due to* — handleChargeByCreditCard method
- handleChargeByCreditCard method — *can return* — null
- controller:627 — *dereferences without* — Null check
- Payrexx card declines — *reach* — luz_store
- Payrexx card declines — *as* — ERROR status
- DECLINED status — *falls through* — Invoice charge-failure handling
- Invoice charge-failure handling — *is in* — luz_store

%% ai-graph-end %%