---
title: "[Research] Protect ePost inbox with new Digital_Letterbox permission"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47393046550/Research+Protect+ePost+inbox+with+new+Digital_Letterbox+permission
space: "TP2020"
topic: security
relevance: 0.701
depth: 2.4
updated: 2023-06-19
attachments: 3
tags:
  - confluence
  - security
  - space/tp2020
---

# [Research] Protect ePost inbox with new Digital_Letterbox permission

> [!info] Imported from Confluence
> Space **TP2020** · updated 2023-06-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47393046550/Research+Protect+ePost+inbox+with+new+Digital_Letterbox+permission)
> Relevance 0.701 · topic `security`

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="1c19bb18-a724-44da-b84f-69b5f5f1aaaa" macro-name="toc" numberedoutline="false" structure="list">

</div>

Related items:

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-99048" macro-id="0b21acf4-1e26-4424-8ce4-e4fd25a4507f" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-99048" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-99048</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93880" macro-id="315ec053-4426-400e-93dd-dc070f7b880c" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93880" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93880</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93883" macro-id="2383cb64-b42d-4b46-bf44-96fc9f8a9cf5" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93883" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93883</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93882" macro-id="89868c43-357c-44e5-b97e-1c2c03dc321d" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93882" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93882</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

Related confluence pages:

[Verify KLARA Permissions](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513322076/Verify+KLARA+Permissions)

<a href="https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47294939137/BUSINESS+User+roles+and+permissions+for+ePost----+TARGET" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47294939137/BUSINESS+User+roles+and+permissions+for+ePost----+TARGET</a>

## 1. How can we create new permission?

Related module: luzsec_service

- Create a new permission constant to protect the inbox in the `PermissionConstant` class

  

![[47393046550-image-20230601-083800.png]]



- Add the constant created above to the related roles

  

![[47393046550-image-20230601-083922.png]]



## 2. The list of Roles that need to be added to this new Digital_Letterbox permission?

<div>

|     |                              |
|-----|------------------------------|
|     | **Role**                     |
| 1   | COMPANY_ADMINISTRATOR        |
| 2   | TRUSTED_USER (external_user) |
| 3   | ePost Letterbox (new role)   |
| 4   | INDIVIDUAL_ADMIN             |
| 5   | INDIVIDUAL_TRUSTED_USER      |
| 6   | INDIVIDUAL_DIRECTORY_USER    |

</div>

## 3. What happens for the user doesn’t have the new Digital_Letterbox permission?

<div>

|  |  |  |  |
|----|----|----|----|
|  | **Need to do** | **Status** | **User story implement** |
| 1 | Hide the Inbox accesspoint in **business web's** left menue bar | <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="c1c51002-ff13-44b2-93e6-9033a7f0d76e" macro-name="status">DONE</span> | <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93880" macro-id="00c5e275-0908-4842-8462-9a910e261fd2" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93880" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93880</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> |
| 2 | Hide the deleted letter from inbox in the TRASH folder | <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="dbc969fb-268f-4c20-9299-8f7cd3c00ff5" macro-name="status">DONE</span> | <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93880" macro-id="63ff3915-af4d-4f05-95e1-7afe5956dd48" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93880" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93880</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> |
| 3 | Just show the stored branded document inside branded folder | <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="e1af6d75-fbc2-4e19-bf83-17ade76d1500" macro-name="status">DONE</span> | <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47393046550_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-93880" macro-id="7971dcd5-996c-4617-a1e7-66934b6d2f53" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-93880" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-93880</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> |

</div>

## 4. The current `LUZ_DOCS_VIEW_CONTROLLER` permission to protect API

#### 4.1 Which module requires this permission?

luz_docs_view_controller

#### 4.2 Which are roles added to `LUZ_DOCS_VIEW_CONTROLLER` permission?

<div>

|     |                               |
|-----|-------------------------------|
|     | **Role**                      |
| 1   | COMPANY_ADMINISTRATOR         |
| 2   | TRUSTED_USER (external_user)  |
| 3   | PAYROLL_SPECIALIST            |
| 4   | accountant                    |
| 5   | CUSTOMER_RELATIONSHIP_MANAGER |
| 6   | BOOKING                       |
| 7   | INDIVIDUAL_ADMIN              |
| 8   | INDIVIDUAL_TRUSTED_USER       |
| 9   | LUZ_DOCS_VIEW_CONTROLLER      |
| 10  | INDIVIDUAL_DIRECTORY_USER     |

</div>

## 6. Questions

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Question</strong></p></th>
<th><p><strong>Answer</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>a. The new permission is just to protect the inbox access point on the UI only. Is it correct?</p>
<p>b. if a) yes, That mean we no need to protect the eArchive actions with the new permission. Is it correct?</p></td>
<td><p>Correct</p>
<p>Correct</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Example the user A can not access the Inbox, however user A still can access the eArchive. Do we allow the user A can see the branded folder?</p></td>
<td><p>Correct.</p>
<p>Yes</p></td>
</tr>
</tbody>
</table>

</div>
