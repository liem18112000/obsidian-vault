---
ai_hash: 0adc2107a8351218
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- Failed INDIVIDUAL charge (Invoice Run v2)
- State fields
- transaction_state field
- invoice_item.state field
- invoice_charge_tracking.state field
- invoice_item table
- invoice_charge_tracking table
- invoice_credit_card_transaction table
- InvoiceItemState enum
- ChargeTrackingState enum
- TransactionState enum
- CREDIT_CARD_CHARGED_PENDING state
- CREDIT_CARD_CHARGED_FAILED state
- PENDING state (ChargeTrackingState)
- SUSPENDED state (ChargeTrackingState)
- FAILED state (TransactionState)
- transaction_status=ERROR condition
- InvoiceRunServiceController
- COMPANY item
- INDIVIDUAL item
- Payrexx
- ChargeFailedNotificationTrigger.findFailedItems
- payment method CREDIT
- luz-store-seed-failed-charge skill
- LUZ-157478 ticket
- InvoiceCreditCardTransactionConverter
- SUCCESS state (TransactionState)
- ROLLBACKED state (TransactionState)
- REFUND_FAILED state (TransactionState)
- Notification flow
- CC transaction
- Payrexx card declines
- luz_store
- DECLINED status
- invoice charge-failure handling
- Invoice Run v2
- timeout/exception
- individual charge failed and needs retry/notification
source: session 2026-08-04
status: seedling
tags:
- luz_store
- invoice-run-v2
- charge-tracking
- LUZ-157478
- gotcha
title: Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state
  fields
type: lesson
---

# Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state fields

A failed **INDIVIDUAL** step-2 credit-card charge in Invoice Run v2 is represented across **three distinct state fields** — different enums, different tables — and the one most people reach for (`transaction_state`) is the *least* reliable signal.

| Field | Table | Enum | Failure value |
|---|---|---|---|
| `state` | `invoice_item` | `InvoiceItemState` | **`CREDIT_CARD_CHARGED_PENDING`** (individual) / `CREDIT_CARD_CHARGED_FAILED` (company) |
| `state` | `invoice_charge_tracking` | `ChargeTrackingState` | **`PENDING`** (fresh) or **`SUSPENDED`** (older row superseded) |
| `transaction_state` | `invoice_credit_card_transaction` | `TransactionState` | `FAILED` — but the row is optional, and code matches on `transaction_status=ERROR` |

## Why

- The COMPANY vs INDIVIDUAL split is in `InvoiceRunServiceController` (~lines 562-568 and 668-672): on a failed CREDIT charge an individual item goes to `CREDIT_CARD_CHARGED_PENDING`, a company item to `CREDIT_CARD_CHARGED_FAILED`. So individuals essentially never show `ChargeTrackingState.FAILED` on a normal fail.
- `ChargeTrackingState.PENDING`/`SUSPENDED` and `TransactionState` are **different enums**. `transaction_state` can only ever be SUCCESS/FAILED/ROLLBACKED/REFUND_FAILED (`InvoiceCreditCardTransactionConverter`) — PENDING/SUSPENDED never appear there. Conflating them produces a wrong verify query.
- The `invoice_credit_card_transaction` row is written **only if Payrexx actually returned a decline**. A timeout/exception leaves the item PENDING with no transaction row at all (the notification trigger has a fallback for exactly this).

## Authoritative signal

The notification flow (`ChargeFailedNotificationTrigger.findFailedItems`) keys off **`invoice_item.state = CREDIT_CARD_CHARGED_PENDING`** + payment method CREDIT. That is the source of truth for "this individual charge failed and needs retry/notification" — not `transaction_state=FAILED`. The CC transaction, when present, is matched by `transaction_status=ERROR`, not by `transaction_state`.

Discovered while correcting the verify step in the `luz-store-seed-failed-charge` skill (LUZ-157478).

## Related

- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[DECLINED status falls through invoice charge-failure handling in luz_store]]
- [[Invoice run v2 shows charge failures via verbatim message copy at controller line 628]]

%% ai-graph-start %%

