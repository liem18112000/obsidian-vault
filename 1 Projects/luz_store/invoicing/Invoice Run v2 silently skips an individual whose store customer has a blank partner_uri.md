---
ai_hash: cc1369530d2c3865
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-04
entities:
- Invoice Run v2
- individual
- store customer
- partner_uri
- luz_store
- billing row
- run-selection stamp
- invoice_run_uuid
- invoice_item
- item-building
- InvoiceRunServiceV2
- getStoreCustomerByCustomerIds
- service/InvoiceRunServiceV2.java
- StringUtils.isNotBlank
- customer
- buildBasicInfoInvoiceItems
- WARNING InvoiceRunServiceV2 Skip invoicing of store customer id <ID> due to missing
  customer information
- getTotalAmount
- getCustomerCache
- luzfin_finance customer
- BillingDao.updateBillingsWithUuidForInvoicing
- price_plan
- no_invoiced
- invoice_number
- tenant_type
- dev logs
- company_uri
- customer.id
- Skip invoicing of store customer
- luzfin_finance record
- luzfin_finance
- freshly-seeded/incomplete individual customers
- properly-onboarded ones
- individual 9388f0ab
- store customer 392709
- run 2778
- d675a04b
- 955 billing rows
- 1 invoice item
- individual 3fce29c4
- customer 384940
- prepareInvoiceData timestamp
- seeding failing individual charges
- seeded billing row
- Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state fields
source: session 2026-08-04 dev investigation
status: seedling
tags:
- luz_store
- invoice-run-v2
- partner_uri
- individual
- gotcha
- seeding
title: Invoice Run v2 silently skips an individual whose store customer has a blank
  partner_uri
type: lesson
---

# Invoice Run v2 silently skips an individual whose store customer has a blank partner_uri

In **Invoice Run v2** (luz_store), a `billing` row can pass the run-selection **stamp** (get `invoice_run_uuid` set) yet never become an `invoice_item` — because item-building drops any store customer whose **`partner_uri` is blank**.

## The filter
`InvoiceRunServiceV2.getStoreCustomerByCustomerIds` (`service/InvoiceRunServiceV2.java:653`) builds the customer map with:
```java
storeCustomers.stream().filter(item -> StringUtils.isNotBlank(item.get_partnerUri()))
```
So a `customer` with empty `partner_uri` is excluded. Then `buildBasicInfoInvoiceItems` (L668-671) finds `mapStoreCustomer.get(id) == null` and logs:
```
WARNING InvoiceRunServiceV2  Skip invoicing of store customer id <ID> due to missing customer information
```
and produces no invoice item. `partner_uri` is also needed downstream at `getTotalAmount`/`getCustomerCache` (L694), so it must point to a REAL luzfin_finance customer, not just be non-blank.

## Why this is easy to miss
- The billing **selection/stamp** (`BillingDao.updateBillingsWithUuidForInvoicing`) does NOT check the customer at all — it filters only on price_plan / no_invoiced / invoice_number / tenant_type. So the row looks "collected" (has invoice_run_uuid) but isn`t invoiced.
- The skip is logged by **store customer id**, NOT by company_uri — so grepping dev logs for the profile/company URI in the calc window finds nothing. Search by the numeric `customer.id`, or for the literal string `Skip invoicing of store customer`.
- `partner_uri` is the link to the customer`s luzfin_finance record (`/luzfin_finance/api/<financeTenant>/companies/1/customers/<customerId>`). Freshly-seeded/incomplete individual customers often lack it; properly-onboarded ones have it.

## Verified on dev (2026-08-04)
Individual `9388f0ab` (store customer id 392709, blank partner_uri) was skipped by run 2778 (`d675a04b`), which stamped 955 billing rows but produced only 1 invoice item — for a different individual (`3fce29c4`, customer 384940, which HAS a partner_uri). Log line seen at the exact prepareInvoiceData timestamp.

Relates to seeding failing individual charges — a seeded billing row is useless if its customer has no partner_uri. See [[Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state fields]].

## Related

- [[Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state fields]]

%% ai-graph-start %%

**Related notes:**
- [[Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state fields]]
- [[Invoice run v2 shows charge failures via verbatim message copy at controller line 628]]
- [[Write-time localization into the existing message column avoids schema change]]
- [[Observed Payrexx prose vocabulary in dev is only three messages]]
- [[DECLINED status falls through invoice charge-failure handling in luz_store]]

**Relations:**
- Invoice Run v2 — *operates_in* — luz_store
- Invoice Run v2 — *skips* — individual
- individual — *has* — store customer
- store customer — *has* — partner_uri
- partner_uri — *is* — blank
- billing row — *passes* — run-selection stamp
- run-selection stamp — *sets* — invoice_run_uuid
- billing row — *never_becomes* — invoice_item
- item-building — *drops* — store customer
- blank partner_uri — *causes* — item-building drops store customer
- InvoiceRunServiceV2 — *contains_method* — getStoreCustomerByCustomerIds
- getStoreCustomerByCustomerIds — *located_in* — service/InvoiceRunServiceV2.java
- getStoreCustomerByCustomerIds — *uses* — StringUtils.isNotBlank
- StringUtils.isNotBlank — *checks* — partner_uri
- customer — *has* — partner_uri
- empty partner_uri — *causes* — customer is excluded
- InvoiceRunServiceV2 — *contains_method* — buildBasicInfoInvoiceItems
- buildBasicInfoInvoiceItems — *logs* — WARNING InvoiceRunServiceV2 Skip invoicing of store customer id <ID> due to missing customer information
- buildBasicInfoInvoiceItems — *produces_no* — invoice_item
- partner_uri — *needed_by* — getTotalAmount
- partner_uri — *needed_by* — getCustomerCache
- partner_uri — *points_to* — luzfin_finance customer
- BillingDao.updateBillingsWithUuidForInvoicing — *does_not_check* — customer
- BillingDao.updateBillingsWithUuidForInvoicing — *filters_on* — price_plan
- BillingDao.updateBillingsWithUuidForInvoicing — *filters_on* — no_invoiced
- BillingDao.updateBillingsWithUuidForInvoicing — *filters_on* — invoice_number
- BillingDao.updateBillingsWithUuidForInvoicing — *filters_on* — tenant_type
- skip — *logged_by* — store customer id
- skip — *not_logged_by* — company_uri
- dev logs — *searchable_by* — customer.id
- dev logs — *searchable_by* — Skip invoicing of store customer
- partner_uri — *links_to* — luzfin_finance record
- luzfin_finance record — *part_of* — luzfin_finance
- freshly-seeded/incomplete individual customers — *lack* — partner_uri
- properly-onboarded ones — *have* — partner_uri
- individual 9388f0ab — *has_store_customer_id* — store customer 392709
- individual 9388f0ab — *has_blank* — partner_uri
- individual 9388f0ab — *skipped_by* — run 2778
- run 2778 — *has_uuid* — d675a04b
- run 2778 — *stamped* — 955 billing rows
- run 2778 — *produced* — 1 invoice item
- 1 invoice item — *for* — individual 3fce29c4
- individual 3fce29c4 — *has_customer_id* — customer 384940
- individual 3fce29c4 — *has* — partner_uri
- log line — *seen_at* — prepareInvoiceData timestamp
- seeded billing row — *useless_if_customer_has_no* — partner_uri
- Invoice Run v2 — *relates_to* — Failed INDIVIDUAL Invoice Run v2 charge is tracked by three distinct state fields

%% ai-graph-end %%