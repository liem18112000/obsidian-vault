---
ai_hash: a9deb3eba71394cc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502837650/Luz_google+Api+Document
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Luz_google Api Document
topic: programming
type: source
updated: 2020-07-22
---

# Luz_google Api Document

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-07-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20502837650/Luz_google+Api+Document)
> Relevance 0.786 · topic `programming`

### 

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="52c879f3-5363-4026-8fd2-c02e6accbc43" macro-name="toc">

</div>

  
1. GET Categories list

<div>

<table style="width: 90.7801%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details</th>
</tr>
&#10;<tr>
<td rowspan="3">GET</td>
<td>URL</td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/categories</td>
</tr>
<tr>
<td>Parameter</td>
<td><del><span class="inline-comment-marker" data-ref="bec0c470-e7e7-4436-a2ab-fee84e446466">email (user gmail)</span></del>, <span class="inline-comment-marker" data-ref="e6aae50c-3543-46f3-9a0e-2d077218158c">regionCode, languageCode</span></td>
</tr>
<tr>
<td>Response</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1487dd24-365b-4eb7-9a99-d818fbd2aec8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;categories&quot;: [
    {
      &quot;displayName&quot;: &quot;Wedding Portrait Studio&quot;,
      &quot;categoryId&quot;: &quot;gcid:wedding_portrait_studio&quot;
    },
    {
      &quot;displayName&quot;: &quot;Used Store&quot;,
      &quot;categoryId&quot;: &quot;gcid:used_store&quot;
    } ... ] }</code></pre>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

### **2. GET Locations** 

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details</th>
</tr>
&#10;<tr>
<td rowspan="4">GET</td>
<td>URL</td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/locations</td>
</tr>
<tr>
<td>Parameter</td>
<td> <span class="inline-comment-marker" data-ref="6b91161f-b618-436e-ab19-a7278721ed6b"><del>accountName</del> (<del>from 1st  get account call response</del>)</span></td>
</tr>
<tr>
<td><span class="inline-comment-marker" data-ref="967ad44e-0210-4a0e-9a4c-f0a863f4c45e">Response</span></td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5c48fb6d-b702-4f3e-a8c0-fb6ddb06103f" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Empty Location</strong>
</div>
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;locations&quot;: []
}</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8b6f1c78-eaba-4246-9b5a-dabe489d525a" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Existing Location</strong>
</div>
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;locations&quot;: [
    {
      &quot;workplaceId&quot;: &quot;&quot;,
      &quot;locationId&quot;:&quot;&quot;,
      &quot;name&quot;: &quot;Soft Testing Solution&quot;,
      &quot;phone&quot;: &quot;021 234 56 78&quot;,
      &quot;category&quot;: &quot;Computer Software Shop&quot;,
      &quot;websiteUrl&quot;: &quot;http://www.avdevs.com/&quot;,
      &quot;periods&quot;:{[
          {
            &quot;openDay&quot;: &quot;MONDAY&quot;,
            &quot;openTime&quot;: &quot;09:00&quot;,
            &quot;closeDay&quot;: &quot;MONDAY&quot;,
            &quot;closeTime&quot;: &quot;17:00&quot;
          },
          {
            &quot;openDay&quot;: &quot;TUESDAY&quot;,
            &quot;openTime&quot;: &quot;09:00&quot;,
            &quot;closeDay&quot;: &quot;TUESDAY&quot;,
            &quot;closeTime&quot;: &quot;17:00&quot;
          } ...
        ]
      },
      &quot;specialPeriods&quot;:{[
            {
                &quot;startDate&quot;: &quot;2020/12/23&quot;,
                &quot;openTime&quot;: &quot;12:00&quot;,
                &quot;endDate&quot;: &quot;2020/12/23&quot;,
                &quot;closeTime&quot;: &quot;15:00&quot;,
                &quot;isClosed&quot;: false
            }
        ]
      },
      &quot;languageCode&quot;: &quot;en&quot;,
      &quot;address&quot;: {
        &quot;regionCode&quot;: &quot;CH&quot;,
        &quot;postalCode&quot;: &quot;8000&quot;,
        &quot;locality&quot;: &quot;Zurich&quot;,
        &quot;addressLines&quot;: [
          &quot;Bahnhofstrasse 23&quot;
        ]
      },
      &quot;description&quot;: &quot;string&quot;,
      &quot;obcBanner&quot;: &quot;https://lh3.googleusercontent.com/p/AF1QipMuGDk6poXe7Y0is0kWd5pbTYNmI45YdaHZlT5O=s300&quot;,
      &quot;logo&quot;: &quot;https://lh3.googleusercontent.com/p/AF1QipOED5asGXqpzg55OL3ki0TVnbozMCkdR6XWkUvX=s300&quot;,
      &quot;linked&quot; : true,
      &quot;verified&quot;: false,
      &quot;reviewLink&quot; : &quot;https://www.google.com.vn/imgres&quot;,
      &quot;mapsUrl&quot; : &quot;https://www.google.com.vn/imgres&quot;
    } ...
  ]
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td>Note</td>
<td>map the locationId("name": "accounts/1007447829838/locations/1176036302") and mediaId(logo and OBCBanner)  with workplaceId in luz_google DB.</td>
</tr>
</tbody>
</table>

