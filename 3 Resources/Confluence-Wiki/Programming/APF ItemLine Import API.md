---
title: "APF ItemLine Import API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/34422799702/APF+ItemLine+Import+API
space: "X4"
topic: programming
relevance: 0.703
depth: 2.38
updated: 2020-10-01
attachments: 7
tags:
  - confluence
  - programming
  - space/x4
---

# APF ItemLine Import API

> [!info] Imported from Confluence
> Space **X4** · updated 2020-10-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/34422799702/APF+ItemLine+Import+API)
> Relevance 0.703 · topic `programming`

## 

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="9977b147-2d2d-4b0d-acae-5a98814fbb96" macro-name="toc">

</div>

## **Overview**

The API support we can import ItemLines for existed ItemHead. We can see on on the Swagger UI:


![[34422799702-image2020-9-22_10-8-44.png]]



## **Guideline**

- ##### **ItemHead's UUID is required**

 We support not only manual import on Swagger UI we also support third party import ItemLine immediately after imported ItemHead. So for this case may third party can not know ItemHeadId because  ItemHeadID is auto generated when ItemHead gets created. Therefore the third party should generate ItemHead UUID.

- **CostTypeCode**

 Code of CostType.

- ##### **CostCenterCode**

 Code of CostCenter.

- ##### **VatCode**

 Code of Vat.

- ##### **uuid**

 UUID of itemLine. This value must unique. Can be input or let it empty, system will generate if it null.

- **FieldName **

Field name of individualField, can be check in IndividualFieldConfig.code. Each individualFieldData's fieldName can not duplicate.

- ##### **FieldValue**

Field value of field name above, with individual datatype support multi values like: autoSelect, selectOneMenu, we refer input code.

- ##### **valueId can be NULL** 

If we input field value above we let this field NULL.

With individual datatype support multi values, we refer input code. But some case user can input Id of record data, we can input here.  
  

## **API**

- ##### **Import ItemLines**

Using method POST to import one or many ItemLine to an ItemHead.

- itemHeadUUID: ItemHead's UUID.
- itemLine: ItemLine infomation.
- individualFieldDatas: individual field data of itemLine.  Each individualFieldData's fieldName can not duplicate.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9f1476e2-06fc-46ae-8c33-f24f6e119849" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">

**Method POST**<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
[
  {
    "itemHeadUUID": "4e5cbc87-c71c-4cba-8f38-334857648f1c",
    "costCenterCode": "101203",
    "costTypeCode": "14001",
    "vatCode": "380",
    "vatRate": 3.8,
    "grossAmount": 2000,
    "netAmount": 1926.78,
    "vatAmount": 73.22,
    "postingText": "",
    "uuid": "56a5c819-82fd-452a-a860-0qqhb00c2116",
    "individualFieldDatas": [
      {
        "fieldName": "ProjectNoColumn",
        "fieldValue": "ADM",
        "valueId": null
      },
      {
        "fieldName": "Indv_ITH_003",
        "fieldValue": null,
        "valueId": 102
      }
    ]
  }
]
```

</div>

</div>

  

- ##### **Update ItemLines**

Using PUT method to update one or many ItemLine.

  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4c110d8b-9b9f-4d44-887a-e02a4825f486" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">

**Method PUT**<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
[
  {
    "id": 0,
    "version": 0,
    "metaData": {
      "createdDate": "2020-09-22T02:21:08.612Z",
      "updatedDate": "2020-09-22T02:21:08.612Z",
      "createdUser": "string",
      "updatedUser": "string"
    },
    "validMetaData": {
      "validFrom": "2020-09-22T02:21:08.612Z",
      "validTo": "2020-09-22T02:21:08.612Z"
    },
    "costCenter": {
      "id": 0,
      "version": 0,
      "code": "string",
      "companyId": 0,
      "name": "string",
      "metaData": {
        "createdDate": "2020-09-22T02:21:08.612Z",
        "updatedDate": "2020-09-22T02:21:08.612Z",
        "createdUser": "string",
        "updatedUser": "string"
      },
      "validMetaData": {
        "validFrom": "2020-09-22T02:21:08.612Z",
        "validTo": "2020-09-22T02:21:08.612Z"
      }
    },
    "costType": {
      "id": 0,
      "version": 0,
      "code": "string",
      "companyId": 0,
      "name": "string",
      "metaData": {
        "createdDate": "2020-09-22T02:21:08.612Z",
        "updatedDate": "2020-09-22T02:21:08.612Z",
        "createdUser": "string",
        "updatedUser": "string"
      },
      "validMetaData": {
        "validFrom": "2020-09-22T02:21:08.612Z",
        "validTo": "2020-09-22T02:21:08.612Z"
      }
    },
    "vatRate": 0,
    "vat": {
      "id": 0,
      "version": 0,
      "metaData": {
        "createdDate": "2020-09-22T02:21:08.612Z",
        "updatedDate": "2020-09-22T02:21:08.612Z",
        "createdUser": "string",
        "updatedUser": "string"
      },
      "validMetaData": {
        "validFrom": "2020-09-22T02:21:08.612Z",
        "validTo": "2020-09-22T02:21:08.612Z"
      },
      "vatCode": "string",
      "shortName": "string",
      "description": "string",
      "country": "string",
      "vatRate": 0,
      "vatInclude": false
    },
    "grossAmount": 0,
    "netAmount": 0,
    "vatAmount": 0,
    "postingText": "string",
    "requestState": "string",
    "uuid": "string",
    "itemHeadId": 0,
    "newImport": false
  }
]
```

