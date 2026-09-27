---
title: "SAP_Master.csv — the customer master export"
created: 2026-07-27
updated: 2026-07-27
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49617568019/SAP_Master.csv+the+customer+master+export
confluence_id: "49617568019"
confluence_path: "Team Kepler > Developer note > [Invoice Run] Credit-card-only billing for individual clients — Finance / SAP correctness"
tags: [confluence, invoice-run, sap]
---

# SAP_Master.csv — the customer master export

*Confluence source · Team Kepler › Developer note › [Invoice Run] Credit-card-only billing for individual clients — Finance / SAP correctness · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49617568019/SAP_Master.csv+the+customer+master+export) · updated 2026-07-27*

> **In one sentence:** `SAP_Master_<timestamp>.csv` tells SAP **who was invoiced** — one header line, then one "customer master" line per invoice (name, address, VAT id, email, plus fixed SAP bookkeeping settings).

Part of the SAP export feature — see the `feature overview` for the big picture. This page is only about the **master** file. Its sibling is `SAP_Booking.csv`.

|  |  |
|----|----|
| Picture | Covers |
| ![[master-01-flow.png]] | **concept · flow · dependencies** — how the file is built and where the data comes from |
| ![[master-02-fields.png]] | **process · concepts** — the 25 fields, their source, and what the cryptic ones mean |
| ![[image-20260727-072423.png]] | Sample of a file look like |

### WHAT

A plain-text file, **UTF-8**, fields separated by `;`, named `SAP_Master_<yyMMddHHmmss>.csv`.

- **Line 1** = a fixed **HEADER** line (column titles / control row), taken straight from configuration.

- **Lines 2…N** = one **customer-master line per invoice** in the run — 25 fields each.

In SAP terms this is the **debtor (customer) master**: the record SAP keeps for each customer it can bill. Each line is modelled by the Java class `SAPMasterReport` (25 string fields).

### WHY

- Before SAP can post an invoice against a customer, it needs that customer to **exist as a debtor** with an address, a language, a reconciliation account, payment terms, etc.

- So the export sends the customer master **together with** the bookings: `SAP_Master.csv` creates/updates the debtors, `SAP_Booking.csv` posts against them.

### WHEN

- Exactly the same moment as the booking file the user clicks **"Download SAP booking file"**

- `generateSAPMasterAndBookingFiles(...)` builds **both** files (booking first, then master) and zips them.

- Zero-amount invoices are skipped.

### HOW

The method `exportSAPMaster(...)` builds the file:

1.  Write the **HEADER** line once, from `SAPReport.masterHeader` (a template that comes from the `/default-sap` config endpoint) via `SapCSVUtil.convertMasterToString`.

2.  For **each invoice**: - clone the **template** `SAPReport.masterContent` (holds the fixed SAP fields that are the same for every customer), - call `buildMasterContent(...)` to **overwrite the dynamic fields** with this customer's data, - turn the object into a `;`-joined line with `convertMasterToString` (25 fields, in order).

So every customer line = *fixed template fields* + *this customer's fields*.