</div>

### **3. Create Location**

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details</th>
</tr>
&#10;<tr>
<td rowspan="6">POST</td>
<td>URL</td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/locations</td>
</tr>
<tr>
<td>Parameter</td>
<td> <del>accountName (from 1st  get account call response)</del>  syncWithGmb=true/false  (default true)</td>
</tr>
<tr>
<td>content Type </td>
<td>application/json</td>
</tr>
<tr>
<td><span class="inline-comment-marker" data-ref="a8fdaac9-0811-4acf-acfe-57cc2fcff5b8">Request Payload</span></td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="2126717c-c4a5-488f-8998-9070d2e903ae" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;workplaceId&quot;: &quot;&quot;,
    &quot;obcBanner&quot;:&quot;https://www.talkwalker.com/images/2020/blog-headers/image-analysis.png&quot;,
    &quot;logo&quot;:&quot;http://logok.org/wp-content/uploads/2014/05/Total-logo-earth-880x660.png&quot;
    &quot;languageCode&quot;: &quot;en&quot;,
    &quot;name&quot;: &quot;Soft Testing Solution&quot;,
    &quot;phone&quot;: &quot;9988199881&quot;,
    &quot;description&quot;: &quot;string&quot;,
    &quot;address&quot;: {
        &quot;regionCode&quot;: &quot;IN&quot;,
        &quot;postalCode&quot;: &quot;390007&quot;,
        &quot;administrativeArea&quot;: &quot;Gujarat&quot;,
        &quot;locality&quot;: &quot;Vadodara&quot;,
        &quot;addressLines&quot;: [
            &quot;vasna&quot;
        ]
    },
    &quot;categoryId&quot;: &quot;gcid:computer_software_store&quot;,
    &quot;websiteUrl&quot;: &quot;http://www.avdevs.com/&quot;,
    &quot;periods&quot;:{ [
           {
            &quot;openDay&quot;: &quot;MONDAY&quot;,
            &quot;openTime&quot;: &quot;09:00&quot;,
            &quot;closeDay&quot;: &quot;MONDAY&quot;,
            &quot;closeTime&quot;: &quot;17:00&quot;
          },
          {
            &quot;openDay&quot;: &quot;TUESDAY&quot;,
            &quot;openTime&quot;: &quot;09:00&quot;,
            &quot;closeDay&quot;: &quot;TUESDAY&quot;,
            &quot;closeTime&quot;: &quot;17:00&quot;
          } ....
        ]
    },
    &quot;specialPeriods&quot;: {[
            {
                &quot;startDate&quot;: &quot;YYYY/MM/DD&quot;,
                &quot;openTime&quot;: &quot;HH:MM&quot;,
                &quot;endDate&quot;: &quot;YYYY/MM/DD&quot;,
                &quot;closeTime&quot;: &quot;HH:MM&quot;,
                &quot;isClosed&quot;: boolean
            } ...
        ]
    }
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td>Response</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8dc2236d-db78-4864-8f4b-e568c78eae46" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;workplaceId&quot;: &quot;&quot;,
    &quot;locationId&quot;:&quot;&quot;,
    &quot;languageCode&quot;: &quot;en&quot;,
    &quot;name&quot;: &quot;Soft Testing Solution&quot;,
    &quot;phone&quot;: &quot;9988199881&quot;,
    &quot;description&quot;: &quot;string&quot;,
    &quot;address&quot;: {
        &quot;regionCode&quot;: &quot;IN&quot;,
        &quot;postalCode&quot;: &quot;390007&quot;,
        &quot;administrativeArea&quot;: &quot;Gujarat&quot;,
        &quot;locality&quot;: &quot;Vadodara&quot;,
        &quot;addressLines&quot;: [
            &quot;vasna&quot;
        ]
    },
    &quot;categoryId&quot;: &quot;gcid:computer_software_store&quot;,
    &quot;websiteUrl&quot;: &quot;http://www.avdevs.com/&quot;,
    &quot;periods&quot;: { [
           {
            &quot;openDay&quot;: &quot;MONDAY&quot;,
            &quot;openTime&quot;: &quot;09:00&quot;,
            &quot;closeDay&quot;: &quot;MONDAY&quot;,
            &quot;closeTime&quot;: &quot;17:00&quot;
          },
          {
            &quot;openDay&quot;: &quot;TUESDAY&quot;,
            &quot;openTime&quot;: &quot;09:00&quot;,
            &quot;closeDay&quot;: &quot;TUESDAY&quot;,
            &quot;closeTime&quot;: &quot;17:00&quot;
          } ....
        ]
    },
    &quot;specialPeriods&quot;: { [
            {
                &quot;startDate&quot;: &quot;YYYY/MM/DD&quot;,
                &quot;openTime&quot;: &quot;HH:MM&quot;,
                &quot;endDate&quot;: &quot;YYYY/MM/DD&quot;,
                &quot;closeTime&quot;: &quot;HH:MM&quot;,
                &quot;isClosed&quot;: boolean
            } ...
        ]
    },
    &quot;obcBanner&quot;: &quot;https://lh3.googleusercontent.com/p/AF1QipMuGDk6poXe7Y0is0kWd5pbTYNmI45YdaHZlT5O=s300&quot;,
    &quot;logo&quot;: &quot;https://lh3.googleusercontent.com/p/AF1QipOED5asGXqpzg55OL3ki0TVnbozMCkdR6XWkUvX=s300&quot;,
    &quot;linked&quot; : true,
    &quot;verified&quot;: false,
    &quot;reviewLink&quot; : &quot;https://www.google.com.vn/imgres&quot;,
    &quot;mapsUrl&quot; : &quot;https://www.google.com.vn/imgres&quot;
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td>Errors</td>
<td><span class="inline-comment-marker" data-ref="bc979d20-ffa4-4657-81a8-e2e735c748fe">if </span><span><span><span class="inline-comment-marker" data-ref="bc979d20-ffa4-4657-81a8-e2e735c748fe">Google returns an error message, that they need latitude and longitude while creating location then API will return following error message is:</span> <br />
</span></span>
<p><span>"message"</span><span class="legacy-color-text-default">: </span><span>"The specified address cannot be located. Please provide a latlng value." <br />
</span></p>
<p><span><span><span class="inline-comment-marker" data-ref="8eb94fd6-817e-47a2-978c-1b45e073a510">you need to add in request payload latlng field like: </span><br />
<span class="inline-comment-marker" data-ref="8eb94fd6-817e-47a2-978c-1b45e073a510">"latlng": {</span><br />
<span class="inline-comment-marker" data-ref="8eb94fd6-817e-47a2-978c-1b45e073a510">      "latitude": 22.2948079,</span><br />
<span class="inline-comment-marker" data-ref="8eb94fd6-817e-47a2-978c-1b45e073a510">      "longitude": 73.1522742</span><br />
<span class="inline-comment-marker" data-ref="8eb94fd6-817e-47a2-978c-1b45e073a510">}</span></span></span></p></td>
</tr>
</tbody>
</table>

