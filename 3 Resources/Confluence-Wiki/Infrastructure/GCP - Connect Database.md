---
ai_hash: 07005575c776a56e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.53
entities: []
relevance: 0.734
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38225223414/GCP+-+Connect+Database
space: Helios
status: reference
tags:
- confluence
- infra
- space/helios
title: GCP - Connect Database
topic: infra
type: source
updated: 2026-07-22
---

# GCP - Connect Database

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-07-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38225223414/GCP+-+Connect+Database)
> Relevance 0.734 · topic `infra`

Copy from <a href="https://jira.axonivy.com/confluence/pages/resumedraft.action?draftId=259534843&amp;draftShareId=b49fcec4-7b0b-4ac7-8433-7318b630bb73&amp;" class="external-link" rel="nofollow">https://jira.axonivy.com/confluence/pages/resumedraft.action?draftId=259534843&amp;draftShareId=b49fcec4-7b0b-4ac7-8433-7318b630bb73&amp;</a>  
  

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="60d78014-fe44-4cea-a414-5ef90e33411f" macro-name="toc">

</div>

# Docker

# Google cloud

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>No</strong></p></th>
<th><p><strong>Tip</strong></p></th>
<th><p><strong>How to</strong></p></th>
</tr>
<tr>
<th></th>
<th colspan="2"><h2 id="GCP-ConnectDatabase-Login" data-local-id="5948290c57ad"><strong>Login</strong></h2></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="GCP-ConnectDatabase-LogintoGCP" data-local-id="385769df025a">Login to GCP</h3></td>
<td><ol>
<li><p>Register your own google account as company email: <a href="https://accounts.google.com/signup/v2/webcreateaccount?hl=en&amp;flowName=GlifWebSignIn&amp;flowEntry=SignUp" class="external-link" rel="nofollow">https://accounts.google.com/signup/v2/webcreateaccount?hl=en&amp;flowName=GlifWebSignIn&amp;flowEntry=SignUp</a></p></li>
</ol>

![[38225223414-image2020-12-3_9-30-6.png]]


<p>2. Ask Future team to update the LDAP email list</p>
<p>3. Then login as following command: </p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="0b08a575-ff79-4818-ab3d-9e581d308252" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>gcloud auth login</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>2</p></td>
<td><h3 id="GCP-ConnectDatabase-LogintoGCPKubernetes" data-local-id="50281e29679e">Login to GCP Kubernetes</h3></td>
<td><p>Get the command to login from GCP <a href="https://console.cloud.google.com/kubernetes/" class="external-link" rel="nofollow">https://console.cloud.google.com/kubernetes/</a></p>

![[38225223414-image2020-10-5_8-58-2.png]]

![[38225223414-image2020-10-5_9-1-18.png]]


<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8af5fec1-fb9f-4577-a484-9c2a085c8a11" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>gcloud container clusters get-credentials klara-nonprod --zone europe-west6-a --project klara-nonprod</code></pre>
</div>
</div></td>
</tr>
<tr>
<td></td>
<td colspan="2"><h2 id="GCP-ConnectDatabase-Pods" data-local-id="a1d75f843054"><strong>Pods</strong></h2></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="GCP-ConnectDatabase-Getrunningpodslist" data-local-id="dcaddbfe0b31">Get running pods list</h3></td>
<td>

![[38225223414-image2020-9-10_13-46-51.png]]


