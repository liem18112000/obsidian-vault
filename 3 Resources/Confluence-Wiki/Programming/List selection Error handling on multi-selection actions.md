---
title: "[List selection] Error handling on multi-selection actions"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48718774383/List+selection+Error+handling+on+multi-selection+actions
space: "Helios"
topic: programming
relevance: 0.792
depth: 3
updated: 2026-01-30
attachments: 69
tags:
  - confluence
  - programming
  - space/helios
---

# [List selection] Error handling on multi-selection actions

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-01-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48718774383/List+selection+Error+handling+on+multi-selection+actions)
> Relevance 0.792 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="9685d9c1-e909-41e1-a2c7-5b864815fb8e" macro-name="toc">

</div>

**Related stories**

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-134710" macro-id="4034e787-141c-4a8a-8003-782d9f2d6cab" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-134710" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-134710</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-138835" macro-id="69c1011f-7599-4a8b-8696-343ff51f0589" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-138835" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-138835</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-138606" macro-id="339d4923-3288-4859-a949-4e2b5b7494bf" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-138606" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-138606</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-134603" macro-id="91fd7bd5-c2c5-4d81-b1d9-e58473f1a81e" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-134603" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-134603</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-141005" macro-id="4d2332e7-d589-49b2-8b63-e185a634bf8e" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-141005" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-141005</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-141186" macro-id="4cf6fc47-2498-4c7c-9d85-3e2d395f5cf5" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-141186" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-141186</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

- <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-140069" macro-id="7bd6e5cb-9d1f-440c-9c69-ac5b9eb467a5" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-140069" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-140069</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

### **I. Message List Update**

<div id="expander-252835506" class="expand-container conf-macro output-block" hasbody="true" macro-id="9717a248-fec9-490c-b55d-a055865734ab" macro-name="expand">

<div id="expander-control-252835506" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">1. Single Action: update messages</span>

</div>

<div id="expander-content-252835506" class="expand-content expand-hidden">

**Do actions:**

- Mark message as read / unread

- Delete messages

- Undo messages

**API Response:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b79661f0-5dac-4772-81d7-c69e64b0bac5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
HTTP Code: 200 (mark read/unread)
HTTP Code: 207 (delete/undo/hard-delete messages)
--------------
{
    notFoundDocuments: string[];
    permissionDeniedDocuments: string[];
    unknownErrorDocuments: string[];
}
```

</div>

</div>

<div>

<table>
<tbody>
<tr>
<th></th>
<th><p><strong>Use case</strong></p></th>
<th><p><strong>API response</strong></p></th>
<th><p><strong>UI show</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>

![[48718774383-2705.png]]

 [Success] All items succeed</p></td>
<td>

![[48718774383-ii_1_1_01-20251003-143341.png]]

</td>
<td>

![[48718774383-1_1-20251029-063800.png]]

</td>
<td><p>The same response when messages are already in the requested state (deleted, undone, marked read/unread)</p></td>
</tr>
<tr>
<td>2</td>
<td><p>

![[48718774383-error.png]]

 [Failed] All items failed due to business error</p></td>
<td></td>
<td rowspan="2">

![[48718774383-1_2-20251029-063604.png]]

</td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>

![[48718774383-error.png]]

 [Failed] All items failed due to HTTP error</p></td>
<td><p>Http code: 4xx, 5xx</p></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>

![[48718774383-warning.png]]

 [Partial Success] Some items succeeded, some failed</p></td>
<td>

![[48718774383-ii_1_4_01-20251003-142933.png]]

</td>
<td>

![[48718774383-1_4-20251029-063207.png]]

</td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="expander-1196221693" class="expand-container conf-macro output-block" hasbody="true" macro-id="8d380e9a-63bb-4515-8208-0d1167bd0fd9" macro-name="expand">

<div id="expander-control-1196221693" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">2. Combine actions: delete both messages and one draft</span>

</div>

<div id="expander-content-1196221693" class="expand-content expand-hidden">

**Do actions:**

- Delete drafts

**API Response:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1c50d775-6ef0-4e8b-8bcf-ce60f2bc157e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
HTTP Code: 207 (delete drafts)
--------------
{
    '207': Array<{
        groupId: string;
        updateStatus: {
            notFoundDocuments: string[];
            permissionDeniedDocuments: string[];
            unknownErrorDocuments: string[];
        }
    }>;
    otherStatus: Array<{
        groupId: string;
        statusCode: string; // e.g., 400, 500
    }>;
}
```

