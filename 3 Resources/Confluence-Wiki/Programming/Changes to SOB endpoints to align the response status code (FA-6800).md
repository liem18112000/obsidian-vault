---
ai_hash: d15b963d981fe8a6
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.806
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/49025220678/Changes+to+SOB+endpoints+to+align+the+response+status+code+FA-6800
space: Arrow
status: reference
tags:
- confluence
- programming
- space/arrow
title: Changes to SOB endpoints to align the response status code (FA-6800)
topic: programming
type: source
updated: 2026-02-25
---

# Changes to SOB endpoints to align the response status code (FA-6800)

> [!info] Imported from Confluence
> Space **Arrow** · updated 2026-02-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/49025220678/Changes+to+SOB+endpoints+to+align+the+response+status+code+FA-6800)
> Relevance 0.806 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="6f70d97b-cb3a-4b6b-a34e-99763684741f" macro-name="toc">

</div>

Related ticket: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49025220678_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-6800" macro-id="28079be9-baab-49d7-9ca8-78ad23f4995d" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-6800" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-6800</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

# 1. Endpoints changes overview 

<div>

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
<th></th>
<th><p><strong>Endpoint</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Existing behavior</strong></p></th>
<th><p><strong>New behavior</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>GET</p>
<p>/dossiers?onboardingKey=xxx</p></td>
<td><p>Get a dossier by onboarding key</p></td>
<td><p>Dossier not found: return 404</p></td>
<td><p>Dossier not found: return 204 + empty data</p></td>
<td><p>5pts</p></td>
</tr>
<tr>
<td>2</td>
<td><p>POST</p>
<p>/dossiers</p></td>
<td><p>Create a dossier</p></td>
<td><p>Dossier exists by onboarding key: return 409</p></td>
<td><p><span>Not change </span></p></td>
<td><p><span>0pts</span></p></td>
</tr>
<tr>
<td>3</td>
<td><p>GET</p>
<p>/dossiers/{dossierId}?requestId=xxx</p></td>
<td><p>Get a dossier by dossier id</p></td>
<td><p>Invalid requestId: return 403</p>
<p>Dossier not found: return 404</p></td>
<td><p>Dossier not found: return 204 + empty data</p>
<p>403: return 200 + error in response</p></td>
<td><p>5pts</p></td>
</tr>
<tr>
<td>4</td>
<td><p>GET</p>
<p>/dossiers/{dossierId}/selections</p></td>
<td><p>Get valid options for cards/packages selection for dossier</p></td>
<td><p>Dossier not found: return 404</p>
<p>Operation not allowed (e.g. dossierProgress = null): return 403</p></td>
<td><p>Dossier not found: 200 + error in response</p>
<p>Operation not allowed: 200 + error in response</p></td>
<td><p>3pts</p></td>
</tr>
<tr>
<td>5</td>
<td><p>PATCH</p>
<p>/dossiers/{dossierId}/selections</p></td>
<td><p>Update selections for dossier (card, packages,…)</p></td>
<td><p>Dossier not found: return 404</p>
<p>Operation not allowed (<span>Updated 19/01/2026</span>): return 403. For example: isNotAllowedUpdateSelection(), isAllowInStateSigning = false,…</p>
<p>Business rejected (<span>Updated 19/01/2026</span>): return 400. For example, isAgreedTermAndCondition = false, isInvalidAdditionalProducts(),…</p></td>
<td><p>Dossier not found: 200 + error in response</p>
<p>Operation not allowed: 200 + error in response</p>
<p>Business rejected: 200 + error in response</p></td>
<td><p>3pts</p></td>
</tr>
<tr>
<td>6</td>
<td><p>POST</p>
<p>/dossiers/{dossierId}/identifications</p></td>
<td><p>Register &amp; initialize an identification process for the dossier</p></td>
<td><p>COUNTRY_NOT_OFFERED, COUNTRY_NOT_SUPPORTED_ONLINE_IDENTIFICATION, REACHED_MAXIMUM_TIME, FidentityInvalidNationalityException…: return 400</p></td>
<td><p>For 400 cases: return 200 + error in response</p></td>
<td><p>5pts<br />
<span>Also used by Part Automisation</span></p></td>
</tr>
<tr>
<td>7</td>
<td><p>POST</p>
<p>/dossiers/{dossierId}/registrations</p></td>
<td><p>Verify OTP code</p></td>
<td><p>RETRY_OTP_CODE_REACHED_MAXIMUM_TIME, EXPIRED, FAILED,…: return 400</p>
<p>State in DB not valid: 403</p></td>
<td><p>For 400 cases: return 200 + error in response</p>
<p>403: return 200 + error in response</p></td>
<td><p>3pts</p></td>
</tr>
<tr>
<td>8</td>
<td><p>PATCH</p>
<p>/dossiers/{dossierId}/registrations</p></td>
<td><p>Generate OTP &amp; collect phone number</p></td>
<td><p>Dossier not found: 404</p>
<p>REACHED_MAXIMUM_TIME, RETRY_OTP_CODE_REACHED_MAXIMUM_TIME,…: 400</p></td>
<td><p>Dossier not found: 200 + error in response</p>
<p>For 400 cases: change to 200 + error in response</p></td>
<td><p>3pts</p></td>
</tr>
<tr>
<td>9</td>
<td><p>PATCH</p>
<p>/dossiers/{dossierId}/collections</p></td>
<td><p>Collect user inputs</p></td>
<td><p>Zip code excluded, address required… (BUSINESS_REJECTED): 400</p>
<p>State in DB not valid: 403</p></td>
<td><p>For 400 cases: change to 200 + error in response</p>
<p>403: return 200 + error in response</p></td>
<td><p>2pts</p></td>
</tr>
<tr>
<td>10</td>
<td><p>POST</p>
<p>/dossiers/{dossierId}/verifications</p></td>
<td><p>Third check</p></td>
<td><p>NOK_FRAUD, NOK_MINOR, DUPLICATED_PERSON,…: 400</p>
<p>Current state of dossier != COMPLIANCE_VERIFICATION: 403</p></td>
<td><p>For 400 &amp; 403 cases: change to 200 + error in response</p></td>
<td><p>6pts</p>
<p>Handle also error payload isAgeInMinorRange (Duplicated and Minor):</p>

