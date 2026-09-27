---
ai_hash: 38acebee94342db5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 43
depth: 3
entities: []
relevance: 0.886
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47567929416/LUZ-110826+Public+API+-+Widget+subscription+by+activation+code
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: LUZ-110826 Public API - Widget subscription by activation code
topic: programming
type: source
updated: 2023-12-06
---

# LUZ-110826 Public API - Widget subscription by activation code

> [!info] Imported from Confluence
> Space **TS** · updated 2023-12-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47567929416/LUZ-110826+Public+API+-+Widget+subscription+by+activation+code)
> Relevance 0.886 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47567929416_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-110826" macro-id="38061e4a-a505-461b-b6cf-36516c613f1b" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-110826" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-110826</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

## **1. DATA**

<div id="expander-1392480121" class="expand-container conf-macro output-block" hasbody="true" macro-id="13e14cd1-a8e6-40e0-9c19-0751fcf15e81" macro-name="expand">

<div id="expander-control-1392480121" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">example json</span>

</div>

<div id="expander-content-1392480121" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fbb1822d-f503-4f5a-86e5-ee735c8e4dc8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "name": "Fairgate API With Activation code",
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

Guide to create marketing code : <a href="https://axonivy.atlassian.net/wiki/spaces/KS/pages/18782087954?atlOrigin=eyJpIjoiZTIxYzU1MmUxMDk1NGY2ZTk1OTk5ZWY1MTMwZDhiMGEiLCJwIjoiYyJ9" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/KS/pages/18782087954?atlOrigin=eyJpIjoiZTIxYzU1MmUxMDk1NGY2ZTk1OTk5ZWY1MTMwZDhiMGEiLCJwIjoiYyJ9</a>

## **2. TEST REPORT CREATE BUSINESS TENANT**

**DEV**

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
<th><p><strong>No</strong></p></th>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Product</strong></p></th>
<th><p><strong>Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><ul>
<li><p>New company from Public API : <code>cfc59306-5519-4e84-93f8-9e58c179f5f3</code></p></li>
<li><p>New product</p></li>
</ul></td>
<td><p>Name : EBILL ACTIVATION CODE TEST</p>
<p>Code : EBILL_ACTIVATION_CODE</p>
<p>Marketing code : K-08-8040-00-M</p>
<p>Widget code : EBILL-ACTIVATION-CODE</p>
<p>Price plan : Monthly</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-103846.png]]


<p>

![[47567929416-check.png]]

 Existing subscription</p>

![[47567929416-image-20231124-104117.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><ul>
<li><p>New company from Public API : <code>cfc59306-5519-4e84-93f8-9e58c179f5f3</code></p></li>
<li><p>New product</p></li>
</ul></td>
<td><p>Name : EBILL ACTIVATION CODE TEST 2</p>
<p>Code : EBILL_ACTIVATION_CODE_2</p>
<p>Marketing code : K-09-9042-01-M,K-09-9042-02-M</p>
<p>Widget code : EBILL-ACTIVATION-CODE-2</p>
<p>Price plan : Monthly</p>
<p>Variant : 2</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-110012.png]]

</td>
</tr>
<tr>
<td><p>3</p></td>
<td><ul>
<li><p>New company from Public API : <code>cfc59306-5519-4e84-93f8-9e58c179f5f3</code></p></li>
<li><p>New product</p></li>
</ul></td>
<td><p>Name : EBILL ACTIVATION CODE TEST 3</p>
<p>Code : EBILL_ACTIVATION_CODE_3</p>
<p>Marketing code : K-08-8042-00-V120</p>
<p>Widget code : EBILL-ACTIVATION-CODE-3</p>
<p>Price plan : Volume</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-110121.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><ul>
<li><p>New company from Public API : <code>9ba91876-99a3-449a-91b2-91985441334</code></p></li>
<li><p>New products</p></li>
</ul></td>
<td><p>Marketing codes :</p>
<ul>
<li><p>K-08-8040-00-M</p></li>
<li><p>K-09-9042-01-M</p></li>
<li><p>K-08-8042-00-V120</p></li>
</ul></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231127-015723.png]]

![[47567929416-image-20231127-015755.png]]

</td>
</tr>
<tr>
<td><p>5</p></td>
<td><ul>
<li><p>New company from Public API : <code>cfc59306-5519-4e84-93f8-9e58c179f5f3</code></p></li>
<li><p>Existing product</p></li>
</ul></td>
<td><p>Name : ebill - Send invoice</p>
<p>Code : EBILL_HIDDEN_PRICING</p>
<p>Marketing code : K-08-8043-00-Q</p>
<p>Widget code : EBILL_HIDDEN_PRICING</p>
<p>Price plan : Quarterly</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-110446.png]]

