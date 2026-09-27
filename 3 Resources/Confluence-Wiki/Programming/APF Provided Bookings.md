---
ai_hash: ed47d6ab811324c2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 16
depth: 2.97
entities: []
relevance: 0.708
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/47101706774/APF+Provided+Bookings
space: X4
status: reference
tags:
- confluence
- programming
- space/x4
title: APF Provided Bookings
topic: programming
type: source
updated: 2022-08-31
---

# APF Provided Bookings

> [!info] Imported from Confluence
> Space **X4** · updated 2022-08-31 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47101706774/APF+Provided+Bookings)
> Relevance 0.708 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2" macro-id="aaaaaec53a540a5ee0f2d655d0ac35a5" macro-name="toc">

</div>

This document is about to describe detail how to use provided bookings.

## REST APIs


![[47101706774-image-20220719-021821.png]]



#### Create Provided Booking by POST `http://localhost:8080/eapf_rest/api/provided-bookings`

`workflowId`: Optional

`companyId`, `supplierId`: required.

`assigned `of ItemLine : Optional field, please noted this is a technical field and not necessary for CREATE/UPDATE.

<div id="expander-1296164056" class="expand-container conf-macro output-block" hasbody="true" macro-id="c10717e0-713e-4d60-bc2a-cf5283e7efd8" macro-name="expand">

<div id="expander-control-1296164056" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Sample JSON</span>

</div>

<div id="expander-content-1296164056" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="99c41563-fc66-41c4-a622-e8f9ca6b2965" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "providedBookingIdentifier": "00001",
  "name": "test",
  "sourceSystemName": "test",
  "itemDate": "2022-05-04T03:26:20.856Z",
  "isAmountInNumeric": false,
  "itemLines": [
    {
      "costCenterCode": "1000",
      "costTypeCode": "2000",
      "projectCode": "P2",
      "orderCode": "O1",
      "vatCode": "0036",
      "vatRate": 2.5,
      "grossAmount": 1000,
      "grossAmountPercent": 0,
      "netAmount": 975.60,
      "vatAmount":24.40,
      "postingText": "test",
      "individualFields": [
        {
          "fieldName": "Project",
          "fieldValue": "P01"
        },
        {
          "fieldName": "QrType",
          "fieldValue": "Qr01"
        }
      ]
    }
  ],
  "workflowId": 10039,
  "companyId": 2,
  "supplierId": 11120,
  "version": 0,
  "metaData": {
    "createdDate": "2022-05-04T03:26:20.856Z",
    "updatedDate": "2022-05-04T03:26:20.856Z",
    "createdUser": "system",
    "updatedUser": "system"
  }
}
```

</div>

</div>

</div>

</div>

#### Bulk Create Provided Bookings by POST `http://localhost:8080/eapf_rest/api/provided-bookings/bulk-creation`

workflowId: Optional  
companyId and supplierId: required.

<div id="expander-1114585060" class="expand-container conf-macro output-block" hasbody="true" macro-id="fca2d92d-a566-461e-a636-62e18ff38256" macro-name="expand">

<div id="expander-control-1114585060" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Sample JSON</span>

</div>

<div id="expander-content-1114585060" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c2ab5083-92bd-416e-946b-aa1d8d9f369c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {
    "providedBookingIdentifier": "00002",
    "name": "test",
    "sourceSystemName": "test",
    "itemDate": "2022-05-04T03:26:20.856Z",
    "isAmountInNumeric": false,
    "itemLines": [
      {
        "costCenterCode": "1000",
        "costTypeCode": "2000",
        "projectCode": "P2",
        "orderCode": "O1",
        "vatCode": "0036",
        "vatRate": 2.5,
        "grossAmount": 1000,
        "grossAmountPercent": 0,
        "netAmount": 975.60,
        "vatAmount":24.40,
        "postingText": "test",
        "individualFields": [
          {
            "fieldName": "Project",
            "fieldValue": "P01"
          },
          {
            "fieldName": "QrType",
            "fieldValue": "Qr01"
          }
        ]
      }
    ],
    "workflowId": 10039,
    "companyId": 2,
    "supplierId": 11120,
    "version": 0,
    "metaData": {
      "createdDate": "2022-05-04T03:26:20.856Z",
      "updatedDate": "2022-05-04T03:26:20.856Z",
      "createdUser": "system",
      "updatedUser": "system"
    }
  }

]
```

</div>

</div>

</div>

</div>

#### Update Provided Booking by PUT `http://localhost:8080/eapf_rest/api/provided-bookings`