</div>

### **4. Update Location**

<div>

<table style="width: 99.9127%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details</th>
</tr>
&#10;<tr>
<td rowspan="5">PUT</td>
<td>URL</td>
<td><span class="inline-comment-marker" data-ref="106b1fe5-1ba0-4462-af3e-0c50e01b6ee0">luz_google/api/{company-tenant-id}/companies/{companyId}/locations/{locationId}</span></td>
</tr>
<tr>
<td>Content Type</td>
<td>appication/json</td>
</tr>
<tr>
<td>Parameter</td>
<td>syncWithGmb=true/false  (default true)</td>
</tr>
<tr>
<td><span class="inline-comment-marker" data-ref="406645d4-efbe-4233-8c1a-1f56b2698ff5">Request</span></td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c5e74aa2-3ed9-44c6-ae75-61b35f54d657" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{ 
    &quot;workplaceId&quot;: &quot;&quot;,
    only those fields which are requried to update in payload  
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td>Response</td>
<td>location details same as create</td>
</tr>
</tbody>
</table>

</div>

### 5. Verification Location Status

<div>

<table style="width: 63.289%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details(only Linked location with GMB and Klara)</th>
</tr>
&#10;<tr>
<td>GET</td>
<td>URL</td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/workplaces/{workplaceId}/verifications</td>
</tr>
<tr>
<td><br />
</td>
<td>Response</td>
<td><div class="content-wrapper">
<p><br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="00d197df-c919-4399-8af8-feebcc5485bd" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>State</strong>
</div>
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;methods&quot;: [
        &quot;ADDRESS&quot; or &quot;PHONE_CALL&quot; or &quot;EMAIL&quot; or &quot;SMS&quot; or &quot;AUTO&quot; or &quot;VETTED_PARTNER&quot;
    ],
    &quot;state&quot;: &quot;PENDING&quot; or &quot;COMPLETED&quot; or &quot;FAILED&quot;
}</code></pre>
</div>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

