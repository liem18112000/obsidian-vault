---
ai_hash: 80491887f1d13fb1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.76
entities: []
relevance: 0.79
source: https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/49513988106/Creating+invoice+GUI+flow+vs.+Public+API
space: NEXT
status: reference
tags:
- confluence
- programming
- space/next
title: Creating invoice — GUI flow vs. Public API
topic: programming
type: source
updated: 2026-06-17
---

# Creating invoice — GUI flow vs. Public API

> [!info] Imported from Confluence
> Space **NEXT** · updated 2026-06-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/NEXT/pages/49513988106/Creating+invoice+GUI+flow+vs.+Public+API)
> Relevance 0.79 · topic `programming`

**Purpose.** Explain in business terms (1) how a user creates an invoice in the KLARA Order-Management GUI — the steps, the decisions they make, and the checks they must clear before the invoice is accepted — and (2) why exposing this as a public API does **not** reproduce the full GUI experience.

**Scope.** "Create invoice for a customer" in Order Management, covering both finishing paths (save directly as INVOICED, or save as DRAFT and finalize later).

## 1. The GUI flow (how a user creates an invoice today)

Creating an invoice is a **3-step wizard** ("Invoice Page"). The user can finish in one sitting (**save as INVOICED**) or stop midway (**save as DRAFT**) and come back later to finalize.

### Flow at a glance


![[49513988106-image-20260617-052116.png]]



### Step-by-step — what the user does

**Step 1 — Choose the customer**

- Pick an **existing customer** (type-ahead search), or **create a new** one.

- For a new customer: enter company/person + address; the system warns on **possible duplicates**.

**Step 2 — Build the invoice**

- Optional header info: subject, our/your reference, document date, payment terms.

- **Add line items**: search the article catalogue; the GUI **auto-fills** price, VAT, unit and the accounting tags from the article. The user adjusts quantity / price / discount, and **totals recompute live**.

- Optional: attach documents; run a Creditreform credit check.

- Preview the PDF.

- The user may **Save & continue later** here → the invoice is stored as **DRAFT** (no accounting booking yet).

**Step 3 — Deliver & finish**

- Choose a **distribution channel**: Email / ePost / eBill / Print&Send / Print & manual.