The “version” value on JSON request **must be the same as** version on database, otherwise it wouldn’t allow to update data.

<div id="expander-513930921" class="expand-container conf-macro output-block" hasbody="true" macro-id="34091e07-ad6e-4879-956e-5b4a5cffa3a9" macro-name="expand">

<div id="expander-control-513930921" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Sample JSON</span>

</div>

<div id="expander-content-513930921" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f3c902d-c536-4b4c-a8ae-bfdba126d7e7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "id": 16,
    "providedBookingIdentifier": "00002",
    "name": "test updated",
    "sourceSystemName": "updated",
    "itemDate": "2022-05-04T03:26:20.856+0000",
    "isAmountInNumeric": false,
    "itemLines": [
      {
        "id": 10,
        "costCenterCode": "1000",
        "costTypeCode": "2000",
        "projectCode": "P2",
        "orderCode": "O1",
        "vatCode": "0036",
        "vatRate": 2.5,
        "grossAmount": 1000,
        "grossAmountPercent": 0,
        "netAmount": 975.6,
        "vatAmount": 24.4,
        "postingText": "test",
        "individualFields": [
          {
            "id": 13,
            "fieldName": "Project",
            "fieldValue": "P01"
          }
        ]
      }
    ],
    "workflowId": 10039,
    "companyId": 2,
    "supplierId": 11120,
    "version": 2,
    "metaData": {
      "createdDate": "2022-05-04T03:44:45.635+0000",
      "updatedDate": "2022-05-04T03:26:20.856+0000",
      "createdUser": "pn",
      "updatedUser": "pn"
    }
  }
```

</div>

</div>

</div>

</div>

#### Create Booking Assignments by POST `http://localhost:8080/eapf_rest/api/booking-assignments`

<div id="expander-509258081" class="expand-container conf-macro output-block" hasbody="true" macro-id="1f323a86-fbd2-41c3-b18a-bd3ed432d626" macro-name="expand">

<div id="expander-control-509258081" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Sample JSON</span>

</div>

<div id="expander-content-509258081" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aa11afe6-21c2-43bd-b96e-cd1e0d9965e8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {
    "providedBookingLineId": 10,
    "itemLineId": 41617,
    "itemHeadId": 59572,
    "metaData": {
      "createdDate": "2022-05-04T03:26:20.849Z",
      "updatedDate": "2022-05-04T03:26:20.849Z",
      "createdUser": "system",
      "updatedUser": "system"
    }
  }
]
```

</div>

</div>

</div>

</div>

#### Filter Provided Booking Identifiers by Parameters `http://localhost:8080/eapf_rest/api/provided-bookings/filterBookingIdentifiers`


![[47101706774-image-20220719-030041.png]]



#### Filter Booking Assignments by GET `http://localhost:8080/eapf_rest/api/provided-bookings/getBookings` 


![[47101706774-image-20220719-030532.png]]



## Database structure 


![[47101706774-image-20220831-032349.png]]



Column details:  


![[47101706774-7e60c3a9-f04d-4d8d-88db-dfb3d05a0026#media-blob-url=true&id=d6dbc024-93c1-4a70-b.png]]



## Setup Guide 

1.Execute this SQL script to add constraint for table ProvidedBookings and ProvidedBookingAssignments

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="526938e2-28f2-485f-9791-5be752ceb04e" macro-name="view-file"><a href="../_attachments/47101706774-V21__alter_table_providedbooking_add_constraint.sql" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/47101706774/V21__alter_table_providedbooking_add_constraint.sql?version=1&amp;modificationDate=1654142558645&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[47101706774-V21__alter_table_providedbooking_add_constraint.sql]]

</a></span>

2\. Execute SQL script to create Index for improve performance for filter provided booking  

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="0357f62d-12f2-4853-96f8-2e7ed767b6d5" macro-name="view-file"><a href="../_attachments/47101706774-V22_create_index_for_provided_bookings.sql" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/47101706774/V22_create_index_for_provided_bookings.sql?version=1&amp;modificationDate=1658200095212&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[47101706774-V22_create_index_for_provided_bookings.sql]]

</a></span>

## Use Provided Bookings in ItemHead detail

