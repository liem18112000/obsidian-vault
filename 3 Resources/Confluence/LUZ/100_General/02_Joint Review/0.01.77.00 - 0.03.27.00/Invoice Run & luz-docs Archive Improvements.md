---
title: "Invoice Run & luz-docs Archive Improvements"
created: 2026-06-29
updated: 2026-06-29
type: source
status: reference
source: "Confluence · LUZ - LUZ"
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49542430803/Invoice+Run+luz-docs+Archive+Improvements
confluence_id: "49542430803"
confluence_path: "LUZ Home > 100_General > 02_Joint Review > 0.01.77.00 - 0.03.27.00 > Joint review 0.03.24.00 (16.06.2026 - 29.06.2026)"
tags: [confluence, invoice-run, luz-docs]
---

# Invoice Run & luz-docs Archive Improvements

*Confluence source · LUZ Home › 100_General › 02_Joint Review › 0.01.77.00 - 0.03.27.00 › Joint review 0.03.24.00 (16.06.2026 - 29.06.2026) · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49542430803/Invoice+Run+luz-docs+Archive+Improvements) · updated 2026-06-29*

## Sprint 159 Deliverables

### 1. Invoice Run - Invoice and Receipt

- **Problem:** When Invoice Run sends an invoice that has already been paid by credit card, the ePost app still displays the “Pay” button. This creates a risk that customers may pay the same invoice twice.

- **Solution:** When an invoice has already been charged to a credit card and is sent via Invoice Run, we will add metadata so the ePost app can recognize the paid status and hide the “Pay” button.

### 2. Invoice Run - Sending Invoice via Digital Channel as Default

- **Problem:** Some customers report that they cannot find invoices sent through the Invoice Run process, which prevents them from paying on time.

- **Solution:** To simplify delivery, Invoice Run now sends invoices via the Digital Channel by default, even if the user has not subscribed to the Digital Letterbox widget.

  ![[image-20260629-063309.png]]

### 3. Invoice Run - Additional Delivery Method as Billing email

- **Solution:** Because the Digital Channel is now the default delivery channel, customers who also want to receive invoices by email can receive an additional copy when a billing email is configured in Customer/Partner.

### 4. Archive at scale (luz-docs)

- Problem: Deleting an eArchive folder with tens of thousands of documents handled them one-by-one and **timed out past a few thousand documents**.

- Solution: Deletion is now **batched and index-backed**, scaling past **100,000**
  **documents per folder**. → *Large clean-ups finish reliably without stalling the system.*

## **Net effect:**

- Fewer wrong payments due to wrong “Pay“ button

- Invoices delivered the customer's way (digital and email)

- More digital reach

- Deletion archive can scales.

## References

[[3 Resources/Confluence/LUZ/100_General/02_Joint Review/0.01.77.00 - 0.03.27.00/attachments/invoice-run-luz-docs-archive-improvements/Sprint-159-Customer-Experience-EN-avatar.mp4|Sprint-159-Customer-Experience-EN-avatar.mp4]]

## Attachments

*Attached to the Confluence page but not embedded in its body.*

- [[3 Resources/Confluence/LUZ/100_General/02_Joint Review/0.01.77.00 - 0.03.27.00/attachments/invoice-run-luz-docs-archive-improvements/Sprint-159-Customer-Experience.pptx|Sprint-159-Customer-Experience.pptx]]