</td>
</tr>
<tr>
<td><p>6</p></td>
<td><ul>
<li><p>Existing company create on KLARA GUI : <code>75ff4b57-3f3c-476c-a28b-63fb39ee9939</code></p></li>
<li><p>Existing product</p></li>
</ul></td>
<td><p>Name : ebill - Send invoice</p>
<p>Code : EBILL_HIDDEN_PRICING</p>
<p>Marketing code : K-08-8043-00-Q</p>
<p>Widget code : EBILL_HIDDEN_PRICING</p>
<p>Price plan : Quarterly</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-111644.png]]

</td>
</tr>
</tbody>
</table>

</div>

**Staging**

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
<th><p><strong>No</strong></p></th>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Product</strong></p></th>
<th><p><strong>Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><ul>
<li><p>New company from Public API : <code>a02f2fbe-00c3-48c5-abaf-52013b9c5ec2</code></p></li>
<li><p>Company name : Fairgate API With Activation code 1</p></li>
<li><p>New product</p></li>
</ul></td>
<td><p>Name : EBILL ACTIVATION CODE TEST</p>
<p>Code : EBILL_ACTIVATION_CODE</p>
<p>Marketing code : K-08-8040-00-M</p>
<p>Widget code : EBILL-ACTIVATION-CODE</p>
<p>Price plan : Monthly</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231206-041446.png]]


<p>

![[47567929416-check.png]]

 Existing subscription → But still wrong message</p>

![[47567929416-image-20231206-041747.png]]

</td>
</tr>
<tr>
<td><p>2</p></td>
<td><ul>
<li><p>New company from Public API : <code>a02f2fbe-00c3-48c5-abaf-52013b9c5ec2</code></p></li>
<li><p>Company name : Fairgate API With Activation code 1</p></li>
<li><p>New product</p></li>
</ul></td>
<td><p>Name : EBILL ACTIVATION CODE TEST 2</p>
<p>Code : EBILL_ACTIVATION_CODE_2</p>
<p>Marketing code : K-09-9042-01-M,K-09-9042-02-M</p>
<p>Widget code : EBILL-ACTIVATION-CODE-2</p>
<p>Price plan : Monthly</p>
<p>Variant : 2</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231206-041823.png]]

</td>
</tr>
<tr>
<td><p>3</p></td>
<td><ul>
<li><p>New company from Public API : <code>a02f2fbe-00c3-48c5-abaf-52013b9c5ec2</code></p></li>
<li><p>Company name : Fairgate API With Activation code 1</p></li>
<li><p>New product</p></li>
</ul></td>
<td><p>Name : EBILL ACTIVATION CODE TEST 3</p>
<p>Code : EBILL_ACTIVATION_CODE_3</p>
<p>Marketing code : K-08-8042-00-V120</p>
<p>Widget code : EBILL-ACTIVATION-CODE-3</p>
<p>Price plan : Volume</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231206-041840.png]]

</td>
</tr>
<tr>
<td><p>4</p></td>
<td><ul>
<li><p>New company from Public API : <code>bfd80b0b-f699-484e-95d3-63b78872132e</code></p></li>
<li><p>Company name : Fairgate API With Activation code 2</p></li>
<li><p>New products</p></li>
</ul></td>
<td><p>Marketing codes :</p>
<ul>
<li><p>K-08-8040-00-M</p></li>
<li><p>K-09-9042-01-M</p></li>
<li><p>K-08-8042-00-V120</p></li>
</ul></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231127-015723.png]]

![[47567929416-image-20231206-041945.png]]

</td>
</tr>
<tr>
<td><p>5</p></td>
<td><ul>
<li><p>New company from Public API : <code>a02f2fbe-00c3-48c5-abaf-52013b9c5ec2</code></p></li>
<li><p>Company name : Fairgate API With Activation code 1</p></li>
<li><p>Existing product</p></li>
</ul></td>
<td><p>Name : Rechnung als eBill versenden</p>
<p>Code : EBILL</p>
<p>Marketing code : K-08-8043-00-S</p>
<p>Widget code : EBILL</p>
<p>Price plan : SINGLE</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-110446.png]]

</td>
</tr>
<tr>
<td><p>6</p></td>
<td><ul>
<li><p>Existing company create on KLARA GUI : <code>a04d4146-44bd-4db9-b0ad-c2250d0e34b6</code></p></li>
<li><p>Company name : Fairgate Customer</p></li>
<li><p>Existing product</p></li>
</ul></td>
<td><p>Name : Rechnung als eBill versenden</p>
<p>Code : EBILL</p>
<p>Marketing code : K-08-8043-00-S</p>
<p>Widget code : EBILL</p>
<p>Price plan : SINGLE</p>
<p>Variant : NO</p></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231124-111644.png]]

