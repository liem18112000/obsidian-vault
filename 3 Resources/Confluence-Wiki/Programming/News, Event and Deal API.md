---
title: "News, Event and Deal API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508017980/News+Event+and+Deal+API
space: "LUZ"
topic: programming
relevance: 0.786
depth: 3
updated: 2020-06-18
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# News, Event and Deal API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-06-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508017980/News+Event+and+Deal+API)
> Relevance 0.786 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="f204a90b-4930-4397-89f6-87e887df9712" macro-name="toc">

</div>

### <span class="inline-comment-marker" ref="30ff6501-9cfb-4ca7-9e9f-0f40fb9104dd">1. Create Local Post (News/Event/Deal)</span>

<div>

<table style="width: 94.5899%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><span class="legacy-color-text-blue3">Method</span></th>
<th><br />
</th>
<th><span class="legacy-color-text-blue3">Details</span></th>
</tr>
&#10;<tr>
<td>POST</td>
<td><span class="legacy-color-text-blue3">URL</span></td>
<td>luz_google/api/{company-tenant-id}/companies/{companyId}/localpost</td>
</tr>
<tr>
<td><br />
</td>
<td>Content Type</td>
<td><span class="inline-comment-marker" data-ref="9d35c289-0b03-4f52-a14d-1ff0a3a31832">application/json</span></td>
</tr>
<tr>
<td><br />
</td>
<td><span class="inline-comment-marker" data-ref="e87bb936-d8b3-480c-b325-7805e2fd1606">Request</span></td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="81291798-b030-4b29-b85d-cd05975ec962" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;workplaceId&quot;: &quot;&quot;,
  &quot;details&quot;: [
          {
               &quot;languageCode&quot;: &quot;DE&quot;,
               &quot;title&quot;: &quot;title in german&quot;,
               &quot;leadText&quot;: &quot;leadText in german&quot;,
               &quot;content&quot;: &quot;content in german&quot;,
          },
          {
               &quot;languageCode&quot;: &quot;EN&quot;,
               &quot;title&quot;: &quot;title in english&quot;,
               &quot;leadText&quot;: &quot;leadText in english&quot;,
               &quot;content&quot;: &quot;content in english&quot;
          },
          {
               &quot;languageCode&quot;: &quot;FR&quot;,
               &quot;title&quot;: &quot;title in french&quot;,
               &quot;leadText&quot;: &quot;leadText in french&quot;,
               &quot;content&quot;: &quot;content in french&quot;
          },
          {
               &quot;languageCode&quot;: &quot;IT&quot;,
               &quot;title&quot;: &quot;title in italian&quot;,
               &quot;leadText&quot;: &quot;leadText in italian&quot;,
               &quot;content&quot;: &quot;content in italian&quot;
          }
   ],
  &quot;eventPeriods&quot;: [
    {
      &quot;id&quot; :1,
      &quot;startDate&quot;: &quot;2020/12/23&quot;,
      &quot;endDate&quot;: &quot;2020/12/23&quot;,
      &quot;startTime&quot;: &quot;12:00:00&quot;,
      &quot;endTime&quot;: &quot;15:00:00&quot;
    },
    {
      &quot;id&quot;:2,
      &quot;startDate&quot;: &quot;2020/12/23&quot;,
      &quot;endDate&quot;: &quot;2020/12/23&quot;,
      &quot;startTime&quot;: &quot;12:00:00&quot;,
      &quot;endTime&quot;: &quot;15:00:00&quot;
    }
    .
    .
    .
  ],
  &quot;dealCode&quot;: &quot;XYZ&quot;,
  &quot;website&quot;: &quot;https://exmple.com&quot;,
  &quot;imageUrl&quot;: &quot;https://www.familienleben.ch/images/Quiz-Schwyz-Einstiegsbild-600.jpg&quot;,
  &quot;topicType&quot;: &quot;NEWS/EVENT/REGIO_DEAL&quot;
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td><br />
</td>
<td>Response of NEWS</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9c1b5bc1-2819-4562-9db7-6d286b715063" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;postId&quot;: &quot;5217567501100621319&quot;,
  &quot;workplaceId&quot;: &quot;&quot;,
  &quot;details&quot;: [
          {
               &quot;languageCode: &quot;DE&quot;,
               &quot;title&quot;: &quot;title in german&quot;,
               &quot;summary&quot;: &quot;summary in german&quot;,
               &quot;link&quot;: &quot;https://www.google.com.vn/imgres&quot;
          },
          {
               &quot;languageCode&quot;: &quot;EN&quot;,
               &quot;title&quot;: &quot;title in english&quot;,
               &quot;summary&quot;: &quot;summary in english&quot;,
               &quot;link&quot;: &quot;https://www.google.com.vn/imgres&quot;
          } 
          {
               &quot;languageCode: &quot;FR&quot;,
               &quot;title&quot;: &quot;title in french&quot;,
               &quot;summary&quot;: &quot;summary in french&quot;,
               &quot;link&quot;: &quot;https://www.google.com.vn/imgres&quot;
          },
          {
               &quot;languageCode&quot;: &quot;IT&quot;,
               &quot;title&quot;: &quot;title in italian&quot;,
               &quot;summary&quot;: &quot;summary in italian&quot;,
               &quot;link&quot;: &quot;https://www.google.com.vn/imgres&quot;
          }
  ],
  &quot;eventPeriods&quot;: [
    {
      &quot;id&quot;:1,
      &quot;startDate&quot;: &quot;2020/12/23&quot;,
      &quot;endDate&quot;: &quot;2020/12/23&quot;,
      &quot;startTime&quot;: &quot;12:00:00&quot;,
      &quot;endTime&quot;: &quot;15:00:00&quot;
    },
    {
      &quot;id&quot;:2,
      &quot;startDate&quot;: &quot;2020/12/23&quot;,
      &quot;endDate&quot;: &quot;2020/12/23&quot;,
      &quot;startTime&quot;: &quot;12:00:00&quot;,
      &quot;endTime&quot;: &quot;15:00:00&quot;
    }
    .
    .
    .
  ],
  &quot;dealCode&quot;: &quot;XYZ&quot;,
  &quot;website&quot;: &quot;https://exmple.com&quot;,
  &quot;imageUrl&quot;: &quot;https://www.familienleben.ch/images/Quiz-Schwyz-Einstiegsbild-600.jpg&quot;,
  &quot;topicType&quot;: &quot;NEWS&quot;
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td><br />
</td>
<td>Response of EVENT/REGIO_DEAL</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="498bef23-db51-4ec8-b952-e8b4bd5ebba3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;postId&quot;: &quot;5217567501100621319&quot;,
  &quot;workplaceId&quot;: &quot;&quot;,
  &quot;details&quot;: [
          {
               &quot;languageCode: &quot;DE&quot;,
               &quot;title&quot;: &quot;title in german&quot;,
               &quot;summary&quot;: &quot;summary in german&quot;,
          },
          {
               &quot;languageCode&quot;: &quot;EN&quot;,
               &quot;title&quot;: &quot;title in english&quot;,
               &quot;summary&quot;: &quot;summary in english&quot;,
          } 
          {
               &quot;languageCode: &quot;FR&quot;,
               &quot;title&quot;: &quot;title in french&quot;,
               &quot;summary&quot;: &quot;summary in french&quot;,
          },
          {
               &quot;languageCode&quot;: &quot;IT&quot;,
               &quot;title&quot;: &quot;title in italian&quot;,
               &quot;summary&quot;: &quot;summary in italian&quot;,
          }
  ],
  &quot;eventPeriods&quot;: [
    {
      &quot;id&quot;:1,
      &quot;startDate&quot;: &quot;2020/12/23&quot;,
      &quot;endDate&quot;: &quot;2020/12/23&quot;,
      &quot;startTime&quot;: &quot;12:00:00&quot;,
      &quot;endTime&quot;: &quot;15:00:00&quot;,
      &quot;linkDe&quot;: &quot;https://www.google.com.vn/de&quot;,
      &quot;linkEn&quot;: &quot;https://www.google.com.vn/en&quot;,
      &quot;linkFr&quot;: &quot;https://www.google.com.vn/fr&quot;,
      &quot;linkIt&quot;: &quot;https://www.google.com.vn/it&quot;
    },
    {
      &quot;id&quot;:2,
      &quot;startDate&quot;: &quot;2020/12/23&quot;,
      &quot;endDate&quot;: &quot;2020/12/23&quot;,
      &quot;startTime&quot;: &quot;12:00:00&quot;,
      &quot;endTime&quot;: &quot;15:00:00&quot;,
      &quot;linkDe&quot;: &quot;https://www.google.com.vn/de&quot;,
      &quot;linkEn&quot;: &quot;https://www.google.com.vn/en&quot;,
      &quot;linkFr&quot;: &quot;https://www.google.com.vn/fr&quot;,
      &quot;linkIt&quot;: &quot;https://www.google.com.vn/it&quot;
    }
    .
    .
    .
  ],
  &quot;dealCode&quot;: &quot;XYZ&quot;,
  &quot;website&quot;: &quot;https://exmple.com&quot;,
  &quot;imageUrl&quot;: &quot;https://www.familienleben.ch/images/Quiz-Schwyz-Einstiegsbild-600.jpg&quot;,
  &quot;topicType&quot;: &quot;EVENT/REGIO_DEAL&quot;
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td rowspan="3">Required Parameter</td>
<td>News</td>
<td>imageUrl, workplaceId, title, leadText, website, languageCode, topicType=NEWS</td>
</tr>
<tr>
<td>Event</td>
<td>imageUrl, workplaceId, title, leadText,  eventPeriods, website, languageCode, topicType=EVENT</td>
</tr>
<tr>
<td>Deal</td>
<td>imageUrl, workplaceId, title, leadText, eventPeriods, website, languageCode, dealCode, topicType=REGIO_DEAL</td>
</tr>
</tbody>
</table>