</div>

</div>

  

- ##### **Delete ItemLines**

Using method post to send one o many ItemLine to delete.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a0011f6d-ece0-4c35-a194-46c371c3f1a5" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">

**Method POST**<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
[
  {
    "id": 0,
    "version": 0,
    "metaData": {
      "createdDate": "2020-09-22T02:21:08.616Z",
      "updatedDate": "2020-09-22T02:21:08.616Z",
      "createdUser": "string",
      "updatedUser": "string"
    },
    "validMetaData": {
      "validFrom": "2020-09-22T02:21:08.616Z",
      "validTo": "2020-09-22T02:21:08.616Z"
    },
    "costCenter": {
      "id": 0,
      "version": 0,
      "code": "string",
      "companyId": 0,
      "name": "string",
      "metaData": {
        "createdDate": "2020-09-22T02:21:08.616Z",
        "updatedDate": "2020-09-22T02:21:08.616Z",
        "createdUser": "string",
        "updatedUser": "string"
      },
      "validMetaData": {
        "validFrom": "2020-09-22T02:21:08.616Z",
        "validTo": "2020-09-22T02:21:08.616Z"
      }
    },
    "costType": {
      "id": 0,
      "version": 0,
      "code": "string",
      "companyId": 0,
      "name": "string",
      "metaData": {
        "createdDate": "2020-09-22T02:21:08.616Z",
        "updatedDate": "2020-09-22T02:21:08.616Z",
        "createdUser": "string",
        "updatedUser": "string"
      },
      "validMetaData": {
        "validFrom": "2020-09-22T02:21:08.616Z",
        "validTo": "2020-09-22T02:21:08.616Z"
      }
    },
    "vatRate": 0,
    "vat": {
      "id": 0,
      "version": 0,
      "metaData": {
        "createdDate": "2020-09-22T02:21:08.616Z",
        "updatedDate": "2020-09-22T02:21:08.616Z",
        "createdUser": "string",
        "updatedUser": "string"
      },
      "validMetaData": {
        "validFrom": "2020-09-22T02:21:08.616Z",
        "validTo": "2020-09-22T02:21:08.616Z"
      },
      "vatCode": "string",
      "shortName": "string",
      "description": "string",
      "country": "string",
      "vatRate": 0,
      "vatInclude": false
    },
    "grossAmount": 0,
    "netAmount": 0,
    "vatAmount": 0,
    "postingText": "string",
    "requestState": "string",
    "uuid": "string",
    "itemHeadId": 0,
    "newImport": false
  }
]
```

</div>

</div>

  

- ##### **Get ItemLines By ItemHead ID**

Using method GET to get all ItemLines of an ItemHead by ItemHeadID.


![[34422799702-image2020-9-22_11-26-47.png]]



- ##### **Get ItemLines By ItemHead ID with NewImport status**

Using method GET to get all ItemLines of an ItemHead by ItemHeadID but we can filter is new import or not.


![[34422799702-image2020-9-22_11-30-27.png]]



- ##### **Error When Import to Invalid ItemHead**

When import an UUID of ItemHead not Inprogress State or not existed we will get the message.  

![[34422799702-image2020-9-22_11-35-32.png]]



## **Where on database is this data stored:**

ItemLine table and IndividualFiedData table.

## **Technical notes**

ItemLineResource, ItemLineBean, ItemLineImportDTO.
