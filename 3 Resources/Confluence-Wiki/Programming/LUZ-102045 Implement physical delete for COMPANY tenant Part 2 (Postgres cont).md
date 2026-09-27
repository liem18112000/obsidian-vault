---
ai_hash: d0b87d2323eec6f5
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.81
entities: []
relevance: 0.724
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47454322785/LUZ-102045+Implement+physical+delete+for+COMPANY+tenant+Part+2+Postgres+cont
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: LUZ-102045 Implement physical delete for COMPANY tenant | Part 2 (Postgres
  cont)
topic: programming
type: source
updated: 2023-08-10
---

# LUZ-102045 Implement physical delete for COMPANY tenant | Part 2 (Postgres cont)

> [!info] Imported from Confluence
> Space **TS** · updated 2023-08-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47454322785/LUZ-102045+Implement+physical+delete+for+COMPANY+tenant+Part+2+Postgres+cont)
> Relevance 0.724 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47454322785_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-102045" macro-id="698d21c2-5d57-4332-aa98-45af8a669390" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-102045" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-102045</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<td colspan="3"><p><strong><span>LOGICAL DELETION</span></strong></p></td>
</tr>
<tr>
<td><p><strong>Test case</strong></p></td>
<td><p><strong>Expect</strong></p></td>
<td><p><strong>How to check</strong></p></td>
</tr>
<tr>
<td rowspan="11"><p>Delete successful</p></td>
<td><p>Save deleted tenant to table <strong>deleted_company_tenant_data</strong> of <strong>luz_tenant_deletion</strong> 

![[47454322785-check.png]]

 <strong></strong></p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b4c838cd-8e41-4979-a687-13ea82cb4ec1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>select * from luztenantdeletion.public.deleted_company_tenant_data dctd 
where dctd.tenant_id = &#39;3d47746e-57a5-4700-bb67-1269dfac4a5a&#39;</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>Mark tenant as deleted in table <strong>tenant</strong> of <strong>luztenant</strong> (all user is assigned) 

![[47454322785-check.png]]

</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="16c3f341-5f4b-40d5-abac-7d283cbfc5ae" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>select * from luztenant.public.tenant t where t.tenantid = &#39;3d47746e-57a5-4700-bb67-1269dfac4a5a&#39;
select * from luztenant.public.companyinfo c where c.tenantid = &#39;3d47746e-57a5-4700-bb67-1269dfac4a5a&#39;
select * from luztenant.public.login_tracking lt where lt.companyinfo_id = 8470</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>Call api to luz_tenant_directory 

![[47454322785-check.png]]

</p></td>
<td><p>mock data</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="df12dcc7-a2e7-4fdf-87a3-dcb3417b817c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>INSERT INTO public.tenant_entry
(participant_id, create_date, update_date, level_of_trust, allow_digital_letter, &quot;type&quot;, create_by, update_by, &quot;language&quot;)
VALUES(&#39;3d47746e-57a5-4700-bb67-1269dfac4a5a&#39;, &#39;2021-08-04 11:22:52.257&#39;, NULL, &#39;SILVER&#39;, true, &#39;COMPANY&#39;, &#39;miracle_annguyen1@axonivy.io&#39;, NULL, &#39;de&#39;);</code></pre>
</div>
</div>
<p>check after delete</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="22af2b51-f032-45e5-91da-4bb9dc8a8646" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>select * from luztenantdir.public.tenant_entry te where te.participant_id = &#39;3d47746e-57a5-4700-bb67-1269dfac4a5a&#39;</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>Delete bank account</p></td>
<td></td>
</tr>
<tr>
<td><p>Delete ELM</p></td>
<td></td>
</tr>
<tr>
<td><p>Delete Klee</p></td>
<td></td>
</tr>
<tr>
<td><p>Disable SSL</p></td>
<td></td>
</tr>
<tr>
<td><p>Delete own domain</p></td>
<td></td>
</tr>
<tr>
<td><p>Delete online website url mapping</p></td>
<td></td>
</tr>
<tr>
<td><p>Send email to supporter if has failed step</p></td>
<td></td>
</tr>
<tr>
<td><p>Send success email to users</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-102459 Implement physical delete for INDIVIDUAL tenant]]
- [[Delete company - Old way]]
- [[Script to list all the information of the tenants]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[15. Update companies by tenant id]]

%% ai-graph-end %%