![[49025220678-image-20260112-033958.png]]

</td>
</tr>
<tr>
<td>11</td>
<td><p>POST /dossiers/{dossierId}/signs</p></td>
<td><p>Generate URL for the QR for the user to scan &amp; start Signing</p></td>
<td><p>FAILED, NOK_OTHERS,…: 400</p></td>
<td><p>For 400 cases: change to 200 + error in response</p></td>
<td><p>5pts</p></td>
</tr>
<tr>
<td>12</td>
<td><p>POST /dossiers/{dossierId}/signs/callback</p></td>
<td><p>Polling to check the Signing result</p></td>
<td><p>STARTED, COMPLETED, NOK_KYC, FAILED,…: (UnattendedDossierSpecificCaseException) 400<br />
<br />
OperationNotAllowedException: 403 (e.g. state != SIGNING,..)</p>
<p>Dossier not found: 404</p></td>
<td><p><span>Should change to GET because of polling?</span></p>
<p>For 400 / 403 / 404 cases: change to 200 + error in response</p></td>
<td><p>6pts</p>
<p><span>Also used by Part Automisation</span></p></td>
</tr>
<tr>
<td>13</td>
<td><p>POST /dossiers/{dossierId}/identifications/callback</p></td>
<td><p><span>Webhook for Fidentity</span> to notify us of the dossier’s identification result</p></td>
<td><p>Passed requestId does not match the dossierId: 403</p>
<p>(Never happens) Can not find a handler for SUSPICION_OF_FRAUD status: 400<br />
<br />
<del>UnattendedDossierSpecificCaseException for OUTSIDE_WORKING_HOUR, REACHED_MAXIMUM_TIME: 400 (</del><span><del>Update 30/01/2026</del></span><del>)</del></p></td>
<td><p>403: return 200 + error in response<br />
(Never happens) 400: return 200 + error in response</p>
<p><span>Notify Fidentity about the changes?</span></p></td>
<td><p><span>2pts</span></p>
<p><span>Also used by Part Automisation</span></p></td>
</tr>
<tr>
<td>14</td>
<td><p>POST</p>
<p>/dossiers/re-entries</p></td>
<td><p>Reentry flow: trigger BE to generate OTP and send to the user’s phone</p></td>
<td><p>Blank phone number (FAILED): 400</p>
<p>Dossier with given phone number does not exist: 404</p></td>
<td><p>Blank phone number (FAILED): 200 + error in response</p>
<p>Dossier with given phone number does not exist: 200 + error in response</p></td>
<td><p>3pts</p></td>
</tr>
<tr>
<td>15</td>
<td><p>POST</p>
<p>/dossiers/re-entries/{reEntryId}/verifications</p></td>
<td><p>Reentry flow: OTP verification</p></td>
<td><p>Any field is blank (FAILED): 400</p>
<p>ReEntryNotFoundException / DossierNotFoundException: 404</p></td>
<td><p>400 &amp; 404 cases change to 200 + error in response</p></td>
<td><p>5pts</p></td>
</tr>
<tr>
<td>16</td>
<td><p>PATCH</p>
<p>/dossiers/{dossierId}/completions</p></td>
<td><p>At the end of onboarding, replace the existing onboarding key with a new one for security</p></td>
<td><p>DossierNotFoundException: 404</p>
<p>OperationNotAllowedException (state in DB does not allow update onboarding key): 403</p></td>
<td><p>403: return 200 + error in response</p>
<p>404 change to 200 + error in response</p></td>
<td><p>3pts</p></td>
</tr>
<tr>
<td>17</td>
<td><p>GET</p>
<p>/dossiers/{dossierId}/states</p></td>
<td><p>Get requestId or state of the dossier for some logics on frontend</p></td>
<td><p>DossierNotFoundException: 404</p></td>
<td><p>DossierNotFoundException: 200 + error in response</p></td>
<td><p>3pts</p>
<p><span>Also used by Part Automisation</span></p></td>
</tr>
<tr>
<td>18</td>
<td><p>PATCH</p>
<p>/dossiers/{dossierId}/flows</p></td>
<td><p>Mark the flow (A or B) for the dossier for A/B Testing epic</p></td>
<td><p>DossierNotFoundException: 404 (dossier or dossierProgress not found)</p></td>
<td><p>DossierNotFoundException: 200 + error in response</p></td>
<td><p>2pts</p></td>
</tr>
</tbody>
</table>

