---
ai_hash: 5b844f27eb5ad33a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 16
depth: 3
entities: []
relevance: 0.858
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47494824249/Public+API+client+performance+analysis
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Public API client performance analysis
topic: programming
type: source
updated: 2023-10-20
---

# Public API client performance analysis

> [!info] Imported from Confluence
> Space **FUT** · updated 2023-10-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47494824249/Public+API+client+performance+analysis)
> Relevance 0.858 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Priority</strong></p></th>
<th><p><strong>API</strong></p></th>
<th><p><strong>Frequency</strong></p></th>
<th><p><strong>Analysis</strong></p></th>
<th><p><strong>Status</strong></p></th>
</tr>
&#10;<tr>
<td></td>
<td><p>luz-public-api-adapter</p>
<p>PUT</p>
<p><code>/core/latest/print-partners/document-status</code></p></td>
<td><p>occasionally</p>

![[47494824249-image-20230921-065955.png]]

</td>
<td>

![[47494824249-image-20230920-101724.png]]


<p>This API is calling 2 other APIs,<br />
1 from luz-doc-output-mgmt (<code>PUT /luz-doc-output-mgmt/api/print-provider/tracking-status</code>),<br />
1 from luz-eletter (<code>PUT /luz_eletter/api/third-party/a35af857-054e-4b0b-99f0-c46be67fd8c4/document-status</code>)</p>
<p>And the API from luz_eletter is taking most of the time.<br />
The API from luz-eletter is performing 2 update queries, 1 query for a tenant schema, 1 for the public schema.</p>
<p><span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="b2bed7c5-6186-4968-9e15-08c2e719f46b" data-macro-name="view-file"><a href="../_attachments/47494824249-update-document-status.sql" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47494824249/update-document-status.sql?version=1&amp;modificationDate=1695205352531&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[47494824249-update-document-status.sql]]

</a></span></p>
<p>And maybe because the data in both of these tables are too large, the queries are slow to execute,<br />
because both queries are doing a sequential scan of the tables before update the data.</p>
<p>Indexing the columns that are used in the where statement could be 1 solution.</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>luz-public-api-adapter</p>
<p><code>GET</code><br />
<code>/core/latest/articles/article-and-variants?include-quantity=true&amp;limit=20&amp;offset=0&amp;sell-in-booking=false&amp;sell-in-online-shop=false&amp;use-pos=true</code></p></td>
<td></td>
<td><p>Log filter</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7d346646-6349-489f-a831-c50f02c71892" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>resource.type=&quot;k8s_container&quot;
resource.labels.project_id=&quot;klara-prod&quot;
resource.labels.location=&quot;europe-west6-a&quot;
resource.labels.cluster_name=&quot;klara-prod&quot;
resource.labels.namespace_name=&quot;prod&quot;
labels.k8s-pod/app=(&quot;luz-public-api-adapter&quot; OR &quot;luz-article&quot;)
&quot;article-and-variants&quot;
OR &quot;articles/online-shop-in-public&quot;</code></pre>
</div>
</div>
<p><span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="656cb3a7-a25c-4f86-a497-86939544b475" data-macro-name="view-file"><a href="../_attachments/47494824249-downloaded-logs-20230921-094209.csv" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47494824249/downloaded-logs-20230921-094209.csv?version=1&amp;modificationDate=1695264187746&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/csv" data-has-thumbnail="true">

![[47494824249-downloaded-logs-20230921-094209.csv]]