### 6. <span class="inline-comment-marker" ref="966077e4-fb6b-4eae-95a0-d289dddfe0ff">Auto Verification Location</span>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details(only Linked location with GMB and Klara)</th>
</tr>
&#10;<tr>
<td>POST</td>
<td>URL</td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/locations/{locationId}/auto-verify</td>
</tr>
<tr>
<td><br />
</td>
<td>Parameter</td>
<td>accessToken (only use for testing with Klara verification account) (Default value is empty string)</td>
</tr>
<tr>
<td><br />
</td>
<td>Response</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b9135519-f8c6-432e-b96b-f1396fe0eea5" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>Response</strong>
</div>
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;verification&quot;: {
    &quot;name&quot;: &quot;accounts/100752911589447829838/locations/402067894617032078/verifications/8T1593514884866&quot;,
    &quot;method&quot;: &quot;VETTED_PARTNER&quot;,
    &quot;state&quot;: &quot;COMPLETED&quot;,
    &quot;createTime&quot;: &quot;2020-06-30T11:01:24.866Z&quot;
  }
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td colspan="2">API Details:</td>
<td>Store <strong>refresh token</strong> from Klara verification account in System Property in wildfly server. System property key name is <strong><span>ch_klara_luz_google_admin_refresh_token.</span></strong> </td>
</tr>
</tbody>
</table>

</div>

### **7. Disconnect GMB Account**

<div>

<table style="width: 63.5217%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<th><br />
</th>
<th>Details</th>
</tr>
&#10;<tr>
<td rowspan="2">DELETE</td>
<td>URL</td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/<del><span class="inline-comment-marker" data-ref="3415d866-daf5-4f8f-8e0b-bad5e295f715">disconnect</span></del></td>
</tr>
<tr>
<td>Response</td>
<td>200 empty</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[News, Event and Deal API]]
- [[Getting tenant list]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[14. Create companies by tenant id]]
- [[API in Community Feature for Business Tenant]]

%% ai-graph-end %%