</div>

# 2. Subtasks

1.  Endpoints 1-5: 8pts <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49025220678_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-6925" macro-id="026cfbb8-f4a4-4a4f-b6d8-3b52b0d521ae" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-6925" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-6925</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

2.  Endpoints 6-9: 8pts <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49025220678_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-6926" macro-id="5c14535a-79fe-46e7-b412-a611a4fd2af2" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-6926" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-6926</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

3.  Endpoints 10-12: 8pts <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49025220678_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-6980" macro-id="9beb7699-571e-477c-af2d-29f588957c59" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-6980" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-6980</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

4.  Endpoints 13-18: 8pts <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49025220678_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-6980" macro-id="fc4ac6cb-a1db-43d6-85d2-3450f5bd098e" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-6980" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-6980</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

5.  Sync changes to the Joint Account branch, **double check the endpoints for needed adjustments**, for example new error code like PARTNER_ACCOUNT_DUPLICATED_PERSON test & bug fixes for Joint Account: 10pts <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49025220678_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-6983" macro-id="3306390d-a279-4af5-9f40-7bcab1d36b6a" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-6983" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-6983</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

# 3. Notes

Estimates include:

- Adjust api specs (may skip)

- Adjust backend (Ivy code) + adapt for unit tests

- Adjust front end (cob-unattended-ui)

- Testing SOB for the related cases

In backend (Ivy), try to fix once in common at this place


![[49025220678-image-20260109-083353.png]]



In FE, many places need to be handled in the callers of this method


![[49025220678-image-20260109-085034.png]]

%% ai-graph-start %%

**Related notes:**
- [[404 addresses a missing resource; an empty filter result is a successful query]]
- [[Estimate for ivy and cob-unattended-business-dossier-service-api-spec]]
- [[Error handling for delete and undo]]
- [[Preview Delivery-Prices API - ForcedOnboading]]
- [[Public API - letterbox - API get deleted letters from trash]]

%% ai-graph-end %%