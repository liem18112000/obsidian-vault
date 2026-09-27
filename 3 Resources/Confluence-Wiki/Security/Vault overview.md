---
title: "Vault overview"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435704/Vault+overview
space: "LUZ"
topic: security
relevance: 0.777
depth: 2.5
updated: 2021-05-17
attachments: 6
tags:
  - confluence
  - security
  - space/luz
---

# Vault overview

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-05-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435704/Vault+overview)
> Relevance 0.777 · topic `security`

# 

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="dad13b0d-dbcc-47fc-ab2b-a24c3981b244" macro-name="toc">

</div>

# Situation

As you known, we have 4 main components for Vault, we need to choose and define the solution that match with the requirement for security, authentication, scaling up, authorization.

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20530435704_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-52048" macro-id="2bce6f34-57db-445d-8a7a-4e2a34dbb2ff" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-52048" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-52048</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

# Squence diagram flow

<span class="legacy-color-text-red2">**Valut work flow that just my asumption how it work.**</span>


![[20530435704-authentication and authorization.png]]



<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Step</th>
<th>Description</th>
</tr>
&#10;<tr>
<td><p>1/ Get client token</p></td>
<td><div class="content-wrapper">
<p>we will login with JWT token and role to get the client key for requesting encrypt and decrypt data</p>

![[20530435704-image2021-5-17_10-28-42.png]]


</div></td>
</tr>
<tr>
<td>2/ Get JWS and role data</td>
<td>Vault will get the JWS and role for validating JWT token.</td>
</tr>
<tr>
<td>3/ Response</td>
<td><br />
</td>
</tr>
<tr>
<td>4/ Validate JWS base on configruation</td>
<td>Vault will do the validate for JWT token also create new entity in first time to keep the tenant id in meta data of vault for authorization.</td>
</tr>
<tr>
<td>5/ Response client token</td>
<td><div class="content-wrapper">

![[20530435704-image2021-5-17_10-30-9.png]]


</div></td>
</tr>
<tr>
<td>6/ Request encrypt with keyring is tenantId</td>
<td><div class="content-wrapper">

![[20530435704-image2021-5-17_10-32-11.png]]


</div></td>
</tr>
<tr>
<td>7/ get policy for checking tenant id</td>
<td><div class="content-wrapper">
<p>we have add some configruation for the authorization in policy  that will detect and check it in entity that already created above.</p>

![[20530435704-image2021-5-17_10-10-8.png]]


</div></td>
</tr>
<tr>
<td>8/ Return the result</td>
<td><br />
</td>
</tr>
<tr>
<td>9/ Call encrypt</td>
<td><br />
</td>
</tr>
<tr>
<td>10/ Return success</td>
<td><br />
</td>
</tr>
<tr>
<td>11/ Return cipher text</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# Authentication method

[Authentication method](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530434585/Authentication+method)

# Authorization & Authentication

[Authorization](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435691/Authorization)

# Storage Backend system

[Storage backend system](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530434634/Storage+backend+system)

# Vault Secret Engine

[Vault Secret Engines](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530434536/Vault+Secret+Engines)

# Vault Unsealing

[Investigate Vault Unsealing](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530435296/Investigate+Vault+Unsealing)
