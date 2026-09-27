---
title: "Cembra API (the real one)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496030223/Cembra+API+the+real+one
space: "LUZ"
topic: programming
relevance: 0.792
depth: 3
updated: 2019-11-15
attachments: 7
tags:
  - confluence
  - programming
  - space/luz
---

# Cembra API (the real one)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2019-11-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20496030223/Cembra+API+the+real+one)
> Relevance 0.792 · topic `programming`

# API

Current Cembra interface for the upcoming integration (as of 11.11.2019) :

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="70ee603f-2dfa-4bea-b83d-b1a6875d4ad6" macro-name="view-file"><a href="../_attachments/20496030223-Klara.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/20496030223/Klara.postman_collection.json?version=1&amp;modificationDate=1573482794000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[20496030223-Klara.postman_collection.json]]

</a></span>

Integrate into Postman (click on the link) :

<a href="https://www.getpostman.com/collections/38360e675caae9846250" class="external-link" rel="nofollow">https://www.getpostman.com/collections/38360e675caae9846250</a>

  

Use the service "calculate" to get the credit amount and the interest rate.


![[20496030223-image2019-11-11_14-36-24.png]]



  

Request attributes

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
<th>Attribute</th>
<th>Sub Attribute</th>
<th>value</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td>country</td>
<td><br />
</td>
<td><br />
</td>
<td>"CHF"</td>
</tr>
<tr>
<td>productType</td>
<td><br />
</td>
<td><br />
</td>
<td>"light"</td>
</tr>
<tr>
<td>loanPurpose</td>
<td><br />
</td>
<td><div class="content-wrapper">
<p>Dropdown, example below :</p>

![[20496030223-image2019-11-12_9-31-16.png]]


<p><br />
</p>
<p>Grund der Kreditaufnahme (default selection) :</p>
<p>marketing_campaigns : Marketing und Werbung</p>
<p>managing_cashflows : Kurzfristige Betriebsmittel</p>
<p>unexpected_expense : Unerwartete Aufwendungen</p>
<p>hiring_staff : Personalbeschaffung</p>
<p>purchase_equipment : Anschaffunf übrige Betriebsausstattung</p>
<p>remodeling_renovation : Renovierung und Erneuerung</p>
<p>bridging_receivable : Übernahme-/Nachfolgefinanzierung</p>
<p>purchase_inventory : Wareneinkauf</p>
<p><br />
</p>
</div></td>
<td><br />
</td>
</tr>
<tr>
<td>yearsInBusiness</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>2_3_years</strong> (as of 15.11.2019 - initially yearsInBusiness should be empty, but Cembra is not ready yet)</td>
</tr>
<tr>
<td>requestedAmount</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>amount</td>
<td>requested amount in CHF, integer</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>currency</td>
<td><br />
</td>
<td>"CHF"</td>
</tr>
<tr>
<td>revenue</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>amount</td>
<td>revenue of the last 12 month excluding current month, amount in CHF, integer</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>currency</td>
<td><br />
</td>
<td>"CHF"</td>
</tr>
</tbody>
</table>

</div>

  

Response attributes

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
<th>Attribute</th>
<th>value</th>
<th>const</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td><p><span>loanDuration</span></p></td>
<td>do not show this value</td>
<td>display 12 month</td>
<td><br />
</td>
</tr>
<tr>
<td><p><span>PrequalificationAmountFrom</span></p></td>
<td>amount in CHF, integer</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><p><span>PrequalificationAmountTo</span></p></td>
<td>amount in CHF, integer</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><p><span>InterestRateFrom</span></p></td>
<td>amount as percent, decimal (#.0)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><p><span>InterestRateTo</span></p></td>
<td>amount as percent, decimal (#.0)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

  

Error Handling

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td>years in business &lt; 2 years</td>
<td><br />
</td>
<td>show business error message, request rejected</td>
</tr>
<tr>
<td>years in business &gt;= 2 years</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td>value returned</td>
<td>a data structure with response is returned</td>
</tr>
<tr>
<td><br />
</td>
<td>http code != 200</td>
<td>a technical error has occurred, show technical error message</td>
</tr>
</tbody>
</table>

</div>

# Security / Connectivity

## Endpoints

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Env</th>
<th>URL</th>
<th>API key</th>
</tr>
&#10;<tr>
<td><p>DEV and TEST</p>
<p>(please check with ICT VN if accessible)</p></td>
<td><span><a href="https://api-sandbox.spotcap.com/cembra/partner/prequalification/calculate" class="external-link" rel="nofollow">https://api-sandbox.spotcap.com/cembra/partner/prequalification/calculate</a></span></td>
<td>x-apikey: 587vlJEszICtvbZ9MvJwyhA8cpxQDRPM</td>
</tr>
<tr>
<td>PROD</td>
<td>tbd</td>
<td>tbd</td>
</tr>
</tbody>
</table>

</div>

## Landing page at Cembra

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Env</th>
<th>URL</th>
</tr>
&#10;<tr>
<td><p>DEV and TEST</p>
<p>(please check with ICT VN if accessible)</p></td>
<td><a href="https://app-framework-staging.spotcap-cembra.com" class="external-link" rel="nofollow">https://app-framework-staging.spotcap-cembra.com</a></td>
</tr>
<tr>
<td>PROD</td>
<td>tbd</td>
</tr>
</tbody>
</table>

</div>

  

===============================================================================

# Deprecated

  

API :

<a href="https://documenter.getpostman.com/view/5504766/SVmtzzh6?version=latest#2f51c728-77d3-4b09-b7be-4164ea5eef3c" class="external-link" rel="nofollow">https://documenter.getpostman.com/view/5504766/SVmtzzh6?version=latest#2f51c728-77d3-4b09-b7be-4164ea5eef3c</a>

<a href="https://cembrabusiness.docs.stoplight.io/api" class="external-link" rel="nofollow">https://cembrabusiness.docs.stoplight.io/api</a>

  

UI

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="bd47cc82-adde-47de-bd8a-61c4bcb2834f" macro-name="view-file"><a href="../_attachments/20496030223-KLARA_text_reviewed_v2.docx" class="confluence-embedded-file" data-nice-type="Microsoft Word Document" data-file-src="/wiki/download/attachments/20496030223/KLARA_text_reviewed_v2.docx?version=1&amp;modificationDate=1569507756000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/vnd.openxmlformats-officedocument.wordprocessingml.document" data-has-thumbnail="true">

![[20496030223-KLARA_text_reviewed_v2.docx]]

</a></span>
