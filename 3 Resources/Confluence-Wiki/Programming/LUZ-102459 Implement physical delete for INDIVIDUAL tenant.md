---
ai_hash: 1306662ffa1f95da
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47440068767/LUZ-102459+Implement+physical+delete+for+INDIVIDUAL+tenant
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: LUZ-102459 Implement physical delete for INDIVIDUAL tenant
topic: programming
type: source
updated: 2023-08-04
---

# LUZ-102459 Implement physical delete for INDIVIDUAL tenant

> [!info] Imported from Confluence
> Space **TS** · updated 2023-08-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47440068767/LUZ-102459+Implement+physical+delete+for+INDIVIDUAL+tenant)
> Relevance 0.786 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47440068767_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-102459" macro-id="7d8a2e49-f015-4e86-b80c-63cceeeb9173" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-102459" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-102459</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<div hasbody="true" macro-id="b120a356-3f71-4a49-bb00-9a858519ec6e" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Script to mock data in luz-ke

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="32cda151-1a8b-4750-a3ea-2a76ac55a644" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-11-23 15:41:56.480', NULL, '2021-11-23 15:41:56.480');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 05:39:11.368', NULL, '2021-12-02 05:39:11.368');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 06:08:11.240', NULL, '2021-12-02 06:08:11.240');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 09:35:45.489', NULL, '2021-12-02 09:35:45.489');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 10:27:30.599', NULL, '2021-12-02 10:27:30.599');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 10:47:34.680', NULL, '2021-12-02 10:47:34.680');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 11:48:59.579', NULL, '2021-12-02 11:48:59.579');
INSERT INTO public.public_key_value_store
(kv_key, kv_value, create_by, create_date, update_by, update_date)
VALUES('ch.klara.bank.creditcard.sb.df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'df36be3c-37aa-42a0-bc0c-4a2f3b5bdfb4', 'SYSTEM', '2021-12-02 17:03:11.373', NULL, '2021-12-02 17:03:11.373');
```

</div>

</div>

<div hasbody="true" macro-id="a6c949ae-0a69-48a3-950e-5ffb755c38ba" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Script to test database

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7f45de17-9141-4865-abfb-24e9fb32e774" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
-- LUZ_TENANT_DIR
select * from luztenantdir.public.tenant_entry te where te.participant_id = 'afade006-4c22-432e-a942-d448970fb1f0'

-- LUZ_TENANT
select * from luztenant.public.tenant t where t.username like '%miracle_annguyen1@axonivy.io-%'

-- LUZ_TENANT_DELETION

select * from luztenantdeletion.public.tenant_deletion_status tds where tds.tenant_id = 'afade006-4c22-432e-a942-d448970fb1f0'

select * from luztenantdeletion.public.tenant_deletion_status_detail tdsd where tdsd.tenant_id = 'afade006-4c22-432e-a942-d448970fb1f0'

-- Need to check the api call to luz_hubsport, luz_audit, luz_docs
```

</div>

</div>

<div hasbody="true" macro-id="a4024a4b-4aad-4b8d-a6be-e798faf89b23" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Script to search log

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="208d8882-6429-401e-8ce4-d40f89cc6df0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
luz-audit: "luz-uri=DELETE /luz_audit/api/"
luz-tenant-dir: "luz-uri=DELETE /luz_tenant_dir/api/tenant-entries/"
luz-hubspot: "luz-uri=DELETE /luz_hubspot/api/individual-tenant/"
luztenant: "luz-uri=DELETE /luztenant/api/"
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5d36218c-a671-4a40-9d5e-bcf4931243fc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
luz-docs:

"[deleteDatabaseTenant]" OR
"[deleteVaultKEKTenant]" OR
"[deleteGCSFileByTenant]" OR
"[deleteDocumentStatistic]" OR
"Successfully delete all tenant data of tenantId" OR
"luz-uri=DELETE /luz_docs/api/"
```

</div>

</div>

## **1. TEST REPORT**

<div hasbody="true" macro-id="c7f1c73c-b8ad-452a-a6f2-b3784771e9f7" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Physical deletion

</div>

</div>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Test case</strong></p></th>
<th><p><strong>Call api success</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="5"><p>Delete individual tenant</p></td>
<td><p>luz_docs, luz_audit (googlecloud inkl. backups)</p></td>
<td><p>luz_docs 

![[47440068767-error.png]]

 

![[47440068767-error.png]]

 

![[47440068767-check.png]]

</p>

![[47440068767-nJtRA9CCQW.png]]


<p>luz_audit 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td></td>
<td></td>
</tr>
<tr>
<td rowspan="4"><p>Delete individual tenant after using digital letter box</p>
<ul>
<li><p>create folder</p></li>
<li><p>upload file</p></li>
<li><p>receive booking confirmation, reminder</p></li>
</ul></td>
<td><p>luz_docs (googlecloud inkl. backups)</p></td>
<td><p>luz_docs 

![[47440068767-error.png]]

 

![[47440068767-error.png]]

 

![[47440068767-check.png]]

</p>
<p>luz_audit 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="4"><p>Tenant login with mobile, but never login with web</p></td>
<td><p>luz_docs (googlecloud inkl. backups)</p></td>
<td><p>luz_docs 

![[47440068767-error.png]]

 

![[47440068767-error.png]]

 

![[47440068767-check.png]]

</p>
<p>luz_audit 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="4"><p>Run with multiple tenant</p></td>
<td><p>luz_docs (googlecloud inkl. backups)</p></td>
<td><p>luz_docs 

![[47440068767-error.png]]

 

![[47440068767-error.png]]

 

![[47440068767-check.png]]

</p>
<p>luz_audit 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Other case</strong></p></th>
<th colspan="2"><p><strong>Expect</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="4"><p>Call api faill and retry 3 time</p></td>
<td><p>luz_docs, luz_audit (googlecloud inkl. backups)</p></td>
<td><p><strong>should persist the failed step to tenant_deletion_status_detail</strong></p>
<p>luz_docs</p>
<ul>
<li><p>retry 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></li>
<li><p>save failed step 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></li>
</ul>
<p>luz_audit</p>
<ul>
<li><p>retry 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></li>
<li><p>save failed step 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>retry 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>save failed step 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>retry 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>save failed step 

![[47440068767-error.png]]

 

![[47440068767-error.png]]

 

![[47440068767-error.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>have other step fail → don’t call to luztenant_service 

![[47440068767-check.png]]

</p>
<p>retry 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>save failed step 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="4"><p>Call fail and run job again → continue fail</p></td>
<td><p>luz_docs, luz_audit (googlecloud inkl. backups)</p></td>
<td><p>update status from “PENDING_RETRY“ → “RETRY_FAILED“ 

![[47440068767-check.png]]

</p>
<p>delete individual tenant (luztenant) 

![[47440068767-check.png]]

</p>
<p>job run next time → don’t execute this tenant 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>update status from “PENDING_RETRY“ → “RETRY_FAILED“ 

![[47440068767-check.png]]

</p>
<p>delete individual tenant (luztenant) 

![[47440068767-check.png]]

</p>
<p>job run next time → don’t execute this tenant 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>update status from “PENDING_RETRY“ → “RETRY_FAILED“ 

![[47440068767-check.png]]

</p>
<p>delete individual tenant (luztenant) 

![[47440068767-check.png]]

</p>
<p>job run next time → don’t execute this tenant 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>update status from “PENDING_RETRY“ → “RETRY_FAILED“ 

![[47440068767-check.png]]

</p>
<p>job run next time → don’t execute this tenant 

![[47440068767-error.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td rowspan="4"><p>Call fail and run job again → call success</p></td>
<td><p>luz_docs, luz_audit (googlecloud inkl. backups)</p></td>
<td><p>update status from “PENDING_RETRY“ → “DELETED“</p>
<p>remove failed step in tenant_deletion_status_detail</p>
<p>luz_docs: 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>luz_audit: 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>tenant_entry (luz_tenant_dir)</p></td>
<td><p>update status from “PENDING_RETRY“ → “DELETED“ 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>remove failed step in tenant_deletion_status_detail</p></td>
</tr>
<tr>
<td><p>Delete Hubspot data</p></td>
<td><p>update status from “PENDING_RETRY“ → “DELETED“ 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>remove failed step in tenant_deletion_status_detail</p></td>
</tr>
<tr>
<td><p>Delete individual tenant (luztenant)</p></td>
<td><p>update status from “PENDING_RETRY“ → “DELETED“ 

![[47440068767-check.png]]

 

![[47440068767-check.png]]

</p>
<p>remove failed step in tenant_deletion_status_detail</p></td>
</tr>
<tr>
<td><p>Call api fail with 404 status</p></td>
<td colspan="2"><p>don’t retry 

![[47440068767-check.png]]

</p>
<p>don’t save failed step 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>multiple tenant have fail and success</p></td>
<td colspan="2"><p>

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p>Only process with limit tenant</p></td>
<td colspan="2"><p>

![[47440068767-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

<div hasbody="true" macro-id="c18adba7-42d4-4427-a4ac-1b23d707aef3" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Logical deletion for BUSINESS COMPANY

</div>

</div>

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Failed step</strong></p></th>
<th><p><strong>Expect</strong></p></th>
</tr>
&#10;<tr>
<td><p><code>DELETE_TENANT_DIRECTORY</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>DELETION_BANK_ACCOUNT</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>DELETE_ELM_COMPANY_BANKING_BACKGROUND_JOB</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>DELETE_KLEE_COMPANY_SYNCHRONIZATION</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>DELETE_ONLINE_WEBSITE</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>DISABLE_SSL_AUTO_RENEWAL</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>DELETE_OWN_DOMAIN</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>INFORM_USERS</code></p></td>
<td><p>we only save the failed step when can not get the generic token. 

![[47440068767-check.png]]

</p>
<p>in case we can not send the email to user → have the log, don’t save the failed step. 

![[47440068767-check.png]]

</p></td>
</tr>
<tr>
<td><p><code>INFORM_SUPPORT</code></p></td>
<td><p>save failed step to database 

![[47440068767-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

## **2. CODE REVIEW REPORT  **

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
<th><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-No."><strong>No.</strong></h3></th>
<th><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-REVIEWLOGIC"><strong>REVIEW LOGIC</strong></h3></th>
<th><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Passed?"><strong>Passed?</strong></h3></th>
<th><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Explanation(textorcapturedimage)"><strong>Explanation (</strong><em>text or captured image</em><strong>)</strong></h3></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-HavecoveredJUnittests?"><strong>Have covered JUnit tests?</strong></h3>
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
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Havenoside-effectfromthechanges?"><strong>Have no side-effect from the changes?</strong></h3>
<h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-(checkotherplacesthatcalltothis)">(<em>check other places that call to this</em>)</h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Handlingerrorsiscorrect?"><strong>Handling errors is correct?</strong></h3>
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
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Noduplicatedcode?"><strong>No duplicated code?</strong></h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-AttachjenkinbuildresultinPR"><strong>Attach jenkin build result in PR</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Checkingimpactwithintegrationtest"><strong>Checking impact with integration test</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Functioniscorrectpurpose(noneedtosplitfunction)"><strong>Function is correct purpose ( no need to split function)</strong></h3>
<h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Datatypeiscorrect"><strong>Datatype is correct</strong></h3></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td></td>
</tr>
<tr>
<td><p><br />
</p></td>
<td colspan="3"><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-REVIEWPERFORMANCEISSUES"><strong>REVIEW PERFORMANCE ISSUES</strong></h3></td>
</tr>
<tr>
<td><p>8</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-NoN+1issue?"><strong>No N + 1 issue?</strong></h3>
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
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Noduplicatedcalls"><strong>No duplicated calls</strong></h3>
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
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Canusecaching?"><strong>Can use caching?</strong></h3>
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
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Checkcorrectionofusingbeanscopes"><strong>Check correction of using  bean scopes</strong></h3>
<p><br />
</p></td>
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
<td colspan="3"><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-REVIEWCODINGCONVENTION"><strong>REVIEW CODING CONVENTION</strong></h3></td>
</tr>
<tr>
<td><p>12</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Followednamingconversion"><strong>Followed naming conversion</strong></h3>
<p><br />
</p></td>
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
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Classes/methodsarewellorganized?"><strong>Classes/methods are well organized?</strong> </h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>14</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Class/methodcouldberefactored?"><strong>Class/method could be refactored?</strong></h3>
<p><br />
</p></td>
<td><ul>
<li><span class="placeholder-inline-tasks">OK</span></li>
<li><span class="placeholder-inline-tasks">NOT OK</span></li>
</ul></td>
<td><p><br />
</p></td>
</tr>
<tr>
<td><p>15</p></td>
<td><h3 id="LUZ-102459ImplementphysicaldeleteforINDIVIDUALtenant-Havejava-docforcomplexclass/method/parameter/api?"><strong>Have java-doc for complex class/method/parameter/api?</strong></h3></td>
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
- [[LUZ-102045 Implement physical delete for COMPANY tenant Part 2 (Postgres cont)]]
- [[LUZ-106177 - Delete credit card expiry reminder]]
- [[How to execute API to create sync event for post from tenant schemas to public table]]
- [[Delete company - Old way]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]

%% ai-graph-end %%