</a></span></p>
<p>The API /luz_article/api/{tenant-id}/companies/1/sale/articles/online-shop-in-public?sell-in-online-shop=false&amp;include-quantity=true&amp;sell-in-booking=false&amp;use-pos=true from luz-article is being called behind the scene, and is taking most of the time.</p>
<p>And that API is doing the following queries:</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>luz-public-api-adapter</p>
<p><code>/core/latest/token</code></p></td>
<td><p>~30%-50% request take &gt;1s</p></td>
<td><p>This API call 1 API in jwt-service. Consequence, jwt-service call 3 others APIs</p>
<ol>
<li><p>Generate token via keycloak (<code>https://login-dev.klara.tech/auth/realms/klara/protocol/openid-connect/token)</code><br />
<strong>Problem:</strong></p>
<p>Time consuming for the API 1 (item 1) is ok in prod. However, there is one problem that is sometimes it could not connect to keycloak. At this time the keycloak was not in high load. Then, we think that it was causing by the issue that for some reasons it could not create Http client and the time out for this process take more than 2 mins to timeout.<br />
<a href="https://console.cloud.google.com/logs/query;query=--%20resource.type:%2528http_load_balancer%2529%20AND%20jsonPayload.enforcedSecurityPolicy.name:%2528prod-security-policy%2529%0A--%20httpRequest.latency%20%3C%20500ms%0A--%20httpRequest.latency%20%3E%20100s%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20httpRequest.latency%20%3E%20501ms%20AND%20httpRequest.latency%20%3C%201000ms%0A--%20%2528textPayload%3D~%22%2Fpublic-api-tokens%22%20AND%20textPayload%3D~%22time-consuming%3D%5Cd%7B4,%7D%22%20AND%20textPayload%3D~%22status-code%3D400%22%2529%0A%2528textPayload%3D~%22Could%20not%20get%20token%20from%20keycloak%22%20OR%20textPayload%3D~%22Code%20is%20broken%22%2529%0A%0A--%20httpRequest.requestUrl:%2528%22api.klara.ch%22%20OR%20%22api.epost.ch%22%2529%20--%20public%20api;pinnedLogId=2023-09-13T09:27:12.934404411Z%2Fpvy6na297ahhfp9z;lfeCustomFields=;cursorTimestamp=2023-09-13T09:27:12.932886387Z;startTime=2023-09-13T09:24:00.000Z;endTime=2023-09-13T10:27:00.000Z?project=klara-prod" class="external-link" rel="nofollow">Error log</a></p>

![[47494824249-image-20230921-083701.png]]


<p><strong>Solution:</strong></p>
<ul>
<li><p>Change to use MP-RestClient</p></li>
<li><p>Set suitable connect timeout and read timeout</p></li>
</ul></li>
<li><p>Get roles of ivy user via webclient (<code>luz/api/users/&lt;ivy_user&gt;/&lt;tenant_id&gt;</code> )<br />
<strong>Problem:</strong> It should not call to web-client<br />
<strong>Solution:</strong> access into db of ivy directly.</p></li>
<li><p>Get all tenants of user (<code>/luztenant/api/martin.grundbacher@abraxas.ch/all-tenants</code>)<br />
Most of the time consuming in PROD by this API. As you can see in the frequency it’s quite often. This problem seems to be caused by several reasons below<br />
<strong>Problem:</strong></p></li>
</ol>
<ul>
<li><p>N+1 query due to user and roles query<br />
<strong>Note: This is general issue mentioned in login flow</strong></p></li>
<li><p>Redundant or not optimal behavior as below<br />
<strong>Solution:</strong><br />
These calls can be merge to one</p>

![[47494824249-image-20230922-111329.png]]

![[47494824249-image-20230925-071247.png]]

</li>
</ul>
<p>Reference: <a href="https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47494365263/Get+tenant+token+from+public+api">Get tenant token from public api</a></p></td>
<td><p>READY</p></td>
</tr>
<tr>
<td></td>
<td><p>luz-compensation</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="db8c7259-5be2-4fc6-8672-2147efe6d062" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>/luz_compensation/api/{company-tenant-id}/companies/{companyId}/barcode-print/file/export/hrmdat</code></pre>
</div>
</div></td>
<td></td>
<td><p>This API is currently calling</p>
<ol>
<li><p>APIs to get tokens. These APIs have performance issues. However, it should be fixed in other tickets</p></li>
<li><p>API to get barcode-print <code>/luz_compensation/api/{company-tenant-id}/companies/{companyId}/barcode-print</code>. The most contribution part of this API is the query to get payslips. This query has 3 issues as below</p>

![[47494824249-image-20231012-074240.png]]


<ol>
<li><p>This query is trying to get huge data and also trying to distinct/make them unique and it contribute about 69% of consuming time.</p>

![[47494824249-image-20231017-074812.png]]


<p>However, it’s possible to change the query to make it a lot faster</p>
<ol>
<li><p>Remove distinct from the query</p>

![[47494824249-image-20231018-104517.png]]

![[47494824249-image-20231018-104736.png]]

</li>
<li><p>Make sure the output is not duplicated by filter out the payslip ids already existed</p>

![[47494824249-image-20231018-104618.png]]

</li>
</ol></li>
<li><p>As manipulate (CRUD) data through Hibernate it’s required to flush information from persistent context to DB it also takes time. As you can see in screenshots below the demonstration is only to get barcode for 5 employees this process also took quite a lot of time. Then, we suggest to consider/think again about to JUST get enough information<br />
</p>

![[47494824249-image-20231019-082544.png]]

![[47494824249-image-20231020-045505.png]]

</li>
<li><p>The sub query/join is doing on an attribute that doesn’t has index</p>

![[47494824249-image-20231018-105403.png]]

![[47494824249-image-20231018-105458.png]]


<p><br />
</p></li>
</ol></li>
</ol>
<p>Reference:</p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47512716203/Compensation#%2Fluz_compensation%2Fapi%2F%7Bcompany-tenant-id%7D%2Fcompanies%2F%7BcompanyId%7D%2Fbarcode-print%2Ffile%2Fexport%2Fhrmdat-out%3Fis-generate-all-tenants%3Dtrue%26isSkippingMigration%3Dtrue%26user-name%3Dklarasync%2540akris.com" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47512716203/Compensation#%2Fluz_compensation%2Fapi%2F%7Bcompany-tenant-id%7D%2Fcompanies%2F%7BcompanyId%7D%2Fbarcode-print%2Ffile%2Fexport%2Fhrmdat-out%3Fis-generate-all-tenants%3Dtrue%26isSkippingMigration%3Dtrue%26user-name%3Dklarasync%2540akris.com</a></p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Helios myKLARA app(luz-mobile) - API Response Performance Analysis]]
- [[Measure API luz-docs]]
- [[Optimus ePost myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis]]
- [[eArchive – Reproduce performance issue and understand the issue on DEV]]
- [[Analytics Analyze API call when accessing eArchive]]

%% ai-graph-end %%