**Related notes:**
- [[DECLINED status falls through invoice charge-failure handling in luz_store]]
- [[Invoice run v2 shows charge failures via verbatim message copy at controller line 628]]
- [[TECHNICAL_ERROR is not retried in-flight but is retry-eligible on invoice-item rerun]]
- [[Payrexx card declines reach luz_store as ERROR with prose, not DECLINED]]
- [[Payrexx declines travel in-band on HTTP 2xx in the luz charge flow]]

**Relations:**
- Failed INDIVIDUAL charge (Invoice Run v2) — *is tracked by* — State fields
- Failed INDIVIDUAL charge (Invoice Run v2) — *is represented across* — State fields
- transaction_state field — *is* — least reliable signal
- invoice_item.state field — *is field of* — invoice_item table
- invoice_item table — *uses enum* — InvoiceItemState enum
- InvoiceItemState enum — *has failure value for individual* — CREDIT_CARD_CHARGED_PENDING state
- InvoiceItemState enum — *has failure value for company* — CREDIT_CARD_CHARGED_FAILED state
- invoice_charge_tracking.state field — *is field of* — invoice_charge_tracking table
- invoice_charge_tracking table — *uses enum* — ChargeTrackingState enum
- ChargeTrackingState enum — *has failure value for fresh* — PENDING state (ChargeTrackingState)
- ChargeTrackingState enum — *has failure value for superseded row* — SUSPENDED state (ChargeTrackingState)
- transaction_state field — *is field of* — invoice_credit_card_transaction table
- invoice_credit_card_transaction table — *uses enum* — TransactionState enum
- TransactionState enum — *has failure value* — FAILED state (TransactionState)
- invoice_credit_card_transaction table — *code matches on* — transaction_status=ERROR condition
- InvoiceRunServiceController — *handles split for* — COMPANY item
- InvoiceRunServiceController — *handles split for* — INDIVIDUAL item
- INDIVIDUAL item — *transitions to* — CREDIT_CARD_CHARGED_PENDING state
- COMPANY item — *transitions to* — CREDIT_CARD_CHARGED_FAILED state
- INDIVIDUAL item — *never shows* — ChargeTrackingState.FAILED state
- ChargeTrackingState enum — *is different from* — TransactionState enum
- TransactionState enum — *can be* — SUCCESS state (TransactionState)
- TransactionState enum — *can be* — FAILED state (TransactionState)
- TransactionState enum — *can be* — ROLLBACKED state (TransactionState)
- TransactionState enum — *can be* — REFUND_FAILED state (TransactionState)
- InvoiceCreditCardTransactionConverter — *defines values for* — TransactionState enum
- PENDING state (ChargeTrackingState) — *never appears in* — TransactionState enum
- SUSPENDED state (ChargeTrackingState) — *never appears in* — TransactionState enum
- invoice_credit_card_transaction table — *row is written if* — Payrexx returned a decline
- timeout/exception — *leaves item in state* — PENDING state (ChargeTrackingState)
- PENDING state (ChargeTrackingState) — *has no* — transaction row
- Notification flow — *is implemented by* — ChargeFailedNotificationTrigger.findFailedItems
- ChargeFailedNotificationTrigger.findFailedItems — *keys off* — invoice_item.state field = CREDIT_CARD_CHARGED_PENDING state
- ChargeFailedNotificationTrigger.findFailedItems — *keys off* — payment method CREDIT
- CREDIT_CARD_CHARGED_PENDING state — *is source of truth for* — individual charge failed and needs retry/notification
- transaction_state field = FAILED state (TransactionState) — *is not source of truth for* — individual charge failed and needs retry/notification
- CC transaction — *is matched by* — transaction_status=ERROR condition
- luz-store-seed-failed-charge skill — *is related to* — LUZ-157478 ticket
- Payrexx card declines — *reach* — luz_store as ERROR
- Payrexx card declines — *do not reach* — luz_store as DECLINED status
- DECLINED status — *falls through* — invoice charge-failure handling
- Invoice Run v2 — *shows* — charge failures

%% ai-graph-end %%