</div>

</div>

**Example: delete 3 messages and 1 draft**


![[48718774383-Screenshot 2025-10-29 at 16.10.16-20251029-091019.png]]



**Test cases:**

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Delete messages</strong></p></th>
<th><p><strong>Delete drafts</strong></p></th>
<th><p><strong>UI show</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td>

![[48718774383-1_2_1_0-20251029-083851.png]]

</td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td>

![[48718774383-1_2_2-20251029-083506.png]]

</td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td>

![[48718774383-1_2_3-20251029-083722.png]]

</td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td>

![[48718774383-1_2_4-20251029-083323.png]]

</td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td>

![[48718774383-1_2_5-20251029-083113.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>2 message(s) were deleted, 1 failed</code><strong>"</strong></p></li>
</ul></td>
</tr>
<tr>
<td>6</td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td>

![[48718774383-1_2_6-20251029-081952.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>2 message(s) were deleted, 1 failed</code><strong>"</strong></p></li>
<li><p>Toast 2 → <strong>"</strong><code>1 draft(s) could not be deleted</code><strong>" (+Retry)</strong></p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

### **II. Group List Update**

<div id="expander-376989683" class="expand-container conf-macro output-block" hasbody="true" macro-id="f30a7c9d-d91b-4ea5-bc83-c38a51a0f1a3" macro-name="expand">

<div id="expander-control-376989683" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">1. Single Action: update groups or drafts</span>

</div>

<div id="expander-content-376989683" class="expand-content expand-hidden">

**Do actions:**

- Mark groups as read

- Mark the last message of groups as unread

- Delete groups

- Undo groups

- Hard-delete groups

- Delete drafts

**API Response:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1c9d0b04-eee6-45c1-b5e2-e6918c03e276" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
HTTP Code: 207
--------------
{
    '207': Array<{
        groupId: string;
        updateStatus: {
            notFoundDocuments: string[];
            permissionDeniedDocuments: string[];
            unknownErrorDocuments: string[];
        }
    }>;
    otherStatus: Array<{
        groupId: string;
        statusCode: string; // e.g., 400, 500
    }>;
}
```

</div>

</div>

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Use case</strong></p></th>
<th><p><strong>API response</strong></p></th>
<th><p><strong>UI show</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>

![[48718774383-2705.png]]

 [Success] All items succeed</p></td>
<td>

![[48718774383-i_1_1_01-20251003-035318.png]]

</td>
<td>

![[48718774383-i_1_1_02-20251003-093421.png]]

</td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>

![[48718774383-error.png]]

 [Failed] All items failed due to business error</p></td>
<td>

![[48718774383-i_1_2_01-20251003-091947.png]]

</td>
<td rowspan="2">

![[48718774383-i_1_2_02-20251003-093515.png]]

</td>
<td><p>if one group has any document error =&gt; group has error =&gt; update group FAILED</p>
<p>ex: we have :</p>
<ul>
<li><p>group_A has 3 messages</p></li>
<li><p>group_B has 2 messages</p></li>
</ul>
<p>then delete 2 groups, if group_A has one message in <code>unknownErrorDocuments</code> =&gt; group_A failed (although 2 messages are deleted successful)<br />
</p>
<p>then, the toast show:</p>
<p>“<code>1 thread(s) were deleted permanently, 1 failed</code>“</p></td>
</tr>
<tr>
<td>3</td>
<td><p>

![[48718774383-error.png]]

 [Failed] All items failed due to HTTP error</p></td>
<td><p>Http code: 4xx, 5xx</p></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>

![[48718774383-warning.png]]

 [Partial Success] Some items succeeded, some failed</p></td>
<td>

![[48718774383-i_1_4_01-20251003-094647.png]]

</td>
<td>

![[48718774383-Screenshot 2025-10-30 at 13.48.09-20251030-064812.png]]

</td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="expander-1605151945" class="expand-container conf-macro output-block" hasbody="true" macro-id="85965708-4a3e-4627-843c-2597c2ffa193" macro-name="expand">

<div id="expander-control-1605151945" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">2. Combine actions: delete both groups and drafts</span>

</div>

<div id="expander-content-1605151945" class="expand-content expand-hidden">

**Do actions:**

- Delete groups with drafts

- Hard-delete groups with drafts

**Example: delete 3 groups and 2 drafts**


![[48718774383-Screenshot 2025-10-29 at 16.07.33-20251029-090735.png]]



**Test cases:**

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Delete groups</strong></p></th>
<th><p><strong>Delete drafts</strong></p></th>
<th><p><strong>UI show</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.06.48-20251029-090654.png]]

</td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.28.21-20251029-092824.png]]

</td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.46.22-20251029-094625.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>3 thread(s) were deleted</code><strong>" (+ Undo)</strong></p></li>
<li><p>Toast 2 → <strong>"</strong><code>1 draft(s) were deleted, 1 failed</code><strong>"</strong></p></li>
</ul></td>
</tr>
<tr>
<td>4</td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.43.42-20251029-094344.png]]

</td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.37.01-20251029-093704.png]]

</td>
<td></td>
</tr>
<tr>
<td>6</td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.44.58-20251029-094500.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>3 thread(s) thread(s) could not be deleted</code><strong>"</strong></p></li>
<li><p>Toast 2 → <strong>"</strong><code>1 draft(s) were deleted, 1 failed</code><strong>"</strong></p></li>
</ul></td>
</tr>
<tr>
<td>7</td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td><p>

![[48718774383-2705.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.41.42-20251029-094145.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>2 thread(s) were deleted, 1 failed</code><strong>" (+ Undo)</strong></p></li>
<li><p>Toast 2 → <code>“2 draft(s) were deleted"</code></p></li>
</ul></td>
</tr>
<tr>
<td>8</td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td><p>

![[48718774383-error.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.40.05-20251029-094010.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>2 thread(s) were deleted, 1 failed</code><strong>" (+ Undo)</strong></p></li>
<li><p>Toast 2 → <strong>"</strong><code>2 draft(s) could not be deleted</code><strong>" (+Retry)</strong></p></li>
</ul></td>
</tr>
<tr>
<td>9</td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td><p>

![[48718774383-warning.png]]

</p></td>
<td>

![[48718774383-Screenshot 2025-10-29 at 16.57.08-20251029-095714.png]]

</td>
<td><ul>
<li><p>Toast 1 → <strong>"</strong><code>2 thread(s) were deleted, 1 failed</code><strong>" (+ Undo)</strong></p></li>
<li><p>Toast 2 → <strong>"</strong><code>1 draft(s) were deleted, 1 failed</code><strong>"</strong></p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

### **III. Hard delete Letters**

- API’s response mentioned in the comment: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48718774383_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-145466" macro-id="8ad10198-6011-479b-826f-98f6c34a6395" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-145466" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-145466</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

  - API url : <a href="http://localhost:8080/luz_unified_inbox/api/6f277a45-b91a-4012-832e-215aacdd44ab/groups/hard-delete?type=ELETTER&amp;message-box=inbox" class="external-link" rel="nofollow">/luz_unified_inbox/api/6f277a45-b91a-4012-832e-215aacdd44ab/groups/hard-delete?type=ELETTER&amp;message-box=inbox</a>

  - The response body only include failed letter id.

  - example of request amt

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f40ea634-d23b-4929-a254-2e2bdd705bd0" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    curl --location --request DELETE 'http://localhost:8080/luz_unified_inbox/api/4656a299-90d2-49f9-9472-9ab27e9dfd14/groups/hard-delete?message-box=draft' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: ••••••' \
    --data '{"ELETTER":["692fc5051ab09d3e4e021f82", "456fc5051ab09d3e4e021f82"], "EMAIL":["123fc5051ab09d3e4e021f82"]}'
    ```

    </div>

    </div>

  - response atm:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c6dca269-91ca-4f11-b492-ecaa0c2348e4" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    {
        "groupUpdateStatuses": {
            "207": [
                {
                    "groupId": "692fc5051ab09d3e4e021f82",
                    "updateStatus": {
                        "notFoundDocuments": [],
                        "permissionDeniedDocuments": [],
                        "unknownErrorDocuments": [],
                        "failedHardDeleteDocuments": [
                            "692fc5051ab09d3e4e021f82"
                        ]
                    },
                    "statusCode": "500"
                },
                {
                    "groupId": "456fc5051ab09d3e4e021f82",
                    "updateStatus": {
                        "notFoundDocuments": [],
                        "permissionDeniedDocuments": [],
                        "unknownErrorDocuments": [],
                        "failedHardDeleteDocuments": [
                            "456fc5051ab09d3e4e021f82"
                        ]
                    },
                    "statusCode": "500"
                }
            ],
            "otherStatus": [  // this is the internal exception case
                {
                    "groupId": "456fc5051ab09d3e4e021f82",
                    "updateStatus": {
                        "notFoundDocuments": [],
                        "permissionDeniedDocuments": [],
                        "unknownErrorDocuments": [
                            "456fc5051ab09d3e4e021f82"
                        ],
                        "failedHardDeleteDocuments": []
                    },
                    "statusCode": "500"
                },
                {
                    "groupId": "123fc5051ab09d3e4e021f82",
                    "statusCode": "500"
                }
            ]
        }
    }
    ```

    </div>

    </div>

  - Server:

    - currently, we follow the existing implementation.

    - Later team might want to adapt the response body with this idea (basically, we will need to discuss and see what kind of error will be returned), as a common API we don’t support specific only for luz-next client

      <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6573953c-f390-4540-9fe2-c3566ddcb3ff" macro-name="code" style="border-width: 1px;">

      <div class="codeContent panelContent pdl">

      ``` syntaxhighlighter-pre
      [{
        "groupId": "692fc5051ab09d3e4e021f82",
        "updateStatus": {
            "notFoundDocuments": [],
            "permissionDeniedDocuments": [],
            "unknownErrorDocuments": [], // this list could contain soft delete failed, if we trigger the delete from inbox
            "failedHardDeleteDocuments": ["692fc5051ab09d3e4e021f82"] // this list only contain letterIds that hard delete failed 
            "maybe_internal_server_error": []
        },
        "groupId": "123fc5051ab09d3e4e021f82",
        "updateStatus": {
            "notFoundDocuments": [],
            "permissionDeniedDocuments": [],
            "unknownErrorDocuments": ["123fc5051ab09d3e4e021f82"], // this list could contain soft delete failed, if we trigger the delete from inbox
            "failedHardDeleteDocuments": []
        }
      },{
        ...
      }]
      ```

      </div>

      </div>

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
<th><p><strong>Hard - Delete groups</strong></p></th>
<th><p><strong>UI show</strong></p></th>
<th><p><strong>Note</strong></p></th>
<th><p><strong>Final decision</strong></p></th>
<th><p><strong>Server response</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p><strong>Hard-Delete a single letter</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2: success</p></li>
</ul></td>
<td></td>
<td><p><br />
<strong>show success toast</strong></p></td>
<td><p>[Success] <code>{numberOfItems} thread(s) were deleted permanently</code></p></td>
<td><p>http response: 207</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cd9bdac2-8362-4dab-809a-5799856bb8a7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;groupUpdateStatuses&quot;: {}
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td>2</td>
<td><p><strong>Hard-Delete a single letter ( fail)</strong></p>
<ul>
<li><p>#1: fail</p></li>
</ul></td>
<td></td>
<td><p><strong>show a general fail toast + Retry button</strong></p></td>
<td><p>[Error] <code>{numberOfItems} thread(s) could not be deleted permanently</code> <strong>(+Retry)</strong></p></td>
<td><p>http response: 207</p>
<p>since we use the same API, reference to response from another example below</p></td>
</tr>
<tr>
<td>3</td>
<td><p><strong>Hard-Delete a single letter (partially fail)</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2: fail</p></li>
</ul></td>
<td>

![[48718774383-Screenshot 2026-01-29 at 17.12.28.png]]

</td>
<td><ul>
<li><p><strong>show Fail toast with message</strong> : “we could not permanently delete the message for you but it is in the Trash now and will be deleted in 30 days”.</p></li>
<li><p><strong>item will be in Trash Folder.</strong></p></li>
</ul></td>
<td><p>[Partial] <code>We could not permanently delete the message for you but it is in the Trash now and will be deleted in 30 days</code></p>
<ul>
<li><p><strong>item will be in Trash Folder.</strong></p></li>
</ul></td>
<td><p>http response: 207</p>
<p>since we use the same API, reference to response from another example below</p></td>
</tr>
<tr>
<td>4</td>
<td><p><strong>Hard-Delete 2 items in Inbox</strong></p>
<ul>
<li><p>item1 : <strong>success</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2 : success</p></li>
</ul></li>
<li><p>items 2: fail hard delete <strong>(view in trash)</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2: fail</p></li>
</ul></li>
</ul></td>
<td>

![[48718774383-Screenshot 2026-01-29 at 17.12.28.png]]

</td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a> <span class="inline-comment-marker" data-ref="50fcac5a-381e-42ca-938f-b7fc7a70a338">Which message will we show?</span></p></td>
<td><p>[Partial] <code>We could not permanently delete the message for you but it is in the Trash now and will be deleted in 30 days</code></p></td>
<td><p>http response: 207</p>
<p>since we use the same API, reference to response from another example below</p></td>
</tr>
<tr>
<td>5</td>
<td><p><strong>Hard-Delete 2 items in Inbox</strong></p>
<ul>
<li><p>item1 : <strong>success</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2 : success</p></li>
</ul></li>
<li><p>items 2: fail soft delete <strong>(still view in inbox)</strong></p>
<ul>
<li><p>#1: fail</p></li>
</ul></li>
</ul></td>
<td>

![[48718774383-Screenshot 2026-01-29 at 17.46.28.png]]

</td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a> <span class="inline-comment-marker" data-ref="df55ad17-7b72-454d-8672-f7e5423bfb95">Which message will we show?</span></p></td>
<td><p>[Partial] <code>Some of items could not be hard deleted.</code></p></td>
<td><p>http response: 207</p>
<p>since we use the same API, reference to response from another example below</p></td>
</tr>
<tr>
<td>6</td>
<td><p><strong>Hard-Delete 3 items in Inbox</strong></p>
<ul>
<li><p>item1 : <strong>success</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2 : success</p></li>
</ul></li>
<li><p>items 2: fail soft delete <strong>(still view in inbox)</strong></p>
<ul>
<li><p>#1: fail</p></li>
</ul></li>
<li><p>item 3: fail hard-delete  <strong>(view in trash)</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2: fail</p></li>
</ul></li>
</ul></td>
<td>

![[48718774383-Screenshot 2026-01-29 at 17.46.28.png]]

</td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a> <span class="inline-comment-marker" data-ref="7f50c8a1-9fd4-4dd2-b919-c0c412949dcc">Which message will we show?</span></p></td>
<td><p>[Partial] <code>Some of items could not be hard deleted.</code></p></td>
<td><p>http response: 207</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="6f233360-4b38-4a9c-afe5-65d0135fe827" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;groupUpdateStatuses&quot;: {
        &quot;207&quot;: [
            {
                &quot;groupId&quot;: &quot;123fc5051ab09d3e4e021f82&quot;,
                &quot;updateStatus&quot;: {
                    &quot;notFoundDocuments&quot;: [],
                    &quot;permissionDeniedDocuments&quot;: [],
                    &quot;unknownErrorDocuments&quot;: [&quot;123fc5051ab09d3e4e021f82&quot;],
                    &quot;failedHardDeleteDocuments&quot;: []
                }
            },{
                &quot;groupId&quot;: &quot;692fc5051ab09d3e4e021f82&quot;,
                &quot;updateStatus&quot;: {
                    &quot;notFoundDocuments&quot;: [],
                    &quot;permissionDeniedDocuments&quot;: [],
                    &quot;unknownErrorDocuments&quot;: [],
                    &quot;failedHardDeleteDocuments&quot;: [
                        &quot;692fc5051ab09d3e4e021f82&quot;
                    ]
                }
            }
        ]      
    }
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td>7</td>
<td><p><strong>Hard-Delete 2 items in Inbox</strong></p>
<ul>
<li><p>item1 : fail soft delete <strong>(still view in inbox)</strong></p>
<ul>
<li><p>#1: fail</p></li>
</ul></li>
<li><p>items 2: fail hard delete <strong>(view in trash)</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2: fail</p></li>
</ul></li>
</ul></td>
<td>

![[48718774383-Screenshot 2026-01-29 at 17.28.57-20260129-102902.png]]

</td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a> <span class="inline-comment-marker" data-ref="139116fb-427e-4da1-af97-e02995113afd">Which message will we show?</span></p></td>
<td><p>[Error] <code>{numberOfItems} thread(s) could not be deleted permanently</code> <strong>(+Retry)</strong></p></td>
<td><p>http response: 207</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7facb292-f5eb-4ab5-b082-da769579c887" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;groupUpdateStatuses&quot;: {
        &quot;207&quot;: [
            {
                &quot;groupId&quot;: &quot;123fc5051ab09d3e4e021f82&quot;,
                &quot;updateStatus&quot;: {
                    &quot;notFoundDocuments&quot;: [],
                    &quot;permissionDeniedDocuments&quot;: [],
                    &quot;unknownErrorDocuments&quot;: [&quot;123fc5051ab09d3e4e021f82&quot;],
                    &quot;failedHardDeleteDocuments&quot;: []
                }
            },{
                &quot;groupId&quot;: &quot;692fc5051ab09d3e4e021f82&quot;,
                &quot;updateStatus&quot;: {
                    &quot;notFoundDocuments&quot;: [],
                    &quot;permissionDeniedDocuments&quot;: [],
                    &quot;unknownErrorDocuments&quot;: [],
                    &quot;failedHardDeleteDocuments&quot;: [
                        &quot;692fc5051ab09d3e4e021f82&quot;
                    ]
                }
            }
        ]      
    }
}</code></pre>
</div>
</div></td>
</tr>
<tr>
<td>8</td>
<td><p><strong>Hard-Delete 2 items in Trash</strong></p>
<ul>
<li><p>item1 : <strong>success</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2 : success</p></li>
</ul></li>
<li><p>items 2: fail hard delete <strong>(still view in trash)</strong></p>
<ul>
<li><p>#1: success</p></li>
<li><p>#2: fail</p></li>
</ul></li>
</ul></td>
<td>

![[48718774383-Screenshot 2026-01-29 at 17.46.28.png]]

</td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a> <span class="inline-comment-marker" data-ref="3639e262-9211-41e6-89ca-3053022da44b">Which message will we show?</span></p></td>
<td><p>[Partial] <code>Some of items could not be hard deleted.</code></p></td>
<td><p>http response: 207</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1a60b063-8dd4-463e-915d-618984ab8c4d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;groupUpdateStatuses&quot;: {
        &quot;207&quot;: [
            {
                &quot;groupId&quot;: &quot;692fc5051ab09d3e4e021f82&quot;,
                &quot;updateStatus&quot;: {
                    &quot;notFoundDocuments&quot;: [],
                    &quot;permissionDeniedDocuments&quot;: [],
                    &quot;unknownErrorDocuments&quot;: [&quot;692fc5051ab09d3e4e021f82&quot;],
                    &quot;failedHardDeleteDocuments&quot;: []
                }
            }
        ]
    }
}</code></pre>
</div>
</div></td>
</tr>
</tbody>
</table>

</div>
