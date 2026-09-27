---
ai_hash: 8ca66714445b90be
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49504649471'
confluence_path: Team Kepler > Developer note
created: 2026-06-15
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
- sprint
title: Sprint 158 - Invoice Run V2 Executive Overview
type: source
updated: 2026-06-15
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49504649471/Sprint+158+-+Invoice+Run+V2+Executive+Overview
---

# Sprint 158 - Invoice Run V2 Executive Overview

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49504649471/Sprint+158+-+Invoice+Run+V2+Executive+Overview) · updated 2026-06-15*

## Overview

Invoice Run V2 has improved in three important areas:

1.  **Invoice accuracy**,

2.  **Delivery behavior**,

3.  **Production stability**.

The team delivered several fixes that reduce billing errors, improve default invoice delivery for business tenants, and prevent full-batch failures during sending.

The main open issue is a billing-address rendering defect in the PDF output, with a small set of follow-up improvements still planned.

## What we have delivered

- **Billing address support improved** — invoices now support a dedicated billing address configuration across backend and frontend flows.

- **Letterbox delivery enabled for business tenants** — ePost Service AG invoices are now routed to the Letterbox by default for company tenants.

- **Batch resilience improved** — one failed item in the sending step no longer causes the entire batch to fail.

- **Production stability restored** — fixes were delivered for `luzfin-finance` container restarts caused by thread-pool exhaustion.

- **Duplicate payment behavior corrected** — processed payments no longer incorrectly reappear in the Pay section.

## Current open risk

**Billing address street line missing in PDF** `IN PROGRESS`

When a separate billing address is used, the invoice PDF can omit the street line in the receiver address window. This is the primary active defect still affecting output quality.

## Next improvements in scope

- **Credit-card receipt misclassification** — resolve incorrect identification of paid receipts as invoices with a Pay action.

- **Email delivery opt-in** — add Company Settings support for email as an additional invoice delivery channel.

- **Letterbox provisioning fallback** — improve handling for clients who do not yet have a Letterbox.

- **PDF product aggregation** — combine identical product lines into a single summarized line with consumption count.

%% ai-graph-start %%

**Related notes:**
- [[Invoice Run & luz-docs Archive Improvements]]
- [[Troubleshooting articles]]
- [[HealthCare And Invoice Run (11.08.2026 - 24.08.2026)]]
- [[Invoice Run V2UAT - Execute - Apply Distributed Cache for customer information during the process of Invoice Run V2ecute]]
- [[Invoice Run V2UAT - Update latest Stimulsoft template - Execution]]

%% ai-graph-end %%