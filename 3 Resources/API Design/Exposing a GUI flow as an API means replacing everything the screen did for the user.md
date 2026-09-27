---
ai_hash: 11d5a0b46ae8d5f5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Creating invoice GUI flow vs Public API (NEXT)'
status: seedling
tags:
- api-design
- public-api
- ux
- product
- scoping
- confluence-distilled
title: Exposing a GUI flow as an API means replacing everything the screen did for
  the user
type: lesson
---

# Exposing a GUI flow as an API means replacing everything the screen did for the user

"Just expose what the GUI does as an API" underestimates the work, because most of what a GUI does is not the final write — it is the **assistance** that gets the user to a valid payload. An API caller inherits all of it.

Walking one invoice-creation wizard shows what the screen was quietly doing:

**Step 1 — choose the customer**
- type-ahead search over existing customers
- create-new inline, with a **possible-duplicate warning**

**Step 2 — build the invoice**
- article-catalogue search, with **auto-fill of price, VAT, unit and accounting tags** from the chosen article
- **live recomputation** of totals as quantity, price and discount change
- optional attachments, an optional external credit check
- **PDF preview**
- **Save & continue later** → stored as `DRAFT`, with no accounting booking yet

**Step 3 — deliver & finish**
- choose a distribution channel (Email / ePost / eBill / Print&Send / print manually)
- on save, several things happen **together**: render the PDF and store it on the invoice as the printed document (`printedFileId`, so it can be re-downloaded), plus the accounting and delivery actions

**What the API caller now has to do themselves:** decide whether a customer already exists (no duplicate warning), know the article's price, VAT, unit and accounting tags (no auto-fill), compute the totals correctly (no live recompute), and accept the document without previewing it.

> [!tip] Enumerate the assistances, then decide each one
> For every GUI affordance, pick: **replicate** it as an API (a duplicate-check endpoint, an article-lookup that returns defaults), **push** it to the caller and document the expectation, or **declare it out of scope**. Writing that list *is* the API design. Skipping it produces an API that technically creates invoices and is unusable without reverse-engineering the GUI.

> [!warning] Compound actions and lifecycle states are the easy things to miss
> Two details from this flow generalise. **Save does several things at once** — render, store, book, dispatch — so an API modelling only "create invoice" silently omits the PDF the GUI would have produced and attached. And **DRAFT is a real state** with different semantics (no accounting booking). An API that only exposes the finished state removes the save-and-return-later workflow entirely.

Related: [[Put a public API adapter between external callers and internal services]] — where this translation work belongs.

Source: [[Creating invoice — GUI flow vs. Public API]] (NEXT, Confluence).

## Related

- [[Put a public API adapter between external callers and internal services]]

%% ai-graph-start %%

**Related notes:**
- [[Creating invoice — GUI flow vs. Public API]]

%% ai-graph-end %%