</div>

### 2. Update Local Post (News/Event/Deal)

<div>

<table style="width: 94.5694%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><span class="legacy-color-text-blue3">Method</span></th>
<th><br />
</th>
<th><span class="legacy-color-text-blue3">Details</span></th>
</tr>
&#10;<tr>
<td><span class="inline-comment-marker" data-ref="6c52fb67-ff1e-4ce9-ab79-53184db36b74">PUT</span></td>
<td><span class="legacy-color-text-blue3">URL</span></td>
<td><span class="inline-comment-marker" data-ref="21f4412a-2244-45c0-be5c-0111abc8acae">luz_google/api/{company-tenant-id}/companies/{companyId}/localpost/{postId}</span></td>
</tr>
<tr>
<td><br />
</td>
<td>Content Type</td>
<td>application/json</td>
</tr>
<tr>
<td><br />
</td>
<td><span class="inline-comment-marker" data-ref="34e0c048-50a6-4da5-8d0f-04b4f0cde8d3">Request</span></td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="86368b51-7bd5-4b92-9457-c7305b639fd1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  Same as create localpost 
}</code></pre>
</div>
</div>
</div></td>
</tr>
<tr>
<td><br />
</td>
<td>Response</td>
<td>Same as create localpost</td>
</tr>
</tbody>
</table>

</div>

### 3. Delete Local Post (News/Event/Deal)

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><span class="legacy-color-text-blue3">Method</span></th>
<th><br />
</th>
<th><span class="legacy-color-text-blue3">Details</span></th>
</tr>
&#10;<tr>
<td>DELETE</td>
<td><span class="legacy-color-text-blue3">URL</span></td>
<td><span class="inline-comment-marker" data-ref="320e1e63-af64-48f2-9d70-34dd6ad74c90">luz_google/api/{company-tenant-id}/companies/{companyId}/localpost/{postId}</span></td>
</tr>
<tr>
<td><br />
</td>
<td>Content type</td>
<td>json</td>
</tr>
<tr>
<td><br />
</td>
<td>Response</td>
<td>200 Ok Empty </td>
</tr>
</tbody>
</table>

</div>
