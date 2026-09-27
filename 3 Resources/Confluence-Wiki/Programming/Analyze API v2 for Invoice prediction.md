---
ai_hash: 248646c1c85158fb
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.33
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47487714001/Analyze+API+v2+for+Invoice+prediction
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Analyze API v2 for Invoice prediction
topic: programming
type: source
updated: 2023-09-18
---

# Analyze API v2 for Invoice prediction

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-09-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47487714001/Analyze+API+v2+for+Invoice+prediction)
> Relevance 0.711 · topic `programming`

Since Invoice API is going to be shutdown, we are going to use Analyze API v2 for AI Prediction.

[Analyze API v2.0](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47401730164/Analyze+API+v2.0)

## Analyze process


![[47487714001-Analyze API v2 prediction flow.png]]



<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Step</strong></p></th>
<th><p><strong>Detail</strong></p></th>
</tr>
&#10;<tr>
<td><p>1 - 2</p></td>
<td><p>Perform analyze document</p>
<ol>
<li><p>Submit a job to Analyze with:<br />
</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="09eb2e72-594d-4cfc-a142-9c82222753c4" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara&#39; \
--header &#39;Content-Type: multipart/form-data&#39; \
--header &#39;Accept: application/json&#39; \
--form &#39;Sofa=@&quot;/C:/Users/all/Downloads/scan_01.pdf&quot;&#39; \
--form &#39;task=&quot;Document.Discover&quot;;type=text/plain&#39; \
--form &#39;Document.Type=@&quot;/C:/Users/all/Downloads/invoice.json&quot;&#39; \
--form &#39;priority=&quot;URGENT&quot;&#39; \
--form &#39;config=&quot;{
    \&quot;predictKlaraAccgBct\&quot;: true,
    \&quot;predictKlaraAccgTag\&quot;:true,
    \&quot;acceptLanguage\&quot;: \&quot;en\&quot;
}&quot;&#39;</code></pre>
</div>
</div></li>
<li><p>Analyze API Response:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e33eed91-f3da-4985-afae-07f3f5dc96b1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;status&quot;: {
        &quot;code&quot;: 202,
        &quot;reason&quot;: &quot;Accepted&quot;,
        &quot;family&quot;: &quot;SUCCESSFUL&quot;
    },
    &quot;data&quot;: &quot;34313&quot;
}</code></pre>
</div>
</div></li>
</ol></td>
</tr>
<tr>
<td><p>3 - 4</p></td>
<td><p>After submit a job, using Job Id to get Job status</p>
<p>Loop calling get Job Status until Job Done/Failed or exceed number of retry.</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8b19c385-6418-4349-a47c-9e3cc1a6b323" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}&#39; \
--header &#39;Accept: application/json&#39;</code></pre>
</div>
</div>
<p>Response:</p>
<p>IF Job Status is CANCELLED or DONE or FAILED then stop calling get job status<br />
ELSE loop calling to get Job status after a certain time until exceed 20 times</p></td>
</tr>
<tr>
<td><p>5 - 6</p></td>
<td><p>If job Status is DONE or FAILED, then call api to get Job Result</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="17008ac4-6244-49c1-9d0c-b51fec485092" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}/result?timeout=0&#39; \
--header &#39;Accept: multipart/form-data&#39;</code></pre>
</div>
</div>
<p>API response example:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8d7fa307-7033-4b46-8f7b-3a1813ed7c7f" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;status&quot;: {
        &quot;code&quot;: 200,
        &quot;reason&quot;: &quot;OK&quot;,
        &quot;family&quot;: &quot;SUCCESSFUL&quot;
    },
    &quot;data&quot;: {
        &quot;id&quot;: 34313,
        &quot;priority&quot;: &quot;URGENT&quot;,
        &quot;status&quot;: &quot;DONE&quot;,
        &quot;entities&quot;: {
            &quot;Analyze.FREngine.Xml&quot;: {
                &quot;scope&quot;: &quot;GENERATED&quot;
            },
            &quot;Document.SearchablePdf&quot;: {
                &quot;scope&quot;: &quot;GENERATED&quot;
            },
            &quot;Invoice.Predictions&quot;: {
                &quot;scope&quot;: &quot;GENERATED&quot;
            },
            &quot;Invoice.AccgBct&quot;: {
                &quot;scope&quot;: &quot;GENERATED&quot;
            },
            &quot;Invoice.AccgTag&quot;: {
                &quot;scope&quot;: &quot;GENERATED&quot;
            }
        }
    }
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>7 - 10</p></td>
<td><p>To get analyze data, call below apis to get each entity<br />
<br />
Call api to get OCR results:</p>
<ol>
<li><p>Analyze.FREngine.Xml entity</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="6cc6220c-122d-4830-a8cd-0c2d8ce55f01" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}/entity/Analyze.FREngine.Xml&#39; \
--header &#39;Accept: application/octet-stream&#39;</code></pre>
</div>
</div></li>
</ol>
<ol start="2">
<li><p>Document.SearchablePdf entity</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="006d21a0-0905-495a-82b4-f60ca5513964" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}/entity/Document.SearchablePdf&#39; \
--header &#39;Accept: application/octet-stream&#39;</code></pre>
</div>
</div></li>
</ol></td>
</tr>
<tr>
<td><p>11 - 12</p></td>
<td><p>Get Invoice.Prediction Entity</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b84d786c-a0fc-49f9-bbbb-daa5446a8c8a" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}/entity/Invoice.Predictions&#39; \
--header &#39;Accept: application/octet-stream&#39;</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>13 - 16</p></td>
<td><p>Get InvoiceAccgBct and InvoiceAccgTag</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ba89a95d-2423-4827-8279-6a78f8c9066d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}/entity/Invoice.AccgBct&#39; \
--header &#39;Accept: application/octet-stream&#39;</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="bebb2206-28a8-44f4-ad46-28d656ef8fd6" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}/entity/Invoice.AccgTag&#39; \
--header &#39;Accept: application/octet-stream&#39;</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>17</p></td>
<td><p>After get all needed job entity then delete the Job</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e3cb4c2d-2fa4-4b5b-bdb1-f497075100d7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>curl --location --request DELETE &#39;http://192.168.1.18/api/v2/jobs/klara/{{JobId}}&#39; \
--header &#39;Accept: application/json&#39;</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Invoice API]]
- [[Invoice API Reference]]
- [[Analyze API]]
- [[Analyze API Demo]]
- [[Analyze API v2.0]]

%% ai-graph-end %%