---
title: "LUZ-109076 Public API - Create/update new tenant (implementation)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47552659507/LUZ-109076+Public+API+-+Create+update+new+tenant+implementation
space: "TS"
topic: programming
relevance: 0.847
depth: 3
updated: 2023-11-22
attachments: 5
tags:
  - confluence
  - programming
  - space/ts
---

# LUZ-109076 Public API - Create/update new tenant (implementation)

> [!info] Imported from Confluence
> Space **TS** · updated 2023-11-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47552659507/LUZ-109076+Public+API+-+Create+update+new+tenant+implementation)
> Relevance 0.847 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47552659507_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-109076" macro-id="86abbdbb-c361-4d34-9cc3-9bf619560d99" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-109076" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-109076</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47552659507_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-109244" macro-id="d23ec05f-edda-47ec-a4c0-7d5c4200b058" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-109244" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-109244</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## **1. DATA**

<div id="expander-96850651" class="expand-container conf-macro output-block" hasbody="true" macro-id="16553f4d-b34a-449b-8c0a-f0c1cc944b0e" macro-name="expand">

<div id="expander-control-96850651" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">example json</span>

</div>

<div id="expander-content-96850651" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c6f88a33-2d2d-4e9a-a5b3-21a1625a49dc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "name": "From Api 1",
  "legalForm": "KG",
  "phones": [
    {
      "phoneNumber": "+41 78 123 23 43",
      "type": "OFFICE"
    }
  ],
  "emails": [
    {
      "emailAddress": "miracle_public_api_1@axongroupio.ch",
      "type": "OFFICE"
    }
  ],
  "addresses": [
    {
      "addressLines": "Chemin de la Caquerette 12",
      "addressType": "PRIVATE",
      "cityName": "Bern",
      "cityZipCode": "3003",
      "countryIso2Code": "CH",
      "countryIso3Code": "CHE",
      "countryNumericCode": "756",
      "additionalAddress": "No. 13, street 123"
    }
  ],
  "language": "de"
}
```

</div>

</div>

</div>

</div>

Guide to test: [How to use Public API to create/update KLARA Business Company](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47553806568/How+to+use+Public+API+to+create+update+KLARA+Business+Company)

## **2. TEST REPORT CREATE BUSINESS TENANT**

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Field</strong></p></th>
<th><p><strong>Is required</strong></p></th>
<th><p><strong>Rule</strong></p></th>
<th><p><strong>Staging</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Example</strong></p></th>
</tr>
&#10;<tr>
<td colspan="2"><p><code>name</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Free input 

![[47552659507-check.png]]

</p>
<p>Can not be blank 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>the name of the company</p></td>
<td><p><code>From Api 15</code></p></td>
</tr>
<tr>
<td colspan="2"><p><code>legalForm</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>One of these data 

![[47552659507-check.png]]

</p>
<div id="expander-1382257916" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="aab8638f-64a2-4be7-aa3e-46a10dca9697" data-macro-name="expand">
<div id="expander-control-1382257916" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">legal form types</span>
</div>
<div id="expander-content-1382257916" class="expand-content expand-hidden">
<p>Individually owned company</p>
<p>General partnership</p>
<p>Limited partnership</p>
<p>Public limited company</p>
<p>Limited liability company</p>
<p>Cooperative company</p>
<p>Association</p>
<p>Foundation</p>
<p>Simple partnership</p>
<p>Other</p>
</div>
</div></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>the legal form of the company</p></td>
<td><div id="expander-1266528792" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="8440ba5d-751b-490a-bc85-1dd8c3cc8070" data-macro-name="expand">
<div id="expander-control-1266528792" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">data</span>
</div>
<div id="expander-content-1266528792" class="expand-content expand-hidden">
<p>Individually owned company → <code>EU</code></p>
<p>General partnership → <code>KG</code></p>
<p>Limited partnership → <code>KDG</code></p>
<p>Public limited company → <code>AG</code></p>
<p>Limited liability company → <code>GmbH</code></p>
<p>Cooperative company → <code>G</code></p>
<p>Association → <code>V</code></p>
<p>Foundation → <code>S</code></p>
<p>Simple partnership → <code>EG</code></p>
<p>Other → <code>OR</code></p>
</div>
</div></td>
</tr>
<tr>
<td colspan="2"><p><code>language</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Support languages: 

![[47552659507-check.png]]

</p>
<div id="expander-75878067" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="9bb3768b-2f22-493e-8573-88978b44fd76" data-macro-name="expand">
<div id="expander-control-75878067" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">list language</span>
</div>
<div id="expander-content-75878067" class="expand-content expand-hidden">
<p>english<br />
german<br />
french<br />
italian</p>
</div>
</div></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><div id="expander-702898005" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="17cbcddf-7f8c-4eb7-aaeb-288abc4df5a4" data-macro-name="expand">
<div id="expander-control-702898005" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">list language</span>
</div>
<div id="expander-content-702898005" class="expand-content expand-hidden">
<p>German: <strong>de</strong></p>
<p>English: <strong>en</strong></p>
<p>French: <strong>fr</strong></p>
<p>Italian: <strong>it</strong></p>
</div>
</div></td>
<td><p><code>en</code></p></td>
</tr>
<tr>
<td colspan="2"><p><code>foundingDate</code></p></td>
<td><p>Optional</p></td>
<td><p>Format: <code>YYYY-MM-DD</code> 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Format: <code>YYYY-MM-DD</code></p></td>
<td><p><code>2019-12-20</code></p></td>
</tr>
<tr>
<td rowspan="2"><p><code>phones</code></p></td>
<td><p><code>phoneNumber</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Only support phone from SWISS 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>The phone number of the company</p></td>
<td><p><code>+41783334444</code></p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Only office possible 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>The phone type, must be <code>OFFICE</code></p></td>
<td><p><code>OFFICE</code></p></td>
</tr>
<tr>
<td rowspan="2"><p><code>emails</code></p></td>
<td><p><code>emailAddress</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Free input → should be email format 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>The email of the company</p></td>
<td><p><code>miracle_public_api_1@axongroupio.ch</code></p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Should be OFFICE type 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>The email type, must be <code>OFFICE</code></p></td>
<td><p><code>OFFICE</code></p></td>
</tr>
<tr>
<td rowspan="8"><p><code>addresses</code></p></td>
<td><p><code>addressLines</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Free input 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Street name and house number</p></td>
<td><p><code>Chemin de la Caquerette 12</code></p></td>
</tr>
<tr>
<td><p><code>addressType</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Should be PRIVATE type 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Must be <code>PRIVATE</code></p></td>
<td><p><code>PRIVATE</code></p></td>
</tr>
<tr>
<td><p><code>cityName</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Only with Switzerland 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>The name of the city</p></td>
<td><p><code>Bern</code></p></td>
</tr>
<tr>
<td><p><code>cityZipCode</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Only with Switzerland 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>The postcode</p></td>
<td><p><code>3003</code></p></td>
</tr>
<tr>
<td><p><code>countryIso2Code</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Only with Switzerland 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Must be <code>CH</code></p></td>
<td><p><code>CH</code></p></td>
</tr>
<tr>
<td><p><code>countryIso3Code</code></p></td>
<td><p>Optional</p></td>
<td><p>Only with Switzerland 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Must be <code>CHE</code></p></td>
<td><p><code>CHE</code></p></td>
</tr>
<tr>
<td><p><code>countryNumericCode</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Only with Switzerland 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>Must be <code>756</code></p></td>
<td><p><code>756</code></p></td>
</tr>
<tr>
<td><p><code>additionalAddress</code></p></td>
<td><p>Optional</p></td>
<td><p>Free input 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>More information about the address</p></td>
<td><p><code>No. 13, street 123</code></p></td>
</tr>
</tbody>
</table>

</div>

<div hasbody="true" macro-id="d76a93f8-b6ca-4ee0-941b-ee9dfba54a1d" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Hubspot data

</div>

</div>

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
<th><p><strong>Expectation</strong></p></th>
<th><p><strong>Staging</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Create new company</p></td>
<td><p>Should create the company in Hubspot 

![[47552659507-check.png]]

</p>

![[47552659507-image-20231116-031948.png]]

</td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>Subscribe 1 widget</p></td>
<td><p>Should add a deal to the company 

![[47552659507-check.png]]

</p>

![[47552659507-DYouOr70ds.png]]

</td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## **3. TEST REPORT GET BUSINESS TENANT INFORMATION**

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Field</strong></p></th>
<th><p><strong>Correct data</strong></p></th>
<th><p><strong>Staging</strong></p></th>
<th><p><strong>Request</strong></p></th>
<th><p><strong>Response</strong></p></th>
</tr>
&#10;<tr>
<td colspan="2"><p><code>name</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td rowspan="16"><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="977e101d-e980-4f15-9e04-43a7d1bac54e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;name&quot;: &quot;From Api 19&quot;,
  &quot;legalForm&quot;: &quot;KG&quot;,
  &quot;phones&quot;: [
    {
      &quot;phoneNumber&quot;: &quot;+41 78 123 23 43&quot;,
      &quot;type&quot;: &quot;OFFICE&quot;
    }
  ],
  &quot;emails&quot;: [
    {
      &quot;emailAddress&quot;: &quot;miracle_public_api_1@axongroupio.ch&quot;,
      &quot;type&quot;: &quot;OFFICE&quot;
    }
  ],
  &quot;addresses&quot;: [
    {
      &quot;addressLines&quot;: &quot;Chemin de la Caquerette 12&quot;,
      &quot;addressType&quot;: &quot;PRIVATE&quot;,
      &quot;cityName&quot;: &quot;Bern&quot;,
      &quot;cityZipCode&quot;: &quot;3003&quot;,
      &quot;countryIso2Code&quot;: &quot;CH&quot;,
      &quot;countryIso3Code&quot;: &quot;CHE&quot;,
      &quot;countryNumericCode&quot;: &quot;756&quot;,
      &quot;additionalAddress&quot;: &quot;No. 13, street 123&quot;
    }
  ],
  &quot;language&quot;: &quot;de&quot;
}</code></pre>
</div>
</div></td>
<td rowspan="16"><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b19b16f3-9890-4c3a-a8a4-732edd8256f8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;name&quot;: &quot;From Api 19&quot;,
    &quot;legalForm&quot;: &quot;GENERAL_PARTNERSHIP&quot;,
    &quot;phones&quot;: [
        {
            &quot;id&quot;: &quot;1&quot;,
            &quot;phoneNumber&quot;: &quot;+41 78 123 23 43&quot;,
            &quot;type&quot;: &quot;OFFICE&quot;
        }
    ],
    &quot;emails&quot;: [
        {
            &quot;id&quot;: &quot;1&quot;,
            &quot;emailAddress&quot;: &quot;miracle_public_api_1@axongroupio.ch&quot;,
            &quot;type&quot;: &quot;OFFICE&quot;
        }
    ],
    &quot;addresses&quot;: [
        {
            &quot;id&quot;: &quot;1&quot;,
            &quot;validFrom&quot;: &quot;2023-11-15&quot;,
            &quot;validTo&quot;: null,
            &quot;addressLines&quot;: &quot;Chemin de la Caquerette 12&quot;,
            &quot;addressType&quot;: &quot;PRIVATE&quot;,
            &quot;cityName&quot;: &quot;Bern&quot;,
            &quot;cityZipCode&quot;: &quot;3003&quot;,
            &quot;countryIso2Code&quot;: &quot;CH&quot;,
            &quot;countryIso3Code&quot;: &quot;CHE&quot;,
            &quot;countryNumericCode&quot;: &quot;756&quot;,
            &quot;definitionName&quot;: null,
            &quot;additionalAddress&quot;: &quot;No. 13, street 123&quot;,
            &quot;city_href&quot;: null
        }
    ],
    &quot;language&quot;: &quot;de&quot;,
    &quot;corporateIdentificationNumber&quot;: null,
    &quot;vatNumber&quot;: null,
    &quot;hrNumber&quot;: null,
    &quot;nogaCode&quot;: null,
    &quot;foundingDate&quot;: null
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td colspan="2"><p><code>legalForm</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td colspan="2"><p><code>language</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td colspan="2"><p><code>foundingDate</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="2"><p><code>phones</code></p></td>
<td><p><code>phoneNumber</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="2"><p><code>emails</code></p></td>
<td><p><code>emailAddress</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="8"><p><code>addresses</code></p></td>
<td><p><code>addressLines</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>addressType</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>cityName</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>cityZipCode</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>countryIso2Code</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>countryIso3Code</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>countryNumericCode</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>additionalAddress</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

## **4. TEST REPORT UPDATE BUSINESS TENANT**

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Field</strong></p></th>
<th><p><strong>Correct data</strong></p></th>
<th><p><strong>Staging</strong></p></th>
<th><p><strong>Request update</strong></p></th>
<th><p><strong>Response</strong></p></th>
<th><p><strong>Get company information</strong></p></th>
</tr>
&#10;<tr>
<td colspan="2"><p><code>name</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td rowspan="16"><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="99411cec-bdec-4a2e-bf75-aeb17db6f100" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;name&quot;: &quot;From Api 21 updated&quot;,
  &quot;legalForm&quot;: &quot;EU&quot;,
  &quot;phones&quot;: [
    {
      &quot;phoneNumber&quot;: &quot;+41 78 321 45 64&quot;,
      &quot;type&quot;: &quot;OFFICE&quot;
    }
  ],
  &quot;emails&quot;: [
    {
      &quot;emailAddress&quot;: &quot;miracle_public_api_2@axongroupio.ch&quot;,
      &quot;type&quot;: &quot;OFFICE&quot;
    }
  ],
  &quot;addresses&quot;: [
    {
      &quot;addressLines&quot;: &quot;testing update&quot;,
      &quot;addressType&quot;: &quot;PRIVATE&quot;,
      &quot;cityName&quot;: &quot;Aarau&quot;,
      &quot;cityZipCode&quot;: &quot;5000&quot;,
      &quot;countryIso2Code&quot;: &quot;CH&quot;,
      &quot;countryIso3Code&quot;: &quot;CHE&quot;,
      &quot;countryNumericCode&quot;: &quot;756&quot;,
      &quot;additionalAddress&quot;: &quot;test additionalAddress&quot;
    }
  ],
  &quot;language&quot;: &quot;en&quot;
}</code></pre>
</div>
</div></td>
<td rowspan="16"><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b6abffd7-97e8-4832-a7c2-f91ed9fd9fce" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;name&quot;: &quot;From Api 21 updated&quot;,
    &quot;legalForm&quot;: &quot;INDIVIDUALLY_OWNED_COMPANY&quot;,
    &quot;phones&quot;: [
        {
            &quot;id&quot;: &quot;9&quot;,
            &quot;phoneNumber&quot;: &quot;+41 78 321 45 64&quot;,
            &quot;type&quot;: &quot;OFFICE&quot;
        }
    ],
    &quot;emails&quot;: [
        {
            &quot;id&quot;: &quot;9&quot;,
            &quot;emailAddress&quot;: &quot;miracle_public_api_2@axongroupio.ch&quot;,
            &quot;type&quot;: &quot;OFFICE&quot;
        }
    ],
    &quot;addresses&quot;: [
        {
            &quot;id&quot;: &quot;9&quot;,
            &quot;validFrom&quot;: &quot;2023-11-15&quot;,
            &quot;validTo&quot;: null,
            &quot;addressLines&quot;: &quot;testing update&quot;,
            &quot;addressType&quot;: &quot;PRIVATE&quot;,
            &quot;cityName&quot;: &quot;Aarau&quot;,
            &quot;cityZipCode&quot;: &quot;5000&quot;,
            &quot;countryIso2Code&quot;: &quot;CH&quot;,
            &quot;countryIso3Code&quot;: &quot;CHE&quot;,
            &quot;countryNumericCode&quot;: &quot;756&quot;,
            &quot;definitionName&quot;: null,
            &quot;additionalAddress&quot;: &quot;test additionalAddress&quot;,
            &quot;city_href&quot;: null
        }
    ],
    &quot;language&quot;: &quot;de&quot;,
    &quot;corporateIdentificationNumber&quot;: null,
    &quot;vatNumber&quot;: null,
    &quot;hrNumber&quot;: null,
    &quot;nogaCode&quot;: null,
    &quot;foundingDate&quot;: null
}</code></pre>
</div>
</div></td>
<td rowspan="16"><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f5659248-6728-473a-9169-02f2e8bdb60d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;name&quot;: &quot;From Api 21 updated&quot;,
    &quot;legalForm&quot;: &quot;INDIVIDUALLY_OWNED_COMPANY&quot;,
    &quot;phones&quot;: [
        {
            &quot;id&quot;: &quot;9&quot;,
            &quot;phoneNumber&quot;: &quot;+41 78 321 45 64&quot;,
            &quot;type&quot;: &quot;OFFICE&quot;
        }
    ],
    &quot;emails&quot;: [
        {
            &quot;id&quot;: &quot;9&quot;,
            &quot;emailAddress&quot;: &quot;miracle_public_api_2@axongroupio.ch&quot;,
            &quot;type&quot;: &quot;OFFICE&quot;
        }
    ],
    &quot;addresses&quot;: [
        {
            &quot;id&quot;: &quot;9&quot;,
            &quot;validFrom&quot;: &quot;2023-11-15&quot;,
            &quot;validTo&quot;: null,
            &quot;addressLines&quot;: &quot;testing update&quot;,
            &quot;addressType&quot;: &quot;PRIVATE&quot;,
            &quot;cityName&quot;: &quot;Aarau&quot;,
            &quot;cityZipCode&quot;: &quot;5000&quot;,
            &quot;countryIso2Code&quot;: &quot;CH&quot;,
            &quot;countryIso3Code&quot;: &quot;CHE&quot;,
            &quot;countryNumericCode&quot;: &quot;756&quot;,
            &quot;definitionName&quot;: null,
            &quot;additionalAddress&quot;: &quot;test additionalAddress&quot;,
            &quot;city_href&quot;: null
        }
    ],
    &quot;language&quot;: &quot;de&quot;,
    &quot;corporateIdentificationNumber&quot;: null,
    &quot;vatNumber&quot;: null,
    &quot;hrNumber&quot;: null,
    &quot;nogaCode&quot;: null,
    &quot;foundingDate&quot;: null
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td colspan="2"><p><code>legalForm</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td colspan="2"><p><code>language</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td colspan="2"><p><code>foundingDate</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="2"><p><code>phones</code></p></td>
<td><p><code>phoneNumber</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="2"><p><code>emails</code></p></td>
<td><p><code>emailAddress</code></p></td>
<td></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>type</code></p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="8"><p><code>addresses</code></p></td>
<td><p><code>addressLines</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>addressType</code></p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>cityName</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>cityZipCode</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>countryIso2Code</code></p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>countryIso3Code</code></p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>countryNumericCode</code></p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
<td><p>Don’t support 

![[47552659507-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>additionalAddress</code></p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
<td><p>

![[47552659507-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

## **5. CODE REVIEW REPORT**

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
<th><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-No."><strong>No.</strong></h3></th>
<th><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-HavecoveredJUnittests?"><strong>Have covered JUnit tests?</strong></h3>
<p>(<em>check possible cases are coverage by JUnit test</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-(checkotherplacesthatcalltothis)">(<em>check other places that call to this</em>)</h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
<p>(<em>check NPE, try/catch, validate...</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Noduplicatedcode?"><strong>No duplicated code?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-AttachjenkinbuildresultinPR"><strong>Attach jenkin build result in PR</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Checkingimpactwithintegrationtest"><strong>Checking impact with integration test</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Functioniscorrectpurpose(noneedtosplitfunction)"><strong>Function is correct purpose ( no need to split function)</strong></h3>
<h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Datatypeiscorrect"><strong>Datatype is correct</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
<p>(<em>Check DB &amp; API calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>9</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
<p>(<em>Check DB &amp; API, method calls</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>10</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Canusecaching?"><strong>Can use caching?</strong></h3>
<p>(<em>Check the data, resource can be cached to improve performance</em>)</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>11</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Checkcorrectionofusingbeanscopes"><strong>Check correction of using  bean scopes</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Followednamingconversion"><strong>Followed naming conversion</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
</ul>
<ul>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>13</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>14</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>15</p></td>
<td><h3 id="LUZ-109076PublicAPI-Create/updatenewtenant(implementation)-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
</tbody>
</table>

</div>