</td>
</tr>
</tbody>
</table>

</div>

## **3. INVOICE RUN**

**DEV**

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
<th></th>
<th><p><strong>Description</strong></p></th>
<th></th>
<th><p><strong>Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>New company tenant</p></td>
<td colspan="2"><ul>
<li><p>7d8ccd5a-5bf2-440d-bd02-38fb61f18281 : Create Company tenant and subscription from public API<br />
Partner name : Fairgate API With invoice run 1</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b36fd4ef-6b9c-48f6-96c3-b44eabf308bf" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>marketing codes : K-08-8040-00-M, K-09-9042-01-M, K-08-8042-00-V120</code></pre>
</div>
</div></li>
<li><p>4e230421-ba82-4e80-b25a-209a17af8fa7 : Create Company tenant and subscription<br />
Partner name : Fairgate API With invoice run 2</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c8820476-76d9-42ab-8f12-425d4f7697e4" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>marketing codes : K-08-8043-00-Q, K-09-9042-02-M</code></pre>
</div>
</div></li>
<li><p>8707c1a1-9ce8-4530-af1b-7a655151dca3 : Create company tenants and subscription on KLARA GUI<br />
Partner name : Fairgate customer 500</p></li>
</ul>
<p><strong>How to trigger invoice run</strong></p>
<ul>
<li><p>Should choose product with price plan is volume and change <code>consumption_date</code> to previous month on billing table.</p></li>
<li><p>user : <a href="mailto:klaratenant@axonivy.io" class="external-link" rel="nofollow">klaratenant@axonivy.io</a> | <a href="mailto:klaratenant@axonivy.io" class="external-link" rel="nofollow">KlaraTenant</a></p>

![[47567929416-image-20231128-070241.png]]

</li>
</ul></td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231128-063612.png]]


<p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231128-063644.png]]


<p>Invoice files : <span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="fda96213-028b-454f-94ea-9acf4fc4f500" data-macro-name="view-file"><a href="../_attachments/47567929416-PDF-invoices_28.11.2023_07_37_16.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/47567929416/PDF-invoices_28.11.2023_07_37_16.zip?version=2&amp;modificationDate=1701153498898&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[47567929416-PDF-invoices_28.11.2023_07_37_16.zip]]

</a></span></p></td>
</tr>
<tr>
<td><p>Sync up company tenant to KLARA Business AG (Partner) when calling update company from public API</p></td>
<td><h1 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Beforeupdate" style="text-align: center;">Before update</h1>
<ul>
<li><p>Company tenant<br />
- Name : Fairgate API With invoice run 1<br />
- Tenant : <code>7d8ccd5a-5bf2-440d-bd02-38fb61f18281</code><br />
- Email : <a href="mailto:miracle_public_api_dev_500@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_dev_500@axongroupio.ch</a><br />
- Phone : +41 78 123 23 43<br />
- Address : Chemin de la Caquerette 12<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
</p></li>
<li><p>Partner AG<br />
- Name : Fairgate API With invoice run 1<br />
- Email : <a href="mailto:miracle_public_api_dev_500@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_dev_500@axongroupio.ch</a><br />
- Phone : +41 78 123 23 43<br />
- Address : Chemin de la Caquerette 12</p></li>
</ul></td>
<td><h2 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Afterupdateviapublicapi" style="text-align: center;">After update via public api</h2>

![[47567929416-image-20231129-071748.png]]

![[47567929416-image-20231129-072038.png]]



![[47567929416-image-20231129-071942.png]]

![[47567929416-image-20231129-085424.png]]


<p><br />
</p></td>
<td><p>

![[47567929416-check.png]]

</p>
<p>Invoice run overview</p>

![[47567929416-image-20231129-073911.png]]


<p>Send to new email</p>

![[47567929416-image-20231129-073854.png]]


<p>Invoice pdf <span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="a9334bea-e2c5-4bc7-b952-3ced1624e041" data-macro-name="view-file"><a href="../_attachments/47567929416-PDF-invoices_29.11.2023_08_36_53.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/47567929416/PDF-invoices_29.11.2023_08_36_53.zip?version=1&amp;modificationDate=1701243614352&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[47567929416-PDF-invoices_29.11.2023_08_36_53.zip]]

</a></span></p></td>
</tr>
</tbody>
</table>

</div>

