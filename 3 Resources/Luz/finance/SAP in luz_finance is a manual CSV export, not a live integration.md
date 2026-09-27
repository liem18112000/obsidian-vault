---
ai_hash: 602c81ad702dc44d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: SAP in luz_finance — What, Why, How, When (2026-07-27)'
status: seedling
tags:
- sap
- luz-finance
- invoice-run
- accounting
- kepler
- naming
title: SAP in luz_finance is a manual CSV export, not a live integration
type: observation
---

# SAP in luz_finance is a manual CSV export, not a live integration

In `luz_finance`, "SAP" is **not a live integration**. There is no connection to an SAP system, no API, no sync. It is a **manual, one-way file export**: a user clicks *"Download SAP booking file"*, the code writes two `;`-separated UTF-8 CSVs, zips them as `SAP_master_and_booking_files_<timestamp>.zip`, and a human imports them into SAP by hand.

- `SAP_Master_<ts>.csv` — the **customer master** (debtors: name, address, VAT id, email).
- `SAP_Booking_<ts>.csv` — the **accounting postings** (amounts, VAT codes, accounts).

Worth writing down because the name actively misleads. Seeing "SAP" in a dependency list or a ticket suggests an integration with the failure modes of one — retries, idempotency, connectivity, reconciliation. None of those exist here. The real failure modes are a **human forgetting to download**, **downloading twice**, or **importing a file built with stale defaults**.

The button is deliberately gated on the invoice-run lifecycle — in V2 it appears only for `INVOICES_CHARGING_AND_PDF_CREATING`, `INVOICES_CHARGED_AND_PDF_CREATED`, `INVOICES_SENDING`, `FINISHED`, i.e. **after invoices are charged and PDFs exist** — because exporting before that would post bookings for invoices that may still change.

General lesson: **when a component is named after an external system, check whether it talks to it.** "The X integration" is often a file drop, and the distinction changes every assumption about consistency.

## Related

- [[SAP export defaults come from the default-sap config service so accounting codes change without a deploy]]
- [[Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference]]

## Related

- [[SAP export defaults come from the default-sap config service so accounting codes change without a deploy]]
- [[Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference]]

%% ai-graph-start %%

**Related notes:**
- [[SAP export defaults come from the default-sap config service so accounting codes change without a deploy]]
- [[SAP in `luz_finance` — What, Why, How, When]]
- [[Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference]]
- [[SAP_Booking.csv — the accounting bookings export]]
- [[SAP_Master.csv — the customer master export]]

%% ai-graph-end %%