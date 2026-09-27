---
ai_hash: ca06962b47a041c8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49618092035'
confluence_path: Team Kepler > Developer note > [Invoice Run] Credit-card-only billing
  for individual clients — Finance / SAP correctness
created: 2026-07-27
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
- sap
title: SAP in `luz_finance` — What, Why, How, When
type: source
updated: 2026-07-27
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49618092035/SAP+in+luz_finance+What+Why+How+When
---

# SAP in `luz_finance` — What, Why, How, When

*Confluence source · Team Kepler › Developer note › [Invoice Run] Credit-card-only billing for individual clients — Finance / SAP correctness · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49618092035/SAP+in+luz_finance+What+Why+How+When) · updated 2026-07-27*

> **In one sentence:** "SAP" in this project is **not** a live connection to an SAP system — it is a feature that **writes two CSV files** (`SAP_Master.csv` + `SAP_Booking.csv`) from KLARA AG's own invoices, so a person can **download them and import them into SAP by hand**.

This folder documents everything about SAP in the code, in plain language, with pictures.

|  |  |
|----|----|
| Picture | What it explains |
| ![[01-concept.png]] | **Concept** — what "SAP" really is here (a one-way file export) |
| ![[02-dependencies.png]] | **Dependencies** — the classes involved and who uses whom |
| ![[03-flow.png]] | **Flow** — when the button appears and what it does step by step |
| ![[04-booking-process.png]] | **Process** — how one invoice becomes booking lines, and the VAT/posting-key rules |

------------------------------------------------------------------------

### WHAT is it?

A pair of **export files** that KLARA AG's finance team can load into their **SAP accounting system**:

- `SAP_Master_<timestamp>.csv` — the **customer master** (who was invoiced: name, address, VAT id, email).

- `SAP_Booking_<timestamp>.csv` — the **accounting bookings** (the invoice postings: amounts, VAT, accounts).

Both files are plain text, one record per line, **fields separated by** `;`, written in UTF-8. When the user clicks the button, the two files are zipped together as `SAP_master_and_booking_files_<timestamp>.zip` and downloaded by the browser.

### WHY does it exist?

KLARA AG bills its own customers for their subscriptions ("widget invoices"). That revenue has to be recorded in KLARA's **corporate bookkeeping**, which runs on **SAP**. So after an invoice run, the finance team needs the invoices and the customers in a shape SAP can import:

- SAP needs **customer master records** (debtors) → `SAP_Master.csv`.

- SAP needs **financial postings** with the right VAT codes, posting keys and accounts → `SAP_Booking.csv`.

Because the codes must match SAP's own configuration (VAT codes, posting keys, contact qualifiers, account numbers, headers), those defaults are **not hard-coded** — they are fetched from a configuration service (`/default-sap`) and merged into the files at export time.

### WHEN does it run?

It is **manual** — a user clicks **"Download SAP booking file"** on an invoice-run detail page. The button only appears at the right point in the **invoice-run lifecycle** (diagram 3):

- **V2 (**`InvoiceRunDetailV2`**)** — the button is shown only when the run is in one of: `INVOICES_CHARGING_AND_PDF_CREATING`, `INVOICES_CHARGED_AND_PDF_CREATED`, `INVOICES_SENDING`, `FINISHED` — i.e. **after invoices are charged and PDFs are made**, and it stays disabled until preparation is finished.

- **V1 (**`WidgetInvoiceDetail`**)** — the button is shown only when the feature switch `featureSwitchBean.isKlaraInvoiceRunSAPExportEnabled()` is on.

Clicking runs the HTML-dialog process method `downloadSAPBookingFile()`, which calls Step A + Step B, zips the result, and streams it to the browser via PrimeFaces `p:fileDownload`.

### HOW does it work?

Two steps: **get the defaults**, then **build the two files**.

#### Step A — get the SAP defaults

`DefaultSapContainer.getDefaultSAPValues()` (`DefaultSapContainer.java`) calls the REST endpoint `FINANCE_PUBLIC` **→** `/default-sap` and receives a `SAPReport` object. `SAPReport` is a template bag holding:

- header + content templates for the master and booking files,

- the default **VAT codes**, **posting keys**, and **contact qualifiers**.

If `masterHeader` or `bookingHeader` come back empty, it throws `ClientException` (nothing is exported).

#### Step B — build the files

`generateSAPMasterAndBookingFiles(...)` does the work. It exists in two places (see *When* below):

- V1: `WidgetInvoiceDataExportBean` (`WidgetInvoiceDataExportBean.java:189`)

- V2: `AbstractInvoiceRunDetailController` (`AbstractInvoiceRunDetailController.java:322`)

Both: 1. **Drop zero-amount invoices** (`filterInvoiceItemsHasNoZeroAmount`). 2. **Build the booking file** (`exportSAPBooking`) — see diagram 4 for the line-by-line anatomy. 3. **Build the master file** (`exportSAPMaster`) — a header line, then one customer line per invoice. 4. Each object → one CSV line via `SapCSVUtil.convertBookingToString` **/** `convertMasterToString` (`SapCSVUtil.java`).

`SapCSVUtil` also holds the **business rules** for mapping values (see §6):

- `generateVATCode(rate, ...)` — VAT rate → SAP VAT code.

- `generatePostingKey(...)` — pick the posting key (credit notes differ).

- `adjustSAPAmountForVATInformation(...)` — fix rounding so the VAT line balances.

- `escapeForCsv(...)` — quote/escape fields that contain `;`, `"`, newlines.

### The business rules (read these from the tests!)

These are the "logic" a structural code map can't see — they live in `SapCSVUtil` and are pinned down by `SapCSVUtilTest`:

**VAT rate → SAP VAT code** (`generateVATCode`):

|              |                |
|--------------|----------------|
| VAT rate     | SAP code       |
| `7.7 %`      | `A1`           |
| `8.1 %`      | `C1`           |
| `0 %` / none | `AL` (default) |

**Posting key** (`generatePostingKey`): a normal booking uses the default posting key; a **negative amount (credit note)** uses `40` and the amount is negated (ticket `LUZ-111391`).

**Rounding** (`adjustSAPAmountForVATInformation`): the VAT-info line's amount is nudged up or down so `included = excluded + vat-exclusive` stays exact after per-line rounding.

> The concrete codes above are what the tests assert. The *live* values (which codes for which tenant) come from the `/default-sap` endpoint at runtime — so treat `A1/C1/AL/40/50` as examples, not constants.

%% ai-graph-start %%

**Related notes:**
- [[SAP_Booking.csv — the accounting bookings export]]
- [[SAP in luz_finance is a manual CSV export, not a live integration]]
- [[SAP_Master.csv — the customer master export]]
- [[SAP export defaults come from the default-sap config service so accounting codes change without a deploy]]
- [[Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference]]

%% ai-graph-end %%