**Staging**

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
<th></th>
<th><p><strong>Description</strong></p></th>
<th></th>
<th><p><strong>Result</strong></p></th>
</tr>
&#10;<tr>
<td><p>New company tenant</p></td>
<td colspan="2"><p><strong>How to trigger invoice run</strong></p>
<ul>
<li><p>Should choose product with price plan is volume and change <code>consumption_date</code> to previous month on billing table.</p></li>
<li><p>user : <a href="mailto:klaratenant@axonivy.io" class="external-link" rel="nofollow">klaratenant@axonivy.io</a> | <a href="mailto:klaratenant@axonivy.io" class="external-link" rel="nofollow">KlaraTenant</a></p></li>
</ul>

![[47567929416-image-20231206-063516.png]]

</td>
<td><p>

![[47567929416-check.png]]

</p>

![[47567929416-image-20231206-063715.png]]


<p>Invoice files : <span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="ae377667-400c-4221-9cf1-08dcb90d7437" data-macro-name="view-file"><a href="../_attachments/47567929416-PDF-invoices_06.12.2023_07_36_41.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/47567929416/PDF-invoices_06.12.2023_07_36_41.zip?version=1&amp;modificationDate=1701844623406&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[47567929416-PDF-invoices_06.12.2023_07_36_41.zip]]

</a></span></p></td>
</tr>
<tr>
<td><p>Sync up company tenant to KLARA Business AG (Partner) when calling update company from public API</p></td>
<td><h1 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Beforeupdate.1" style="text-align: center;">Before update</h1>
<ul>
<li><p>Company teant<br />
- Name : Fairgate API With Activation code 1<br />
- Tenant : <code>a02f2fbe-00c3-48c5-abaf-52013b9c5ec2</code><br />
- Email : <a href="mailto:miracle_public_api_dev_500@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_dev_500@axongroupio.ch</a><br />
- Phone : +41 78 123 23 43<br />
- Address : Chemin de la Caquerette 12</p></li>
<li><p>Partner AG<br />
- Name : <strong>Fairgate API With Activation code 1</strong><br />
- Email : <a href="mailto:miracle_public_api_dev_500@axongroupio.ch" class="external-link" rel="nofollow">miracle_public_api_dev_500@axongroupio.ch</a><br />
- Phone : +41 78 123 23 43<br />
- Address : Chemin de la Caquerette 12</p></li>
</ul></td>
<td><h2 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Afterupdateviapublicapi.1" style="text-align: center;">After update via public api</h2>

![[47567929416-image-20231206-065405.png]]



![[47567929416-image-20231206-065523.png]]

</td>
<td><p>

![[47567929416-check.png]]

</p>
<p>Invoice run overview</p>

![[47567929416-image-20231206-064852.png]]


<p>Send to new email</p>

![[47567929416-image-20231206-064806.png]]


<p>Invoice pdf <span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="61e36475-6975-4fa3-8fca-f2f0226c0920" data-macro-name="view-file"><a href="../_attachments/47567929416-PDF-invoices_06.12.2023_07_48_12.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/47567929416/PDF-invoices_06.12.2023_07_48_12.zip?version=1&amp;modificationDate=1701845319395&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[47567929416-PDF-invoices_06.12.2023_07_48_12.zip]]

</a></span></p></td>
</tr>
</tbody>
</table>

</div>

## **4. CODE REVIEW REPORT**

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
<th><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-No."><strong>No.</strong></h3></th>
<th><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-HavecoveredJUnittests?"><strong>Have covered JUnit tests?</strong></h3>
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
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-(checkotherplacesthatcalltothis)">(<em>check other places that call to this</em>)</h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
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
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Noduplicatedcode?"><strong>No duplicated code?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-AttachjenkinbuildresultinPR"><strong>Attach jenkin build result in PR</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Checkingimpactwithintegrationtest"><strong>Checking impact with integration test</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Functioniscorrectpurpose(noneedtosplitfunction)"><strong>Function is correct purpose ( no need to split function)</strong></h3>
<h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Datatypeiscorrect"><strong>Datatype is correct</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
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
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
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
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Canusecaching?"><strong>Can use caching?</strong></h3>
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
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Checkcorrectionofusingbeanscopes"><strong>Check correction of using  bean scopes</strong></h3></td>
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
<td colspan="3"><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Followednamingconversion"><strong>Followed naming conversion</strong></h3></td>
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
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>14</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>15</p></td>
<td><h3 id="LUZ-110826PublicAPI-Widgetsubscriptionbyactivationcode-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[Test Keycloak - Public API]]
- [[LUZ-115505 Public API - letterbox Part 3]]
- [[How to use Public API to create update KLARA Business Company]]
- [[Public API Print Partner]]

%% ai-graph-end %%