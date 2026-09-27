---
title: "Invoice Run – Credit Card Payment Only for Individual Runs"
created: 2026-09-21
updated: 2026-09-21
type: source
status: reference
source: "Confluence · LUZ - LUZ"
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49771839639/Invoice+Run+Credit+Card+Payment+Only+for+Individual+Runs
confluence_id: "49771839639"
confluence_path: "LUZ Home > 100_General > 02_Joint Review > Joint review 0.03.30.00 (08.09.2026 - 21.09.2026)"
tags: [confluence, invoice-run]
---

# Invoice Run – Credit Card Payment Only for Individual Runs

*Confluence source · LUZ Home › 100_General › 02_Joint Review › Joint review 0.03.30.00 (08.09.2026 - 21.09.2026) · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49771839639/Invoice+Run+Credit+Card+Payment+Only+for+Individual+Runs) · updated 2026-09-21*

## 1. Overview

> [!info]
>
>
>
> **Executive summary:** Starting with the **October Invoice Run**, Individual clients must use **credit card payment**. QR code payment will remain available for **Company Invoice Runs**.
>
>

From the **October Invoice Run**, Individual clients will be required to pay by **credit card**.

This change is designed to reduce QR code payment usage among Individual clients and encourage a more consistent credit card payment flow.

For **Company Invoice Runs**, QR code payment will continue to be supported.

## 2. Business Rationale

This change helps standardize the payment experience for Individual clients and reduce manual follow-up related to QR code payments.

To prepare users for the transition:

- A **marketing letter** has already been sent to Individual clients to announce the upcoming change.

- Users have been encouraged to update their payment method to **credit card before the October Invoice Run**.

- The **October Invoice Run** will be the first run where the new payment rule applies.

------------------------------------------------------------------------

## 3. Payment Rules from October

|                  |              |                         |
|------------------|--------------|-------------------------|
| Invoice Run Type | Credit Card  | QR Payment              |
| **Individual**   | **Required** | **No longer supported** |
| **Company**      | Supported    | Supported               |

> [!note]
>
>
> The new payment restriction applies **only to Individual Invoice Runs**. Company Invoice Runs will continue to support the existing payment methods.
>
>

## 4. Invoice Run Flow

When an Individual Invoice Run starts, the system will attempt to charge the user’s credit card.

### Case 1 — Credit Card Payment Successful

> [!tip]
>
>
>
> **Outcome:** The payment is completed and the Invoice Run proceeds as normal.
>
>

1.  Payment is completed.

2.  The Invoice Run continues normally.

3.  The system creates the PDF invoice.

4.  The invoice is sent to the user.

------------------------------------------------------------------------

### Case 2 — Payment Cannot Be Completed

> [!note]
>
>
>
> **Outcome:** The Invoice Run is moved to **Pending Charge** until the user updates their payment details or a retry succeeds.
>
>

This scenario applies when the user has not updated their payment method or when the credit card charge fails.

1.  The Invoice Run is moved to the **Pending Charge** list.

2.  The user is informed through the following channels:

    - **Notification email**

    - **Digital Simple Message**

3.  The user can update their credit card or payment information before the next automatic retry.

![[image-20260904-075655-20260921-054601.png]]

|  |  |
|----|----|
| ![[msg_A-20260921-031251.png]] | ![[msg_B-20260921-031253.png]] |

------------------------------------------------------------------------

## 5. Automatic Retry Schedule

For users in the **Pending Charge** list, the system will automatically retry payment on the **10th** and **20th** of the month.

|  |  |  |  |
|----|----|----|----|
| Retry Date | System Action | If Successful | If Failed |
| **10th of the month** | The system attempts to charge the user’s credit card again. | Continue the Invoice Run process, create the PDF invoice, and send the invoice to the user. | Keep the user in the Pending Charge flow until the next retry. |
| **20th of the month** | The system performs another automatic payment attempt. | Continue processing the Invoice Run normally. | The user remains in the failed-payment flow and moves toward final handling in the next Invoice Run. |

## 6. Final Attempt in the Next Invoice Run

If the payment fails during the original Invoice Run, fails again on the **10th**, and fails again on the **20th**, the system will make **one final payment attempt during the next Invoice Run**.

### Final attempt succeeds

> [!tip]
>
>
>
> The payment is completed and the Invoice Run continues normally.
>
>

### Final attempt fails

> [!warning]
>
>
>
> The system terminates the user’s **paid subscription** and sends a **subscription termination notification**.
>
>

This process gives the user multiple opportunities to update their payment method before subscription termination.

------------------------------------------------------------------------

## 7. Invoice Run Tracking UI

A new **Invoice Run Tracking UI** will be introduced for Individual users whose Invoice Run is in the **Pending Charge** list.

The UI helps users clearly understand:

- Their Invoice Run is waiting for payment.

- Their payment has not been completed.

- They need to update their payment method or resolve the payment issue.

- The system will automatically retry payment according to the retry schedule.

![[image-20260921-045201.png]]
