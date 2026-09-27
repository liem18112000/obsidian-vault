---
ai_hash: 95581126f967d01c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49691787346'
confluence_path: LUZ Home > 100_General > 02_Joint Review > Joint review 0.03.28.00
  (11.08.2026 - 24.08.2026)
created: 2026-08-24
entities: []
source: Confluence · LUZ - LUZ
status: reference
tags:
- confluence
- invoice-run
title: HealthCare And Invoice Run (11.08.2026 - 24.08.2026)
type: source
updated: 2026-08-24
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49691787346/HealthCare+And+Invoice+Run+11.08.2026+-+24.08.2026
---

# HealthCare And Invoice Run (11.08.2026 - 24.08.2026)

*Confluence source · LUZ Home › 100_General › 02_Joint Review › Joint review 0.03.28.00 (11.08.2026 - 24.08.2026) · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49691787346/HealthCare+And+Invoice+Run+11.08.2026+-+24.08.2026) · updated 2026-08-24*

## 1. Invoice Run

> Today, when an **individual (private)** ePost client's credit-card charge fails in the invoice run, we fall back to a QR-code invoice. We want to move individual clients to **credit-card-only** billing (no QR fallback) with a clear dunning flow and customer notifications — to cut the card failed-rate and the QR/support overhead. **Company clients keep the QR fallback** but gain a failed-charge notification.

### 1.1. New State: CREDIT_CARD_CHARGED_PENDING

The state will exist between the two steps: INVOICES_CHARGED_AND_PDF_CREATED and FINISHED.

> [!note]- INVOICES_CHARGED_AND_PDF_CREATED
>
> ![[2f8b1bd9-8100-4bf6-88df-bd938290f03c.png]]
>

> [!note]- FINISHED
>
> ![[image-1787543019748.png]]
>

We have a **retry mechanism** to run on the **10th** and **20th** of **each month** to re-charge invoices that are in the PENDING state.

- if it continues to fail, we send notifications to users. SAP Booking file will remove this invoice out of file.

> [!note]- Email contents
>
> ![[image-20260824-035354.png]]![[image-20260824-035446.png]]
>

- if it charged successfully, we send invoice PDF file to users.

In the next invoice run, if the PENDING state of this invoice still be there, we will **UNSUBSCRIBE** those widgets.

### 1.2. New GUI for monitoring

We also support an interface that allows administrators to check changes related to invoices that were charged but failed during the invoice run and automatic retry attempts, as well as unsubscribe and email notifications.

> [!note]- Click here to expand...
>
> ![[image-20260824-040154.png]]![[image-20260824-040230.png]]
>

## 2. HealthCare Import

### 2.1. How it works

One upload → virus-scanned → unpacked & organized → filed into secure storage → ready in the KLARA myLife app.

![[image-20260824-042502.png]]

### 2.2. Architecture

![[image-20260824-064623.png]]

### 2.3. Live demo

Recorded end-to-end on **dev.klara.tech** (profile *Liem Doan · KLARA myLife*): upload `happy-spec-example.zip` → success screen → open Digital Letterbox → Storage.

[[luz-docs-import-demo.mp4|luz-docs-import-demo.mp4]]

%% ai-graph-start %%

**Related notes:**
- [[Invoice Run & luz-docs Archive Improvements]]
- [[Invoice Run – Credit Card Payment Only for Individual Runs]]
- [[Joint review 0.03.30.00 (08.09.2026 - 21.09.2026)]]
- [[Joint review 0.03.28.00 (11.08.2026 - 24.08.2026)]]
- [[ePost API (28.02.2023 - 13.03.2023)]]

%% ai-graph-end %%