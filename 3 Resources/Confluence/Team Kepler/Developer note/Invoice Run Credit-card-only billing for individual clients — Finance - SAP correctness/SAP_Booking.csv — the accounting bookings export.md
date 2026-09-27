---
title: "SAP_Booking.csv — the accounting bookings export"
created: 2026-07-27
updated: 2026-07-27
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49617567993/SAP_Booking.csv+the+accounting+bookings+export
confluence_id: "49617567993"
confluence_path: "Team Kepler > Developer note > [Invoice Run] Credit-card-only billing for individual clients — Finance / SAP correctness"
tags: [confluence, invoice-run, sap]
---

# SAP_Booking.csv — the accounting bookings export

*Confluence source · Team Kepler › Developer note › [Invoice Run] Credit-card-only billing for individual clients — Finance / SAP correctness · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49617567993/SAP_Booking.csv+the+accounting+bookings+export) · updated 2026-07-27*

> **In one sentence:** `SAP_Booking_<timestamp>.csv` tells SAP **how to post each invoice** — one header line, then, per invoice, an invoice-header line, one detail line per account+VAT-rate, a VAT-info line, and (if paid by card) two credit-card lines.

Part of the SAP export feature — see the `feature overview`. This page is only about the **booking** file. Its sibling is `SAP_Master.csv`.

|  |  |
|----|----|
| Picture | Covers |
| ![[booking-01-lines.png]] | **process · flow · dependencies** — which builder fills which line, and what each sets |
| ![[booking-02-amounts.png]] | **concepts** — net + VAT = gross, and the rounding adjustment |
| ![[image-20260727-072209.png]] | Sample of a file look like |

### WHAT

A plain-text file, **UTF-8**, fields separated by `;`, named `SAP_Booking_<yyMMddHHmmss>.csv`. Each line is modelled by `SAPBookingReport` (20 string fields). The lines for one invoice appear in this order:

1.  **HEADER** — once per file, from `SAPReport.bookingHeader` (template).

2.  **INVOICE HEADER** — the invoice's "gross" posting (amount **incl. VAT**).

3.  **DETAIL LINE × N** — one per `(accountingNumber + VAT rate)` group, amount **excl. VAT**.

4.  **VAT-INFO** — the VAT amount (rounding-adjusted so the invoice balances).

5.  **CREDIT SUB-HEADER + CREDIT-CARD line** — *only if the customer paid by credit card*.

### WHY

- `SAP_Master.csv` says *who* the customer is;

- `SAP_Booking.csv` is the actual **accounting entry**:

  - the amounts, the VAT code, the general-ledger / accounting number, and the **posting key** that tells SAP whether it is a debit or a credit.

  - This is what turns an invoice into a booking in the ledger.

### WHEN

- The user clicks **"Download SAP booking file"** on an invoice-run detail page (only from state `INVOICES_CHARGING_AND_PDF_CREATING` onward).

- `generateSAPMasterAndBookingFiles(...)` builds the booking file **first**, then the master file, zips
  both, and the browser downloads the ZIP.

- Zero-amount invoices are skipped;

- KLARA Relax invoices are split so each billing gets its own booking.

### HOW (see *booking-01* and *booking-02*)

- The method `exportSAPBooking(...)` writes the header, then loops over invoices.

- Each **line type has its own builder** that clones a template from `SAPReport` and fills in the dynamic values:

|  |  |  |
|----|----|----|
| Line | Builder | Key values it sets |
| INVOICE HEADER | `buildSAPInvoiceHeader` | `invoiceNumber`, `invoiceDate`, `postingDate`, `sapCustomer`, `amount` = total **incl. VAT** |
| DETAIL × N | `buildSAPInvoiceDetail` | `amount` = **excl. VAT**, `vatCode`, `accountingNumber`; credit note → `postingKey` 40 + amount negated |
| VAT-INFO | `buildSAPVATInformation` | `amount` = rounding-adjusted VAT-exclusive amount |
| CREDIT SUB-HEADER | `buildSAPInvoiceSubHeader` | dates = run start, `sapCustomer`, `amount` = invoice total |
| CREDIT-CARD | `buildSAPPaymentByCreditCard` | `amount` = invoice total |

Each object becomes a `;`-joined line via `SapCSVUtil.convertBookingToString` (20 fields, in order: `bk ; invoiceType ; invoiceDate ; postingDate ; currency ; invoiceNumber ; postingKey ; sapCustomer ; amount ; vatCode ; postingText ; materialNumber ; product ; profitCenter ; accountingNumber ; quantity ; unit ; adm ; zbed ; sapAcount`).

#### The business rules (the part a code map can't see)

These live in `SapCSVUtil` and are pinned down by `SapCSVUtilTest`:

- **VAT rate → SAP VAT code** (`generateVATCode`): `7.7 % → A1`, `8.1 % → C1`, `0 %/none → AL`.

- **Posting key** (`generatePostingKey`): normal booking → default (e.g. `50`); **credit note / negative amount →** `40` and the amount is negated (`LUZ-111391`).

- **Rounding** (`adjustSAPAmountForVATInformation`, see *booking-02*): each detail line is rounded on its own, so `net + raw-VAT` can miss the gross by a cent. The VAT-info amount is nudged so **net + VAT = gross** exactly. Test example: gross `178.85`, net `166.05`, raw VAT `12.75` → adjusted `12.80` (`166.05 + 12.80 = 178.85`).
