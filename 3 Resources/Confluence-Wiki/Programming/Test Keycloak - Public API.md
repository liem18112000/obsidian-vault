---
ai_hash: 0707fe0498dfbcdf
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47603351609/Test+Keycloak+-+Public+API
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Test Keycloak - Public API
topic: programming
type: source
updated: 2024-01-03
---

# Test Keycloak - Public API

> [!info] Imported from Confluence
> Space **TS** · updated 2024-01-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47603351609/Test+Keycloak+-+Public+API)
> Relevance 0.786 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Test case</strong></p></th>
<th><p><strong>Expected</strong></p></th>
<th><p><strong>Staging</strong></p></th>
<th><p><strong>Notes</strong></p></th>
</tr>
&#10;<tr>
<td><p>Create company</p></td>
<td><p>

![[47603351609-check.png]]

</p>
<div id="expander-1524690566" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f5dcf4ba-5e19-4f4e-8107-6852f466f61f" data-macro-name="expand">
<div id="expander-control-1524690566" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Request</span>
</div>
<div id="expander-content-1524690566" class="expand-content expand-hidden">
<p>{<br />
  "name": "zzz public api {{company_name_number}}",<br />
  "legalForm": "INDIVIDUALLY_OWNED_COMPANY",<br />
  "phones": [<br />
    {<br />
      "phoneNumber": "+41 78 123 23 43",<br />
      "type": "OFFICE"<br />
    }<br />
  ],<br />
  "emails": [<br />
    {<br />
      "emailAddress": "<a href="mailto:miracle_public_api_1@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_1@axongroupio.ch</a>",<br />
      "type": "OFFICE"<br />
    }<br />
  ],<br />
  "addresses": [<br />
    {<br />
      "addressLines": "Chemin de la Caquerette 12",<br />
      "addressType": "PRIVATE",<br />
      "cityName": "Bern",<br />
      "cityZipCode": "3003",<br />
      "countryIso2Code": "CH",<br />
      "countryIso3Code": "CHE",<br />
      "countryNumericCode": "756",<br />
      "additionalAddress": "No. 13, street 123"<br />
    }<br />
  ],<br />
  "language": "de",<br />
  "foundingDate": "2019-12-20"<br />
}</p>
</div>
</div>
<div id="expander-421585224" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="18f7e9e1-7d9f-449a-b2db-8743f835b7a2" data-macro-name="expand">
<div id="expander-control-421585224" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Response</span>
</div>
<div id="expander-content-421585224" class="expand-content expand-hidden">
<p>{<br />
"tenant_id": "f3db0406-4c0d-4147-b83b-21fc26e3f552",<br />
"company_id": 1,<br />
"company_name": "zzz public api 52"<br />
}</p>
</div>
</div>
<div id="expander-235648004" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="fe078856-d622-4bd4-882a-b670ab2707b1" data-macro-name="expand">
<div id="expander-control-235648004" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Company selection</span>
</div>
<div id="expander-content-235648004" class="expand-content expand-hidden">

![[47603351609-image-20231226-092455.png]]


</div>
</div>
<div id="expander-373272450" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="831f8492-4bbd-414e-aff3-a8033a808a36" data-macro-name="expand">
<div id="expander-control-373272450" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Subscribe widget</span>
</div>
<div id="expander-content-373272450" class="expand-content expand-hidden">

![[47603351609-image-20231226-092636.png]]


</div>
</div></td>
<td><p>

