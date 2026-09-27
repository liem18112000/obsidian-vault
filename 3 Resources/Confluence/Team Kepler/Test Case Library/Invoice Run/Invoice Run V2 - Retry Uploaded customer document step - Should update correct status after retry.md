---
ai_hash: 5777acf980a701f8
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '49072144385'
confluence_path: Team Kepler > Test Case Library > Invoice Run
created: 2026-01-22
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
- testing
title: '[Invoice Run V2] - Retry "Uploaded customer document" step - Should update
  correct status after retry'
type: source
updated: 2026-02-03
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49072144385/Invoice+Run+V2+-+Retry+Uploaded+customer+document+step+-+Should+update+correct+status+after+retry
---

# [Invoice Run V2] - Retry "Uploaded customer document" step - Should update correct status after retry

*Confluence source · Team Kepler › Test Case Library › Invoice Run · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49072144385/Invoice+Run+V2+-+Retry+Uploaded+customer+document+step+-+Should+update+correct+status+after+retry) · updated 2026-02-03*

## Test case Template

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Retry "Uploaded customer document"</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-STORE</p></td>
<td><p>**Subsystem: Invoice-Run V2**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 24 Sep 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2]-Retry"Uploadedcustomerdocument"step-Shouldupdatecorrectstatusafterretry-Shortdescription:" data-local-id="b7358e29-104c-46b6-981b-da07aa96a3c6">Short description:</h2>
<p>During the Phase 3 of Invoice Run on Business Tenants, we experienced 2 “Uploaded customer document failed” as LUZ-Webclient was temporarily not available due to a restart.</p>
<p>After the invoice fails have been retried, some invoices remained in status failed, even-though the document has been successfully upload to Accounting booking system.</p></td>
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
<li><p>At step 3, webclient is unavailable</p></li>
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
<li><p>The status should be updated to “Uploaded customer document”</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

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
<td colspan="3"><p>**Test Case**</p></td>
<td colspan="3"><p>**Test Execution**</p></td>
</tr>
<tr>
<td><p>**Step**</p></td>
<td><p>**Action**</p></td>
<td><p>**Expected Behavior**</p></td>
<td><p>**Actual Behavior**</p></td>
<td><p>**Comment**</p></td>
<td><p>**Status**</p></td>
</tr>
<tr>
<td><p>1</p></td>
<td><p>Start an Invoice Run with any invoice item for Company tenant<br />
Continue until finsined step 2 successfully</p></td>
<td>![[image-20260129-034849.png]]</td>
<td></td>
<td></td>
<td rowspan="3"><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Invoice Run/attachments/invoice-run-v2-retry-uploaded-customer-document-step-should-/check.png]]</p>
<p>**Passed**</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Start sending and upload step<br />
Make webclient not available so the error “Uploadd customer document failed” happen</p></td>
<td><p>It should be like this:<br />
</p>
![[image-20260129-035231.png]]</td>
<td></td>
<td><p> </p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Click retry<br />
After retry, the status should upload to “Uploaded customer document”</p></td>
<td><p>It should be like this:</p>
![[image-20260129-035322.png]]</td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

%% ai-graph-start %%

**Related notes:**
- [[Invoice Run V2UAT - No error when luz-store is running with multiple pods]]
- [[Invoice Run V2UAT - Update latest Stimulsoft template - Execution]]
- [[Invoice Run V2UAT - Execute - Apply Distributed Cache for customer information during the process of Invoice Run V2ecute]]
- [[Invoice Run V2UAT - Update latest Stimulsoft template]]
- [[Invoice Run V2UAT - Apply Distributed Cache for customer information during the process of Invoice Run V2]]

%% ai-graph-end %%