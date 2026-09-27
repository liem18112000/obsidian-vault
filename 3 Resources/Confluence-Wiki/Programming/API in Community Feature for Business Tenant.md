---
ai_hash: dc052ac2cc1fd8cd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48621715651/API+in+Community+Feature+for+Business+Tenant
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: API in Community Feature for Business Tenant
topic: programming
type: source
updated: 2025-08-20
---

# API in Community Feature for Business Tenant

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-08-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48621715651/API+in+Community+Feature+for+Business+Tenant)
> Relevance 0.731 · topic `programming`

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
<th><p><strong>UIB api</strong></p></th>
<th><p><strong>LUZ_COMMUNITIES</strong></p></th>
<th></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Get cities</p>
<p><code>GET</code></p>
<p>/:tenantId/address-book/cities</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Create tags</p>
<p><code>GET</code></p>
<p>/:tenantId/address-book/created-tags</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Create contact</p>
<p><code>POST</code></p>
<p>Update contact</p>
<p><code>PUT</code></p>
<p>Get contact</p>
<p><code>GET</code></p>
<p>Delete contact</p>
<p><code>DELETE</code></p>
<p>/:tenantId/address-book/contacts</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Count contact</p>
<p><code>GET</code></p>
<p>/:tenantId/address-book/count'</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Matching ePost CRM run</p>
<p><code>POST</code></p>
<p>/:tenantId/address-book/contacts/crm-epost-matching-run</p></td>
<td></td>
<td></td>
<td><p>mathching contacts with ePost and CRM</p></td>
</tr>
<tr>
<td><p>Check if contact deletetable</p>
<p><code>GET</code></p>
<p>/:tenantId/address-book/contacts/:contactId/deletable</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Import contact</p>
<p><code>POST</code></p>
<p>/:tenantId/address-book/import-contacts</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Import contact status</p>
<p><code>GET</code></p>
<p>/:tenantId/address-book/contacts/progress-import</p></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

luz_communities contact: src/main/java/ch/klara/luz/communities/resource/CommunityContactResource.java

`Ex: http://luz-communities:8080/luz_communities/api/8815e317-f12f-4d5b-93e1-e0bc27f233ab/address-book/cities?maxResult=50&zipCode=22*&cityName=*]`

%% ai-graph-start %%

**Related notes:**
- [[Luz_google Api Document]]
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[Getting tenant list]]
- [[LUZ-146746 Use correct API's for delete, restore and their undo]]
- [[15. Update companies by tenant id]]

%% ai-graph-end %%