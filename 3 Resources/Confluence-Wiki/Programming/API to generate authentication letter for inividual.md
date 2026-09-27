---
title: "API to generate authentication letter for inividual"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49008771150/API+to+generate+authentication+letter+for+inividual
space: "TS"
topic: programming
relevance: 0.762
depth: 2.73
updated: 2026-04-13
attachments: 5
tags:
  - confluence
  - programming
  - space/ts
---

# API to generate authentication letter for inividual

> [!info] Imported from Confluence
> Space **TS** · updated 2026-04-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49008771150/API+to+generate+authentication+letter+for+inividual)
> Relevance 0.762 · topic `programming`

# Overview

- Current flow


![[49008771150-image-20251230-100014.png]]



- After moving to the backend to generate the authentication letter


![[49008771150-image-20251230-095830.png]]



# How does Optimus adapt the flow?

1.  Generate a random code and prepare the person authentication letter payload

- Structure

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e1b326ba-6ac7-4388-8bbe-a4d98e879b7b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "salutation": "<salutation>",
    "fullName": "<fullname>",
    "address": "<additional address>\n<main address>",
    "city": "<zipcode and city>",
    "code": "<generated authentication code>",
    "locale": "<locale>"
}
```

</div>

</div>

<div id="expander-1271564599" class="expand-container conf-macro output-block" hasbody="true" macro-id="0e1f3df0-c331-44ff-b869-e84b5364b245" macro-name="expand">

<div id="expander-control-1271564599" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Example</span>

</div>

<div id="expander-content-1271564599" class="expand-content expand-hidden">


![[49008771150-image-20251231-012818.png]]



The request should be:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="517ad1dd-055b-4f09-9e99-b06d7df7628c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "salutation": "Mr",
    "fullName": "3D Systems SA",
    "address": "My additional address\nRoute de l'Ancienne Papeterie 185",
    "city": "1723 Marly",
    "code": "QKAC-D2X4",
    "locale": "en"
```

</div>

</div>

</div>

</div>

2.  Trigger API to generate the letter

- Path: `POST /luz_compensation/api/{tenant-id}/authentication-letter/individual`

- Body: person authentication letter payload

3.  Send the letter through SPSOutline

- API Path: `/luz_sps_outline/api/{tenant-id}/non-billable-print-and-send`

- Body: Multipart form

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Field</strong></p></th>
<th><p><strong>Value</strong></p></th>
</tr>
&#10;<tr>
<td><p><code>file</code></p></td>
<td><p>&lt;generated PDF letter in step 2&gt;</p></td>
</tr>
<tr>
<td><p><code>print_info</code></p></td>
<td><p>If it has an additional address:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5365ea3b-3ad9-43fb-9237-cd92550d8452" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;addressLine1&quot;: &quot;&lt;recipientName&gt;&quot;,
  &quot;addressLine2&quot;: &quot;&lt;additionalAddress&gt;&quot;,
  &quot;addressLine3&quot;: &quot;&lt;street&gt;&quot;,
  &quot;addressLine4&quot;: &quot;&lt;zipCity&gt;&quot;,
  &quot;addressPos&quot;: &quot;LEFT&quot;,
  &quot;transactionText&quot;: &quot;Address verification letter - &lt;recipientName&gt;&quot;,
  &quot;postage&quot;: &quot;A_POST&quot;
}</code></pre>
</div>
</div>
<p>If no additional address:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="deb846c2-f109-4dfd-8e8b-98723eb60889" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;addressLine1&quot;: &quot;&lt;recipientName&gt;&quot;,
  &quot;addressLine2&quot;: &quot;&lt;street&gt;&quot;,
  &quot;addressLine3&quot;: &quot;&lt;zipCity&gt;&quot;,
  &quot;addressPos&quot;: &quot;LEFT&quot;,
  &quot;transactionText&quot;: &quot;Address verification letter - &lt;recipientName&gt;&quot;,
  &quot;postage&quot;: &quot;A_POST&quot;
}</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

4.  Save the authentication letter history

- API Path: `/luztenant/api/authentication-letter-histories`

- Body: JSON

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c6ce8ca5-2485-4deb-ba55-271142f47982" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "tenantId": "<tenant-id>",
  "letterType": "E_POST",
  "salutation": "<salutation>",
  "salutationWithName": "<salutation> <fullname>",
  "locale": "<locale>",
  "letterCode": "<generated-code>",
  "recipientName": "<fullName>",
  "streetAndAdditional": "<address>",
  "zipCity": "<zipcode> <city>",
  "requestDate": "<now()>",
}
```

</div>

</div>

<div hasbody="true" macro-id="aae368a4-4705-4e2b-8f2f-b8d06f8a6446" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

For more details, please check out the existing flow:  
<a href="https://bitbucket.org/axonivy-prod/luz_components/src/c64638a42a77941a06d0193fc1e26cea4e052c6b/src/com/axonivy/luz/components/mylife/service/AuthenticationLetterService.java#lines-63" class="external-link" data-card-appearance="inline" data-local-id="223df2ae-7b97-4f7c-a367-dc6d72a0017d" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_components/src/c64638a42a77941a06d0193fc1e26cea4e052c6b/src/com/axonivy/luz/components/mylife/service/AuthenticationLetterService.java#lines-63</a>

</div>

</div>

### SPS outline will be terminated at the end of April, so we should switch to ONE API

I create new APIs with help from AI, Branch : <a href="https://bitbucket.org/axonivy-prod/luz_compensation/branch/miracle/LUZ-144972/generate-authentication-lette" class="external-link" data-card-appearance="inline" data-local-id="c1ab524cc06f" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_compensation/branch/miracle/LUZ-144972/generate-authentication-lette</a>

Document : <a href="https://bitbucket.org/axonivy-prod/luz_compensation/src/9a4d224c987967a2dbaf47bf18f175101ef055f1/docs/authentication-letter-api.md" class="external-link" data-card-appearance="inline" data-local-id="4b9953b6b8f8" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_compensation/src/9a4d224c987967a2dbaf47bf18f175101ef055f1/docs/authentication-letter-api.md</a>
