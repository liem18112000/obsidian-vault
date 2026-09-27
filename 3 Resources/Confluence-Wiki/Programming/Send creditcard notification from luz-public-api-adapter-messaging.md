---
title: "Send creditcard notification from luz-public-api-adapter-messaging"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38240846497/Send+creditcard+notification+from+luz-public-api-adapter-messaging
space: "Helios"
topic: programming
relevance: 0.786
depth: 3
updated: 2021-07-29
attachments: 2
tags:
  - confluence
  - programming
  - space/helios
---

# Send creditcard notification from luz-public-api-adapter-messaging

> [!info] Imported from Confluence
> Space **Helios** · updated 2021-07-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38240846497/Send+creditcard+notification+from+luz-public-api-adapter-messaging)
> Relevance 0.786 · topic `programming`

- ### Account test on myKlara

Server: DEV

User: <a href="mailto:hcmc-helios@axonactive.com" class="external-link" rel="nofollow">hcmc-helios@axonactive.com</a>

  

- ### Port forward to call the internal api

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3bf7261e-a60b-4efa-8d8b-a5180d0f8642" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
kubectl port-forward service/luz-public-api-adapter-messaging 8886:8080 -n dev
```

</div>

</div>

  

- ### Call api on Postman

##### Request url:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0454bf40-f98a-4a27-9d7a-4e4c5788f52c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
http://localhost:8886/messaging/v1/creditcard-notifications
```

</div>

</div>

##### Request body:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9ba118ca-af39-4871-88e2-95501f0f6a9a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "authorizationDate": "20210728",//yyyymmdd
  "authorizationTime": "230000",//hhmmss
  "amountInLocalCurrency": 1900,//divide it by 100 then format as currency

  "headerMessageId": "AUTH  2021-02-09-10.26.20.367718<XXXXX>0001",
  "messageVersionNumber": "1",
  "messageSequence": 1,
  "eventType": "AUTH",
  "eventDisposition": "ADD",
  "hdrTimeStamp": "2021-02-09-10.26.20.367718",
  "channel": "POS TERMINAL",
  "authorizationSequenceNr": 488804,
  "b2bCustomerId": "772eec61-43c0-4ecc-a218-b9263f0ad490",
  "instrumentId": "20383",
  "truncatedPan": "536676******1022",
  "merchantZIP": "90210",
  "merchantCountry": "CHE",
  "merchanteCategoryCode": "5999",
  "typeCode": "P",
  "resultCode": 0,
  "cardCurrencyCode": "756",
  "amountInCardCurrency": 225,
  "localCurrencyCode": "756",
  "conversionRate": 1,
  "merchantName": "DENNER BE\\THUN",
  "visaResponseCode": "000",
  "authorizationStatus": "N",
  "transactionCode": "1035",
  "transactionCategoryCode": "AU",
  "messageTypeCode": 1100,
  "purgeDate": "20210216",
  "feeAmount1": 0,
  "feeCode1": 0,
  "feeAmount2": 0,
  "feeCode2": 0,
  "feeAmount3": 0,
  "feeCode3": 0,
  "feeAmount4": 0,
  "feeCode4": 0,
  "feeAmount5": 0,
  "feeCode5": 0,
  "ledgerBalanceSign": "CR",
  "ledgerBalance": 20000,
  "availableBalanceSign": "CR",
  "transactionSign": "DR",
  "totalTransaction": 1000,
  "paymentTransactionServicesCategoryCode": "",
  "amountOther": 0,
  "subType": "ACCEPTED_TRANSACTIONS",
  "availableBalance": 19875,
  "accountNumber": "200030<XXXXX>7655"
}
```

</div>

</div>

<div>

<table style="width: 92.4948%;">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><div class="content-wrapper">

![[38240846497-postman1.PNG]]


</div></th>
<th><div class="content-wrapper">

![[38240846497-postman2.PNG]]


</div></th>
</tr>
&#10;</tbody>
</table>

</div>
