---
ai_hash: 4c56d9e86d3a0194
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-23
entities:
- Detailnachweis PDF
- AggregatedBilling key
- Billing record
- Medidata case
- 2026 invoice run
- presentation-level aggregation
- InvoiceRunV2Converter
- convertToInvoiceRun method
- InvoiceDetailData
- product
- main invoice document
- SAP booking export
- InvoiceXpertlineDataExporter
- DB-level AggregatedBilling group-by
- Grouping key
- AggregatedBilling fields
- productVariantCode
- unitPrice
- volume
- price
- consumptionDate
- billedFrom
- billedTo
- Stimulsoft billingDetail mrt
- MengeQuantity column
- Calc columns
- shared model objects
- view
- invoice
- booking
source: session 2026-06-23 LUZ Detailnachweis aggregation
status: seedling
tags:
- luz_store
- invoicing
- billing
- aggregation
- stimulsoft
title: Luz Detailnachweis PDF aggregates billings by product reusing the AggregatedBilling
  key
type: lesson
---

# Luz Detailnachweis PDF aggregates billings by product reusing the AggregatedBilling key

The invoice **Detailnachweis** (detail-proof) PDF used to render **one row per raw `Billing` record**, so a high-volume client (Medidata case, 2026 invoice run) with thousands of records of the same product produced a 400+ page PDF that failed to generate — blocking the whole invoice.

The fix is a **presentation-level aggregation**: group the item's billings by product in `InvoiceRunV2Converter.convertToInvoiceRun(...)` *before* building `InvoiceDetailData`, so each product appears once with summed volume (quantity) and summed amount.

Crucially, the main invoice document and the SAP booking export (`InvoiceXpertlineDataExporter`) **already** collapse records via the DB-level `AggregatedBilling` group-by — only the detail PDF used raw billings. So reusing the *same key* guarantees the detail PDF collapses to the same line count the invoice already produced, and totals are unchanged.

Grouping key = AggregatedBilling fields (productId, pricePlan, featurePricePlan, vatRate, vatIncluded, endToEndId, invoiceNumber, promotionCode, consultantName, costCenter, origin) **plus** productVariantCode + unitPrice. Aggregates: sum(volume), sum(price), min(consumptionDate), min(billedFrom), max(billedTo).

**Why:** aggregation only changes presentation; merging is safe only when price-relevant attributes match, otherwise lines stay separate.
**How to apply:** when a per-record report explodes for high-volume tenants, check whether a sibling flow (invoice/booking) already aggregates and reuse its exact key.

## Related
[[Stimulsoft billingDetail mrt already had the MengeQuantity column and Calc columns]]
[[Copy shared model objects before aggregating them for a view]]

## Related

- [[Stimulsoft billingDetail mrt already had the MengeQuantity column and Calc columns]]
- [[Copy shared model objects before aggregating them for a view]]

%% ai-graph-start %%

**Related notes:**
- [[Aggregation collapses rows only if the key excludes per-record-unique fields]]
- [[Stimulsoft billingDetail mrt already had the MengeQuantity column and Calc columns]]
- [[Copy shared model objects before aggregating them for a view]]
- [[Use Lombok toBuilder for a shallow model copy, not manual setters or deep clone]]
- [[Rounding per line makes net plus VAT miss gross, so the VAT line absorbs the difference]]

**Relations:**
- Detailnachweis PDF — *aggregates billings by* — product
- Detailnachweis PDF — *reuses* — AggregatedBilling key
- Detailnachweis PDF — *previously rendered* — Billing record
- Medidata case — *involved in* — 2026 invoice run
- presentation-level aggregation — *is a fix for* — Detailnachweis PDF
- InvoiceRunV2Converter — *contains method* — convertToInvoiceRun method
- convertToInvoiceRun method — *groups billings by* — product
- convertToInvoiceRun method — *builds* — InvoiceDetailData
- main invoice document — *collapses records via* — DB-level AggregatedBilling group-by
- SAP booking export — *collapses records via* — DB-level AggregatedBilling group-by
- InvoiceXpertlineDataExporter — *is a type of* — SAP booking export
- AggregatedBilling key — *ensures collapse for* — Detailnachweis PDF
- AggregatedBilling key — *matches collapse of* — main invoice document
- Grouping key — *includes* — AggregatedBilling fields
- Grouping key — *includes* — productVariantCode
- Grouping key — *includes* — unitPrice
- Grouping key — *aggregates* — volume
- Grouping key — *aggregates* — price
- Grouping key — *aggregates* — consumptionDate
- Grouping key — *aggregates* — billedFrom
- Grouping key — *aggregates* — billedTo
- Stimulsoft billingDetail mrt — *contains* — MengeQuantity column
- Stimulsoft billingDetail mrt — *contains* — Calc columns
- shared model objects — *are aggregated for* — view
- invoice — *is a sibling flow to* — Detailnachweis PDF
- booking — *is a sibling flow to* — Detailnachweis PDF

%% ai-graph-end %%