<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="437f4ed6-6bfa-431b-a642-5377c93698d7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>kubectl get pods -n dev</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>4</p></td>
<td><h3 id="GCP-ConnectDatabase-Viewlogfromrunningpod" data-local-id="d811f3b0f6c3">View log from running pod</h3></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b38a0fda-4c9c-497f-8000-40c2bf67d4d3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>kubectl logs -f --tail=5000 service/{service name | luz-accounting} -n dev</code></pre>
</div>
</div></td>
</tr>
<tr>
<td></td>
<td colspan="2"><h2 id="GCP-ConnectDatabase-Portforward" data-local-id="8a9a0b3e1000"><strong>Port forward</strong></h2></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="GCP-ConnectDatabase-Connectdatabase" data-local-id="943118ef4e9a">Connect database</h3></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e1cc0d7d-d41f-4c39-a6ce-d41c9ce104a2" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>kubectl port-forward service/luz-database 5556:5432 -n dev</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="000c6cb4-2051-47d5-971e-98b5af47b9b3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>kubectl port-forward service/luz-alloydb-main 5556:5432 -n dev</code></pre>
</div>
</div>
<p>or</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="dc6aa0b2-4038-4a9b-9966-a15785b97a14" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>kubectl port-forward &lt;pod&#39;s name | luz-database-68dbdf75f7-xhf86&gt; 5433:5432 -n dev</code></pre>
</div>
</div>
<p>Then open pgAdmin to access in localhost port 5433. </p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="GCP-ConnectDatabase-GetTenanttokenofaspecifictenant" data-local-id="e5e05bd69e2e">Get Tenant token of a specific tenant</h3></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="58e0e3cb-1a77-4d05-80fa-66453954157a" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>kubectl port-forward service/jwt-service 8888:8080 -n dev</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="884fa2aa-6570-4458-a702-317024ee2409" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>export HOST_PORT=&#39;localhost:8888&#39;; \
echo $HOST_PORT; \
&#10;export USER_NAME=&#39;admin&#39;; \
echo $USER_NAME; \
&#10;export PASSWORD=&#39;admin&#39;; \
echo $PASSWORD; \
&#10;
export GENERRIC_TOKEN=$(curl -X POST -s -H &#39;Authorization: Basic &#39;$(echo -n $USER_NAME:$PASSWORD | base64) $HOST_PORT&#39;/luzsec/api/tokens&#39; | sed &#39;s/.*token...\(.*\)...publicKey.*/\1/&#39;); \
  echo $GENERRIC_TOKEN; \
curl -X GET --header &#39;Accept: application/json&#39; --header &#39;Authorization: Bearer &#39;$GENERRIC_TOKEN &#39;http://&#39;$HOST_PORT&#39;/luztenant/api/&#39;$USER_NAME&#39;/tenants&#39;; \
&#10;export TENANT_ID=&quot;388c822c-7860-41ae-94ac-330684bb63e0&quot;; \
export TENANT_TOKEN=$(curl -X POST -s -H &#39;Authorization: Basic &#39;$(echo -n $USER_NAME:$PASSWORD | base64) &#39;http://&#39;$HOST_PORT&#39;/luzsec/api/&#39;$TENANT_ID&#39;/access/tokens&#39; | sed &#39;s/.*token...\(.*\)...publicKey.*/\1/&#39;); \
  echo $TENANT_TOKEN;</code></pre>
</div>
</div></td>
</tr>
<tr>
<td></td>
<td colspan="2"><h2 id="GCP-ConnectDatabase-Logging" data-local-id="5aa63f67af9e"><strong>Logging</strong></h2></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="GCP-ConnectDatabase-Advancedlogsqueries" data-local-id="d36d2e4a9def">Advanced logs queries</h3></td>
<td><p><a href="https://cloud.google.com/logging/docs/view/advanced-queries" class="external-link" rel="nofollow">https://cloud.google.com/logging/docs/view/advanced-queries</a></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="2972cc7a-794a-4650-9ea2-a1ec0e89eda4" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>a = e means that a is a name for the expression e.
a b means &quot;a followed by b.&quot;
a | b means &quot;a or b.&quot;
( e ) is used for grouping.
[ e ] means that e is optional.
{ e } means that e can be repeated zero or more times.
&quot;abc&quot; means that abc must be written just as it appears.</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="03faa786-c98c-4104-baba-077f6ff643c8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>=           # equal
!=          # not equal
&gt; &lt; &gt;= &lt;=   # numeric ordering
:           # &quot;has&quot; matches any substring in the log entry field
=~          # regular expression search for a pattern
!~          # regular expression search not for a pattern</code></pre>
</div>
</div>
<p>Here is the example for query logs from Ivy pod with contain the text "1d16ba9f-65ec-44d8-a9e5-6c96c8b0be4f"</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="81c9ffe4-b297-46eb-911c-7b61ae5347e6" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>resource.type=&quot;k8s_container&quot;
resource.labels.project_id=&quot;klara-nonprod&quot;
resource.labels.location=&quot;europe-west6-a&quot;
resource.labels.cluster_name=&quot;klara-nonprod&quot;
resource.labels.namespace_name=&quot;dev&quot;
labels.k8s-pod/app=(&quot;luz-webclient&quot; OR &quot;webclient-nginx-ingress&quot;) AND textPayload:&quot;1d16ba9f-65ec-44d8-a9e5-6c96c8b0be4f&quot;</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Port Forward to call GCP API in localhost]]
- [[Deploy luz-epc-redis-service on GCP]]
- [[Deploy to Kubernetes and get External IP]]
- [[How to connect K8S database from postgres]]
- [[Infrastructure]]

%% ai-graph-end %%