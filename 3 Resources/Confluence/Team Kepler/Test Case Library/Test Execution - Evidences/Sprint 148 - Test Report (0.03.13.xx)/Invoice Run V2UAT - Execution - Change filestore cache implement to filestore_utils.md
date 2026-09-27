---
ai_hash: f5541fc2a3d21103
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49075683329'
confluence_path: Team Kepler > Test Case Library > Test Execution / Evidences > Sprint
  148 - Test Report (0.03.13.xx)
created: 2026-01-23
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
- sprint
- testing
title: '[Invoice Run V2][UAT] - Execution - Change filestore cache implement to filestore_utils'
type: source
updated: 2026-01-23
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49075683329/Invoice+Run+V2+UAT+-+Execution+-+Change+filestore+cache+implement+to+filestore_utils
---

# [Invoice Run V2][UAT] - Execution - Change filestore cache implement to filestore_utils

*Confluence source · Team Kepler › Test Case Library › Test Execution / Evidences › Sprint 148 - Test Report (0.03.13.xx) · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49075683329/Invoice+Run+V2+UAT+-+Execution+-+Change+filestore+cache+implement+to+filestore_utils) · updated 2026-01-23*

## Test case Template

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Change filestore cache implement to filestore_utils</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-STORE</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 23 Jan 2026</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT]-Execution-Changefilestorecacheimplementtofilestore_utils-Shortdescription:" data-local-id="b7358e29-104c-46b6-981b-da07aa96a3c6">Short description:</h2>
<p>Currently, **luz-store** encounters issues when scaling to multiple pods while mounting a shared **NFS (Filestore) volume**. The existing Filestore-based caching implementation is not safe in a multi-pod environment and may lead to inconsistent behavior or data corruption.</p>
<p>To enable safe and reliable use of Filestore for caching when running multiple pods, we need to **refactor the current cache implementation in luz-store** to use the **official** `filestore_utils` **library provided by Team Future**. This library is designed to handle concurrency and multi-pod access correctly.</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Pre – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>`luz-store` and all consuming services are deployed, healthy, and connected.</p></li>
<li><p>Invoice Run test data is available.</p></li>
<li><p>Choosing a invoice run v2 with status “Invoices Charging And Pdf Creating“ with quantity is greater than zero</p></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p>**Post – conditions:**</p></td>
</tr>
<tr>
<td><ul>
<li><p>Invoice Run completes without errors.</p></li>
<li><p>No duplicates or missing records; system is stable for the next run</p></li>
<li><p>There is a file in the NFS existing for 1 hours</p></li>
<li><p>There is a key-value pair for document and its absolute path in NFS</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

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
<td colspan="3"><p>**Test Case**</p></td>
<td colspan="4"><p>**Test Execution**</p></td>
</tr>
<tr>
<td rowspan="2"><p>**Step**</p></td>
<td rowspan="2"><p>**Action**</p></td>
<td rowspan="2"><p>**Expected Behavior**</p></td>
<td rowspan="2"><p>**Actual Behavior**</p></td>
<td rowspan="2"><p>**Comment**</p></td>
<td colspan="2"><p>**Results**</p></td>
</tr>
<tr>
<td><p>As expected</p></td>
<td><p>not as expected</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Choosing a invoice run v2 with status “Invoices Charging And Pdf Creating“ with quantity is greater than zero. Click to view details</p></td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/image-20260123-084154.png]]</td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/check.png]]</p></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/1f6ab.png]]</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Click “Send credit card receipts“ to start the process</p></td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/image-20260123-084405.png]]</td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>After a few seconds, watch the Group Status and Invoice State</p></td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/image-20260123-085102.png]]
<p>The status must be “Done”</p>
<p>The invoice state must be “Uploaded customer document“</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Technical Step: Check log of GKE luz-store to see the keyword with query:</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`resource.type="k8s_container"
resource.labels.project_id="klara-nonprod"
resource.labels.location="europe-west6-a"
resource.labels.cluster_name="klara-nonprod"
resource.labels.namespace_name="dev"
labels.k8s-pod/app="luz-store" severity>=DEFAULT
"[getDocumentCache]"`</pre>
</td>
<td><p>It should be like this:</p>
![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/image-20260123-085825.png]]
<p>the key from cache is: `luzStore_IvyRestClientService_getDocumentCache_00a04daf-f2b3-41d5-8c12-2d1b4c48a36a_1_836271`</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>Technical Step: Check the existence of the key on the devportal of luz-cache:</p>
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence">`curl -X &#39;GET&#39; &#39;https://devportal.klara.ch/luz_cache/api/v2/<TENANT_ID>/<KEY>&#39;
  -H &#39;accept: application/json&#39;
  -H &#39;Authorization: Bearer <TOKEN>&#39;`</pre>
</td>
<td>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Sprint 148 - Test Report (0.03.13.xx)/attachments/invoice-run-v2uat-execution-change-filestore-cache-implement/image-20260123-090641.png]]</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

%% ai-graph-start %%

**Related notes:**
- [[Invoice Run V2UAT - Change filestore cache implement to filestore_utils]]
- [[Invoice Run V2UAT - No error when luz-store is running with multiple pods]]
- [[Invoice Run V2UATExecute - Prevent error when luz-store is multiple pods]]
- [[Invoice Run V2UAT - Execute - Apply Distributed Cache for customer information during the process of Invoice Run V2ecute]]
- [[Invoice Run V2UAT - Apply Distributed Cache for customer information during the process of Invoice Run V2]]

%% ai-graph-end %%