On ItemHead detail screen, click on button Import ItemLine dialog then navigate to tab *Purchase orders / Goods received.*

### Filter section

- By default, user is able to see Purchase orders belongs to company, supplier and workflow of working ItemHead.

- *Checkbox Workflow*: lookup Purchase orders belongs to a ItemHead’s workflow or all workflows.

- *Item date from / to* : range filter for Itemdate of Purchase orders.

- *Item no:* Auto complete with multiple choices . By default Purchase order from ItemHead will be applied. This selected values will be clear/empty if there is no Purchase order match with the filter conditions.

### Choose line to import

- Show all order lines matched with the filter conditions.

- Allow to select one/ many/ all to be import.

- If user choose order lines in intentionally ordering, for example row 3, row 2 and last is row 5, then ItemLines will be imported to Accounting table with the same ordering.

The order lines which are imported will be collected and use for Filter customization.  
  
**Attribute** **isAmountInNumeric**:  
+ When isAmountInNumeric for ProvidedBookings is True (1), we import amount’s value as it it.  
+ When isAmountInNumeric for ProvidedBookings is false (0), Gross Amount, Net Amount and Vat Amount value will be calculated based o ItemHead’s Total Gross Amount.

example:


![[47101706774-image-20220607-095939.png]]




![[47101706774-image-20220607-092615.png]]



  
+ ItemHead Total gross amount = 531.50.  
+ Gross Amount Percent for Provided ItemLine = 50.0000000 (aka 50 %).  
Then Gross Amount = 531.50 \* 50/100 = 265.75  
Vat amount and Net amount will be calculated following current formula:  
Vat Amount = 265.75 \* 7.6 (vat rate) / (100 + 7.6) = 18.75  
Net Amount = 265.75 - 18.75 = 247.00.  

## Technical notes

- Performance notes for query:

  - Query 1: find Booking Identifiers  
    REST API: <a href="http://localhost:8080/eapf_rest/api/provided-bookings/filterBookingIdentifiers?workflowId=10039&amp;companyId=2&amp;supplierId=11121&amp;showAssigned=false&amp;startDate=20220701&amp;endDate=20220731" class="external-link" rel="nofollow"><span class="legacy-color-text-inverse">http://localhost:8080/eapf_rest/api/provided-bookings/filterBookingIdentifiers?workflowId=10039&amp;companyId=2&amp;supplierId=11121&amp;showAssigned=false&amp;startDate=20220701&amp;endDate=20220731</span></a>  
    Generated query:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bf4f7726-2a6e-4926-b9d0-7bbe6a056402" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    select distinct providedbo0_.providedBookingIdentifier as col_0_0_ 
    from ProvidedBookings providedbo0_ inner join ProvidedItemLine itemlines1_ on providedbo0_.id=itemlines1_.providedBookingId left outer join ProvidedBookingAssignments bookingass2_ on itemlines1_.id=bookingass2_.providedBookingLineId 
    where providedbo0_.workflowId=10039 and providedbo0_.companyId=2 and providedbo0_.supplierId=11121 and (providedbo0_.itemDate between ? and ?) and (bookingass2_.id is null)
    order by providedbo0_.providedBookingIdentifier asc
    ```

    </div>

    </div>

  - Query 2: get purchase order lines by selected Booking Identifiers  
    REST API: <a href="http://localhost:8080/eapf_rest/api/provided-bookings/getBookings?showAssigned=true&amp;bookingIdentifiers=BI30001&amp;bookingIdentifiers=BI10472&amp;bookingIdentifiers=BI29813&amp;bookingIdentifiers=BI29934" class="external-link" rel="nofollow"><span class="legacy-color-text-inverse">http://localhost:8080/eapf_rest/api/provided-bookings/getBookings?showAssigned=true&amp;bookingIdentifiers=BI30001&amp;bookingIdentifiers=BI10472</span></a>  
    Generated query:  

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="adbb24b3-528d-4120-92f9-a81ba14af68f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    select distinct providedit0_.id as col_0_0_, bookingass2_.id as col_1_0_, individual3_.id as id1_59_3_, providedbo4_.id as id1_58_4_, providedit0_.id as id1_60_0_, individual3_.id as id1_59_1_, providedbo4_.id as id1_58_2_, providedit0_.costCenterCode as costCent2_60_0_, providedit0_.costTypeCode as costType3_60_0_, providedit0_.grossAmount as grossAmo4_60_0_, providedit0_.grossAmountPercent as grossAmo5_60_0_, providedit0_.netAmount as netAmoun6_60_0_, providedit0_.orderCode as orderCod7_60_0_, providedit0_.postingText as postingT8_60_0_, providedit0_.projectCode as projectC9_60_0_, providedit0_.providedBookingId as provide13_60_0_, providedit0_.vatAmount as vatAmou10_60_0_, providedit0_.vatCode as vatCode11_60_0_, providedit0_.vatRate as vatRate12_60_0_, individual3_.fieldName as fieldNam2_59_1_, individual3_.fieldValue as fieldVal3_59_1_, individual3_.providedItemLineId as provided4_59_0__, individual3_.id as id1_59_0__, providedbo4_.companyId as companyI2_58_2_, providedbo4_.isAmountInNumeric as isAmount3_58_2_, providedbo4_.itemDate as itemDate4_58_2_, providedbo4_.createdDate as createdD5_58_2_, providedbo4_.createdUser as createdU6_58_2_, providedbo4_.modifiedDate as modified7_58_2_, providedbo4_.updatedUser as updatedU8_58_2_, providedbo4_.providedBookingIdentifier as provided9_58_2_, providedbo4_.sourceSystemName as sourceS10_58_2_, providedbo4_.supplierId as supplie11_58_2_, providedbo4_.version as version12_58_2_, providedbo4_.workflowId as workflo13_58_2_ 
    from ProvidedItemLine providedit0_ left outer join ProvidedBookings providedbo1_ on providedit0_.providedBookingId=providedbo1_.id left outer join ProvidedBookingAssignments bookingass2_ on providedit0_.id=bookingass2_.providedBookingLineId left outer join ProvidedIndividualField individual3_ on providedit0_.id=individual3_.providedItemLineId left outer join ProvidedBookings providedbo4_ on providedit0_.providedBookingId=providedbo4_.id 
    where providedbo1_.providedBookingIdentifier in (? , ? , ? , ?)
    ```

    </div>

    </div>

  - SQL Index to improve searching performance  

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="0c8721f2-b681-4819-a78d-d3e07056f078" macro-name="view-file"><a href="../_attachments/47101706774-V22_create_index_for_provided_bookings.sql" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/47101706774/V22_create_index_for_provided_bookings.sql?version=1&amp;modificationDate=1658200095212&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[47101706774-V22_create_index_for_provided_bookings.sql]]