![[47603351609-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>Update company</p></td>
<td><p>

![[47603351609-check.png]]

</p>
<div id="expander-1326039431" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1d109d04-e1aa-435b-93fd-c191e9d1049f" data-macro-name="expand">
<div id="expander-control-1326039431" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Request</span>
</div>
<div id="expander-content-1326039431" class="expand-content expand-hidden">
<p>{<br />
"name": "zzz public api 52 updated",<br />
"legalForm": "INDIVIDUALLY_OWNED_COMPANY",<br />
"phones": [<br />
{<br />
"id": "1",<br />
"phoneNumber": "+41 78 123 25 43",<br />
"type": "OFFICE"<br />
}<br />
],<br />
"emails": [<br />
{<br />
"id": "1",<br />
"emailAddress": "<a href="mailto:miracle_public_api_1@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_1@axongroupio.ch</a>",<br />
"type": "OFFICE"<br />
}<br />
],<br />
"addresses": [<br />
{<br />
"id": "1",<br />
"validFrom": "2023-12-26",<br />
"validTo": null,<br />
"addressLines": "Chemin de la Caquerette 12",<br />
"addressType": "PRIVATE",<br />
"cityName": "Bern",<br />
"cityZipCode": "3003",<br />
"countryIso2Code": "CH",<br />
"countryIso3Code": "CHE",<br />
"countryNumericCode": "756",<br />
"definitionName": null,<br />
"additionalAddress": "No. 13, street 123",<br />
"city_href": null<br />
}<br />
],<br />
"language": "de",<br />
"foundingDate": "2019-12-22"<br />
}</p>
</div>
</div>
<div id="expander-35866482" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="606546a8-5b70-4b63-92fd-b36989dcdb4c" data-macro-name="expand">
<div id="expander-control-35866482" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Response</span>
</div>
<div id="expander-content-35866482" class="expand-content expand-hidden">
<p>{<br />
"name": "zzz public api 52 updated",<br />
"legalForm": "INDIVIDUALLY_OWNED_COMPANY",<br />
"phones": [<br />
{<br />
"id": "1",<br />
"phoneNumber": "+41 78 123 25 43",<br />
"type": "OFFICE"<br />
}<br />
],<br />
"emails": [<br />
{<br />
"id": "1",<br />
"emailAddress": "<a href="mailto:miracle_public_api_1@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_1@axongroupio.ch</a>",<br />
"type": "OFFICE"<br />
}<br />
],<br />
"addresses": [<br />
{<br />
"id": "2",<br />
"validFrom": "2023-12-26",<br />
"validTo": null,<br />
"addressLines": "Chemin de la Caquerette 12",<br />
"addressType": "PRIVATE",<br />
"cityName": "Bern",<br />
"cityZipCode": "3003",<br />
"countryIso2Code": "CH",<br />
"countryIso3Code": "CHE",<br />
"countryNumericCode": "756",<br />
"definitionName": null,<br />
"additionalAddress": "No. 13, street 123",<br />
"city_href": null<br />
}<br />
],<br />
"language": "de",<br />
"foundingDate": "2019-12-22"<br />
}</p>
</div>
</div></td>
<td><p>

![[47603351609-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>Get company</p></td>
<td><p>

![[47603351609-check.png]]

</p>
<div id="expander-1725757844" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="02725ac7-2585-427f-9953-1218c0369dfa" data-macro-name="expand">
<div id="expander-control-1725757844" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Response</span>
</div>
<div id="expander-content-1725757844" class="expand-content expand-hidden">
<p>{<br />
"name": "zzz public api 52",<br />
"legalForm": "INDIVIDUALLY_OWNED_COMPANY",<br />
"phones": [<br />
{<br />
"id": "1",<br />
"phoneNumber": "+41 78 123 23 43",<br />
"type": "OFFICE"<br />
}<br />
],<br />
"emails": [<br />
{<br />
"id": "1",<br />
"emailAddress": "<a href="mailto:miracle_public_api_1@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_1@axongroupio.ch</a>",<br />
"type": "OFFICE"<br />
}<br />
],<br />
"addresses": [<br />
{<br />
"id": "1",<br />
"validFrom": "2023-12-26",<br />
"validTo": null,<br />
"addressLines": "Chemin de la Caquerette 12",<br />
"addressType": "PRIVATE",<br />
"cityName": "Bern",<br />
"cityZipCode": "3003",<br />
"countryIso2Code": "CH",<br />
"countryIso3Code": "CHE",<br />
"countryNumericCode": "756",<br />
"definitionName": null,<br />
"additionalAddress": "No. 13, street 123",<br />
"city_href": null<br />
}<br />
],<br />
"language": "de",<br />
"foundingDate": "2019-12-20"<br />
}</p>
</div>
</div></td>
<td><p>

![[47603351609-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>Create Subscription</p></td>
<td><p>

![[47603351609-check.png]]

</p>
<p>Code: K-08-8040-00-M</p>
<div id="expander-43870851" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1c439def-459b-446d-a27e-cc221e2cf161" data-macro-name="expand">
<div id="expander-control-43870851" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Response</span>
</div>
<div id="expander-content-43870851" class="expand-content expand-hidden">
<p>[<br />
{<br />
"product": {<br />
"code": "EBILL_ACTIVATION_CODE",<br />
"name": "EBILL ACTIVATION CODE TEST"<br />
},<br />
"pricePlan": "MONTHLY",<br />
"subscriptionFrom": "2023-12-26T11:13:50",<br />
"renewalDate": "2024-03-01"<br />
}<br />
]</p>
</div>
</div>
<div id="expander-1316868327" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="e2c1e922-0ec9-4737-af57-8d363b9086f5" data-macro-name="expand">
<div id="expander-control-1316868327" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">GUI</span>
</div>
<div id="expander-content-1316868327" class="expand-content expand-hidden">

![[47603351609-image-20231226-101538.png]]


</div>
</div></td>
<td><p>

![[47603351609-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>Add tenant dunning</p></td>
<td><p>

![[47603351609-check.png]]

</p>

![[47603351609-image-20231226-094620.png]]

</td>
<td><p>

![[47603351609-check.png]]

</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[LUZ-110826 Public API - Widget subscription by activation code]]
- [[Test Keycloak - Company Identity Mapper (script mapper)]]
- [[How to use Public API to create update KLARA Business Company]]
- [[Getting tenant list]]

%% ai-graph-end %%