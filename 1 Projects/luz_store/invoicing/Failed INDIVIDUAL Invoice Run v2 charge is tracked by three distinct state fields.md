---
ai_hash: 6ce71befbb90f038
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- INDIVIDUAL Invoice Run v2 charge
- state fields
- invoice_item.state
- InvoiceItemState
- CREDIT_CARD_CHARGED_PENDING
- CREDIT_CARD_CHARGED_FAILED
- invoice_charge_tracking.state
- ChargeTrackingState
- PENDING (ChargeTrackingState)
- SUSPENDED (ChargeTrackingState)
- invoice_credit_card_transaction.transaction_state
- TransactionState
- FAILED (TransactionState)
- transaction_status=ERROR
- COMPANY charge
- InvoiceRunServiceController
- Payrexx
- ChargeFailedNotificationTrigger.findFailedItems
- payment method CREDIT
- luz-store-seed-failed-charge skill
- LUZ-157478
- Payrexx card declines reach luz_store as ERROR with prose (note)
- not DECLINED (note)
- DECLINED status falls through invoice charge-failure handling in luz_store (note)
- Invoice run v2 shows charge failures via verbatim message copy at controller line
  628 (note)
- Invoice Run v2
- credit-card charge
- notification flow
- InvoiceCreditCardTransactionConverter
- source of truth
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
- INDIVIDUAL Invoice Run v2 charge — *is tracked by* — state fields
- state fields — *include* — invoice_item.state
- state fields — *include* — invoice_charge_tracking.state
- state fields — *include* — invoice_credit_card_transaction.transaction_state
- invoice_item.state — *uses enum* — InvoiceItemState
- InvoiceItemState — *represents individual failure as* — CREDIT_CARD_CHARGED_PENDING
- InvoiceItemState — *represents company failure as* — CREDIT_CARD_CHARGED_FAILED
- invoice_charge_tracking.state — *uses enum* — ChargeTrackingState
- ChargeTrackingState — *has value* — PENDING (ChargeTrackingState)
- ChargeTrackingState — *has value* — SUSPENDED (ChargeTrackingState)
- invoice_credit_card_transaction.transaction_state — *uses enum* — TransactionState
- TransactionState — *has failure value* — FAILED (TransactionState)
- invoice_credit_card_transaction.transaction_state — *is less reliable signal than* — invoice_item.state
- InvoiceRunServiceController — *distinguishes* — INDIVIDUAL Invoice Run v2 charge
- InvoiceRunServiceController — *distinguishes* — COMPANY charge
- InvoiceRunServiceController — *sets invoice_item.state to* — CREDIT_CARD_CHARGED_PENDING
- InvoiceRunServiceController — *for* — INDIVIDUAL Invoice Run v2 charge
- InvoiceRunServiceController — *sets invoice_item.state to* — CREDIT_CARD_CHARGED_FAILED
- InvoiceRunServiceController — *for* — COMPANY charge
- invoice_credit_card_transaction — *row is written on* — Payrexx
- notification flow — *is handled by* — ChargeFailedNotificationTrigger.findFailedItems
- ChargeFailedNotificationTrigger.findFailedItems — *keys off* — invoice_item.state
- ChargeFailedNotificationTrigger.findFailedItems — *keys off* — payment method CREDIT
- invoice_item.state = CREDIT_CARD_CHARGED_PENDING — *is* — source of truth
- source of truth — *for* — notification flow
- invoice_credit_card_transaction — *matches on* — transaction_status=ERROR
- luz-store-seed-failed-charge skill — *was corrected in* — LUZ-157478
- LUZ-157478 — *involved* — verify step
- INDIVIDUAL Invoice Run v2 charge — *is related to* — Payrexx card declines reach luz_store as ERROR with prose (note)
- INDIVIDUAL Invoice Run v2 charge — *is related to* — not DECLINED (note)
- INDIVIDUAL Invoice Run v2 charge — *is related to* — DECLINED status falls through invoice charge-failure handling in luz_store (note)
- INDIVIDUAL Invoice Run v2 charge — *is related to* — Invoice run v2 shows charge failures via verbatim message copy at controller line 628 (note)
- Invoice Run v2 — *processes* — credit-card charge
- InvoiceCreditCardTransactionConverter — *defines enum values for* — TransactionState

%% ai-graph-end %%