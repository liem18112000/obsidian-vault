---
ai_hash: 8f0e022fb295d620
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
confluence_id: '48950509569'
confluence_path: Team Kepler > Test Case Library > Test Execution / Evidences > Archive
  > Sprint 145 - Test Report
created: 2025-12-09
entities: []
source: Confluence · TK - Team Kepler
status: reference
tags:
- confluence
- invoice-run
- luz-store
- sprint
- testing
title: '[Invoice Run V2][UAT][Execute] - Prevent error when luz-store is multiple
  pods'
type: source
updated: 2025-12-17
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48950509569/Invoice+Run+V2+UAT+Execute+-+Prevent+error+when+luz-store+is+multiple+pods
---

# [Invoice Run V2][UAT][Execute] - Prevent error when luz-store is multiple pods

*Confluence source · Team Kepler › Test Case Library › Test Execution / Evidences › Archive › Sprint 145 - Test Report · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48950509569/Invoice+Run+V2+UAT+Execute+-+Prevent+error+when+luz-store+is+multiple+pods) · updated 2025-12-17*

## Test case Template

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>**Test Case#:** 1</p></td>
<td><p>**Test Case Name:** Test Case Template</p></td>
</tr>
<tr>
<td><p>**System:** LUZ-DOCS</p></td>
<td><p>**Subsystem: LUZ-STORE**</p></td>
</tr>
<tr>
<td><p>**Test case designed by:**<br />
[[Kepler] - Liem Doan](https://axonivy.atlassian.net/wiki/people/712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4?ref=confluence)</p></td>
<td><p>**Design Date:** 24 Sep 2025</p></td>
</tr>
<tr>
<td><p>**Short description: **</p></td>
<td><h2 id="id-[InvoiceRunV2][UAT][Execute]-Preventerrorwhenluz-storeismultiplepods-Shortdescription:" data-local-id="94dcafec-8951-4c25-9582-da07f2de76ab">Short description:</h2>
<p>Verify that when `luz-store` runs with multiple pods, all consuming services (`luz-reporting`, `luz-reporting-dotnet`, `luz-docs-creator-client`, `luzfin-finance`, `luz-online-payment`) can scale and handle the increased request load so that the Invoice Run (including PDF generation) completes successfully and without errors in a multi‑pod environment.</p></td>
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
<li><p>The token bucket configuration:</p>
<ul>
<li><p>Initial capacity: 60 tokens</p></li>
<li><p>Refresh rate: 60 tokens / 60 seconds</p></li>
</ul></li>
<li><p>Number of instance: 3 - 5 instance of LUZ-STORE</p></li>
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
<li><p>No duplicates or missing records; system is stable for the next run.</p></li>
</ul></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

### Test Steps

<table>
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
<td><p>1000 event will be published.</p></td>
<td><p>There must be less or equal 60 result after 60 seconds (1 minute).</p></td>
<td></td>
<td></td>
<td><p>![[3 Resources/Confluence/Team Kepler/Test Case Library/Test Execution - Evidences/Archive/attachments/invoice-run-v2uatexecute-prevent-error-when-luz-store-is-mul/check.png]]</p></td>
<td><p>🚫</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>1000 event will be published.</p></td>
<td><p>There must be less or equal 120 result after 120 seconds (1 minute).</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>1000 event will be published.</p></td>
<td><p>There must be less or equal 300 result after 300 seconds (1 minute).</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>1000 event will be published.</p></td>
<td><p>There must be less or equal 600 result after 600 seconds (1 minute).</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>1000 event will be published.</p></td>
<td><p>There must be less or equal 900 result after 60 seconds (1 minute).</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>1000 event will be published.</p></td>
<td><p>There must be less or equal 60 result after 60 seconds (1 minute).</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>8</p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td><p> </p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

%% ai-graph-start %%

**Related notes:**
- [[Invoice Run V2UAT - No error when luz-store is running with multiple pods]]
- [[Invoice Run V2UAT - Execution - Change filestore cache implement to filestore_utils]]
- [[Invoice Run V2UAT - Change filestore cache implement to filestore_utils]]
- [[Invoice Run V2UAT - Execute - Apply Distributed Cache for customer information during the process of Invoice Run V2ecute]]
- [[Invoice Run V2UAT - Apply Distributed Cache for customer information during the process of Invoice Run V2]]

%% ai-graph-end %%