- On save the GUI does three things together:

  1.  **Renders the PDF and stores it on the invoice** (kept as the invoice's printed document / `printedFileId`, so it can be re-downloaded later).

  2.  Sets the status to **INVOICED** and **creates the accounting booking** (books into KLARA accounting; inventory transactions are created).

  3.  **Delivers** the stored PDF through the chosen channel.

### The two ways a user ends up with a finalized invoice

1.  **One pass** — create and finish in Step 3 → **INVOICED** immediately (booked).

2.  **Draft first** — "Save & continue later" in Step 2 → **DRAFT**; reopen later, edit, finish → **INVOICED** (booked).

### Checks the user must resolve in the GUI before the invoice is accepted

<div>

|  |  |
|----|----|
| GUI check | What the user must do |
| No line items | Add at least one item before continuing. |
| Item with zero amount | Confirm it is intentional, or fix the amount. |
| Total not rounded | Acknowledge the rounding warning. |
| No distribution channel chosen | Pick a channel before sending. |
| Possible duplicate customer | Confirm it is not a duplicate (new-customer path). |
| Document date in a closing/closed fiscal year | Explicitly confirm before booking. |
| Booking not allowed (subscription) | Acknowledge; invoice is saved without booking. |
| PDF page size invalid / needs resizing | Accept the resize before print & send. |
| Invalid open-position reconciliation | Fix the selected prepayments / credit notes. |
| Delivery (e.g. email) failed | Retry / handle the failure. |

</div>

### What the system fills in automatically (no user action)

Invoice & order **numbers**, article **prices / VAT / accounting tags**, **line and total amounts**, **closing text**, **address validity for the date**, and the **rendered PDF** (which is then **stored on the invoice** and the **accounting booking is created** on finalize). The user mostly makes *decisions*; the application does the *calculation, storage and booking*.

## 2. Why the public API cannot cover the full GUI functionality

The public API exposes the **persistence + accounting** core — create (`POST /core/latest/invoices`), finalize/update a draft (`PUT /core/latest/invoices`), reload (`GET /core/latest/invoices/{id}`), fetch the PDF (`POST /core/latest/invoices/{id}/printed-document`) — plus the lookups needed to build a request. But several capabilities live **inside the GUI/Ivy application**, not in the shared backend the API forwards to. A partner calling the API alone therefore cannot reproduce them:

### GUI vs. Public API — at a glance

<div>

|  |  |  |
|----|----|----|
| Capability | GUI | Public API |
| Select existing customer | ✅ type-ahead picker | ✅ look up, then pass customer id in the body |
| Auto price / VAT / accounting tag per article | ✅ automatic | ❌ caller must fetch and fill |
| Calculate line & total amounts | ✅ live | ❌ caller computes |
| Create invoice (persist) | ✅ | ✅ `POST /invoices` |
| Save as DRAFT | ✅ "Save & continue later" | ✅ `POST` with `status=DRAFT` |
| Reopen & finalize to INVOICED | ✅ | ✅ `GET /invoices/{id}` + `PUT /invoices` |
| Book into accounting | ✅ on INVOICED/SENT | ✅ on INVOICED/SENT |
| Printed PDF stored on invoice & linked to the booking **at booking time** | ✅ automatic, in one step | ⚠️ not at booking time — print (`POST …/invoices/{id}/printed-document`) then link (`PUT …/bookings/{id}/documents`) as a follow-up |
| Render the PDF | ✅ Aspose + company templates | ⚠️ server-rendered only, no customization |
| **Deliver to customer** (Email/ePost/eBill/Print&Send) | ✅ | ❌ **not exposed** |
| GUI safeguards (zero-amount, rounding, no-items, page-size…) | ✅ enforced for the user | ❌ caller's responsibility |
| Inventory / serial numbers | ✅ | ❌ deferred / internal |

</div>

Legend: ✅ supported · ⚠️ partial · ❌ not available.

<div hasbody="true" macro-id="f4877b58-8e52-47bc-8a51-1b6caefff1a5" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

**⚠️ Important — creating an** `INVOICED` **invoice via the API books it, but leaves it without a printed invoice.**

In the GUI, finalizing an invoice renders the PDF, **stores it on the invoice, and creates the accounting booking — all together, in the right order**. Via the public API, `POST /core/latest/invoices` with `status=INVOICED` **creates the accounting booking immediately, but does NOT render or store any PDF**. The resulting invoice *and its booking* have **no printed document attached**. The partner can still reach the same end state, but must run a **3-step sequence** instead of one action:

1.  **Create + book** — `POST /core/latest/invoices` (`status=INVOICED`); the response returns the booking id(s) in `bookingNumbers` (and `businessCaseId`).

2.  **Print the invoice** — `POST /core/latest/invoices/{id}/printed-document` (renders the PDF and stores its `printedFileId` on the invoice).

3.  **Link the document to the booking** — `PUT /core/latest/bookings/{id}/documents` (replaces the booking's document attachments + document date); pass the invoice's `printedFileId` in the `documentIds` array.

So the capability **exists**, but the booking is created **without** a document and the partner must orchestrate the print-and-link steps explicitly — the GUI does all three in a single action. The same applies when finalizing a DRAFT via `PUT /core/latest/invoices`.

</div>

</div>

1.  **The API does no "smart filling" — the GUI does.** In the GUI, choosing an article auto-populates its price, VAT, accounting tag/type, and the screen recomputes line and invoice totals. The public API is a **thin pass-through**: it stores exactly what the caller sends. A partner must fetch article data and compute totals/tags themselves — and if the accounting tag or VAT is wrong or missing, **booking fails**. The GUI shields the user from this; the API does not.

2.  **Sending the invoice to the customer is not exposed.** In the GUI, Step 3 actually **delivers** the invoice (Email / ePost / eBill / Print&Send) through internal delivery services. The public create/update endpoints **only persist and book** — they do **not** send anything, regardless of the chosen distribution method. A partner can retrieve the PDF but must deliver it through their own channel. So the "deliver to customer" half of the GUI flow has **no public equivalent** today (it would be a separate, new API increment).

3.  **PDF rendering uses assets only the GUI application has.** Invoice PDFs are produced inside the Ivy node using Aspose and **company-specific Word / QR-bill templates**. Partners have no access to those templates or the rendering engine, so they cannot reproduce or customize the document — they can only request the server-rendered PDF as-is.

4.  **Many safeguards are GUI-only.** Several checks in the table above (zero-amount confirm, rounding warning, "no items", distribution-required, page-size, duplicate-customer) are enforced by the **GUI**, not by the backend. A direct API caller bypasses them and could submit data a user would have been stopped from sending. To be safe, those rules must be **re-implemented by the partner** — or added to the API as a separate hardening effort.

5.  **The wizard is human-in-the-loop; the API is system-to-system.** Decisions the GUI surfaces as pop-ups (confirm closing fiscal year, confirm zero amounts, choose a channel) become **explicit parameters** the caller must set deliberately (e.g. `confirmDontMindClosingFiscalYear=true`). There is no interactive prompting — the integration must decide everything up front.

6.  **Some supporting features are intentionally not public.** Inventory/stock selection (storages, serial numbers), subscription/entitlement checks, and CRM indicators are **internal-only** or **deferred**, so stock-aware invoicing and the related GUI behaviours are not available through the API.

### Bottom line

The public API lets a partner **create, finalize (book), reload, and fetch the PDF of** an invoice — the **data + accounting core** of the GUI flow, in both the direct-INVOICED and DRAFT→INVOICED paths. It does **not** reproduce the GUI's **convenience layer** (auto pricing/VAT/tagging and total calculation), its **delivery** (sending to the customer), its **document rendering/customization**, or its **interactive safeguards**. A partner integration must supply those itself, and "send to customer" in particular would require a **new, separately-scoped API increment**.

%% ai-graph-start %%

**Related notes:**
- [[Exposing a GUI flow as an API means replacing everything the screen did for the user]]
- [[Invoice API]]
- [[SAP in `luz_finance` — What, Why, How, When]]
- [[Analyze API v2 for Invoice prediction]]
- [[Invoice Processing Steps for KlaraTenant Account]]

%% ai-graph-end %%