</a></span>

- Dummy data generate:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="8e82af32-766c-421e-9ad6-0b7cd0855fc5" macro-name="view-file"><a href="../_attachments/47101706774-AD-4575-insert-dummy-data.sql" class="confluence-embedded-file" data-nice-type="Text File" data-file-src="/wiki/download/attachments/47101706774/AD-4575-insert-dummy-data.sql?version=2&amp;modificationDate=1658202232900&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/plain" data-has-thumbnail="true">

![[47101706774-AD-4575-insert-dummy-data.sql]]

</a></span>

- To check performance when execute SQL script, append this script to the beginning of the query  

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e00eea82-9814-41c5-838f-a020e2ad825b" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  SET STATISTICS TIME ON
  SET STATISTICS IO ON
  ```

  </div>

  </div>

    

- Related stories:

<a href="https://axonivy.atlassian.net/browse/AD-4574" class="external-link" rel="nofollow">AD-4574 - Part 1</a>

<a href="https://axonivy.atlassian.net/browse/AD-4338" class="external-link" rel="nofollow">AD-4338 - Part 2</a>

<a href="https://axonivy.atlassian.net/browse/AD-4575" class="external-link" rel="nofollow">AD-4575 - Part 3</a>

<a href="https://axonivy.atlassian.net/browse/AD-4919" class="external-link" rel="nofollow">AD-4919: add description to ProvidedBooking</a>

- Pull request

<a href="https://bitbucket.org/soreco_prod/admetos/pull-requests/925" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/soreco_prod/admetos/pull-requests/925</a>

<a href="https://bitbucket.org/soreco_prod/admetos/pull-requests/944" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/soreco_prod/admetos/pull-requests/944</a>

%% ai-graph-start %%

**Related notes:**
- [[APF ItemLine Import API]]
- [[Generic Interface JSON file]]
- [[APF swagger for project eapf_web]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[Preview Delivery-Prices API - ForcedOnboading]]

%% ai-graph-end %%