---
ai_hash: 81267380b2db8626
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.792
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47470903877/Adapt+Order+Management+api+to+include+accounting+tags
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Adapt Order Management api to include accounting tags
topic: programming
type: source
updated: 2023-08-28
---

# Adapt Order Management api to include accounting tags

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-08-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47470903877/Adapt+Order+Management+api+to+include+accounting+tags)
> Relevance 0.792 · topic `programming`

## Introduce new query param.

Introduce new query param name include-accounting-tag with default = false, required = false in order for the code adaption to not have impact on existing workflow.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fcb1e676-1b01-4b36-b43b-b6e5871dcc88" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@QueryParam("include-accounting-tags") boolean isIncludingAccountingTags 
```

</div>

</div>

## Number of APIs we need to adapt.

Invoice → luzfin_finance/api/{{tenant_id}}/companies/{{company_Id}}/invoices

Offer → luzfin_finance/api/{{tenant_id}}/companies/{{company_Id}}/offers

Confirmation → luzfin_finance/api/{{tenant_id}}/companies/{{company_Id}}/confirmations

Delivery notes → luzfin_finance/api/{tenant_id}/companies/{company_Id}/delivery-notes

## Use existing service to get Sellable Articles.

There already a service implemented to get Sellable Aricle `ConvertTemplateToInvoiceService`, inside we can use method

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5d6d6523-8f3d-482f-9b74-d99ef375d733" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Map<String, SellableArticle> findMapSellableArticle(List<Invoice> invoices, Long companyId, boolean exportVat)
```

</div>

</div>

However, we still need to change `List<Invoice> invoices` to `OrderDetail` which is the super class of Invoice, I have investigated and saw that the param is only used to get Order Items list, so this should not cause any impact at all to the existing services.

This service is currently being used in `RecurringInvoiceRunService` refer method `smartDeliveryInvoices()`.

## How do we use it ?

First we need to separate Order details into 2 list of export vat & no export vat.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8f23afa4-cea8-4451-9532-cea028a8f591" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
List<OrderDetail> exportVatOrderDetails = new ArrayList<>();
List<OrderDetail> noExportVatOrderDetails = new ArrayList<>();
for (OrderDetail orderDetail: orderDetails) {
    if (invoice.getUsingExportVat()) {
        exportVatOrderDetails.add(orderDetail);
    } else {
        noExportVatOrderDetails.add(orderDetail);
    }
}
```

</div>

</div>

Then we need to get Sellable Articles for export vat & no export vat Order Details accordingly.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7f851714-df85-4988-b543-7cd4afca36eb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Map<String, SellableArticle> mapExportVatSellableArticle = convertTemplateToInvoiceService
        .findMapSellableArticle(exportVatOrderDetails, companyId, true);
Map<String, SellableArticle> mapNoExportVatSellableArticle = convertTemplateToInvoiceService
        .findMapSellableArticle(noExportVatOrderDetails, companyId, false);
```

</div>

</div>

After getting Sellable Article, all left is set accounting tags to Order Details.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d77d67ab-fcca-4956-ba8e-44296ad77c9a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
for (OrderDetail orderDetail : orderDetails) {
    for(OrderItem orderItem : orderDetail.getOrderItems()) {
        String itemNumber = orderItem.getItemNumer();
        SellableArticle article = orderDetail.getUsingExportVat() ? mapExportVatSellableArticle.get(itemNumber) : mapNoExportVatSellableArticle.get(itemNumber);

        if (article != null) {
            orderItem.setTag(ConvertTemplateToInvoiceService.buildOrderItemTag(article));
        }
    }
}
```

</div>

</div>

`ConvertTemplateToInvoiceService` is a service to convert Sellable Articles into accouning tag with format “tag1,tag2,tag3,etc”, eg: “fruit,apple”.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8d8d0da8-cf9a-4c67-a779-927ca22a02ea" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public static String buildOrderItemTag(SellableArticle sellableArticle) {
   return StringUtils.join(sellableArticle.getAccountingTags(), ",");
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Handle report configuration for new fields (12.06.2023)]]
- [[Research Design architecture concept for the service to generate the Generic Interface File]]
- [[Merging process]]
- [[List out places calling booking function]]
- [[Change get API when paying invoices]]

%% ai-graph-end %%