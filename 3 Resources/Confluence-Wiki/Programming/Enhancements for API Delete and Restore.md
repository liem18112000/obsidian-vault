---
ai_hash: 804f4718de576d1d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.85
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47169732666/Enhancements+for+API+Delete+and+Restore
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Enhancements for API Delete and Restore
topic: programming
type: source
updated: 2022-11-01
---

# Enhancements for API Delete and Restore

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-11-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47169732666/Enhancements+for+API+Delete+and+Restore)
> Relevance 0.85 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="dde212ee3fd3d9e9deac5307f9e7a677" macro-name="toc">

</div>


![[47169732666-image-20220830-015341.png]]



## 1. Delete

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
<th></th>
<th><p><strong>Decision with Kepler</strong></p></th>
<th><p><strong>Our implement</strong></p></th>
<th><p><strong>Status</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Delete folder API</p>
<p><em><span>DELETE</span></em></p>
<p><em><span>localhost:12080/luz_docs_view_controller/api/:tenantid/archives/directories/:directoryId?is-permanent=false&amp;is-detail-response=true</span></em></p></td>
<td><p>Team Kepler enhance to return</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="35afbc83-669d-418f-8aba-94e809bc7a4c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  folderIds deleted: [&quot;folderId&quot;],
  letterIds deleted: [&quot;letterId&quot;],
  letterIds removed with parent folder id: [
      {“letterId1“, “folderId1“}, 
      {“letterId2“, “folderId2“}
    ]
}</code></pre>
</div>
</div></td>
<td><p>We get that list response from delete API to update history for docs/folders</p>
<ol>
<li><p>Call update history for list folder deleted</p></li>
<li><p>Call update history for list letter deleted</p></li>
<li><p>Call update history for list letter removed</p></li>
</ol></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="893265fc-bb96-4cc9-9e1e-2d0188f0f530" data-macro-name="status">ENHANCED</span></p></td>
</tr>
<tr>
<td>2</td>
<td><p>Update history multiple letters deleted</p>
<p><span>PATCH</span></p>
<p><a href="http://localhost:12080/luz_docs/api/:tenant-id/documents" class="external-link" rel="nofollow"><span>http://localhost:12080/luz_docs/api/:tenant-id/documents</span></a></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="45cdca1c-6171-4d34-b808-8b64f390f286" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>BODY
{
    &quot;entries&quot;: [&quot;63032511454beb7c4f0a7202&quot; ,&quot;62e7754ed5bbe9363a7f98e9&quot;],
    &quot;patchQuery&quot;: [
        {
            &quot;op&quot;: &quot;add&quot;,
            &quot;path&quot;: &quot;/deletingHistoryEntries/-&quot;,
            &quot;value&quot;: [
                {
                    &quot;byUser&quot;: &quot;khoa.tang@axonactive.com&quot;,
                    &quot;atTime&quot;: &quot;2022-08-24T09:52:17.373504Z&quot;,
                    &quot;fromDirectoryIds&quot;: []
                }
            ]
        }
    ]
}</code></pre>
</div>
</div></td>
<td><p>If success</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f185ba19-37df-42fb-9aab-baa1c6e70c18" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>1. response 200
2. {
    &quot;failedEntries&quot;: []
}</code></pre>
</div>
</div>
<p>Else</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="abbf546d-f59d-4c6b-9397-48ae474298bf" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>1. response 207
2. {
    &quot;failedEntries&quot;: [
        {
            &quot;62d8f1cc4e57df7d13be5ef1&quot;: 400
        },
        {
            &quot;6245260aefcb7d2c3200f94f&quot;: 400
        },
        {
            &quot;62d8f2024e57df7d13be5f91&quot;: 500
        }
    ]
}</code></pre>
</div>
</div>
<p>Note: <code>failedEntries </code>is a list letterId update history fail with the response status for each letter</p></td>
<td><p>If response 200</p>
<p>→ end call</p>
<p>else</p>
<p>→ We re-call this API for list letters fail which is the status 500</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="278b8476-0f75-4ac6-80ff-42e3fc199700" data-macro-name="status">ENHANCED</span></p></td>
</tr>
<tr>
<td>3</td>
<td><p>Update history multiple folders deleted</p>
<p><span>PATCH</span></p>
<p><a href="http://localhost:12080/luz_docs/api/:tenant-id/folders" class="external-link" rel="nofollow">http://localhost:12080/luz_docs/api/:tenant-id/folders</a></p></td>
<td><p>If success</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b646527d-58d6-408b-85b6-74cb09db86d9" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>1. response 200
2. {
    &quot;failedEntries&quot;: []
}</code></pre>
</div>
</div>
<p>Else</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b0c7d274-89d6-4fb8-827f-c068af7c0103" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>1. response 207
2. {
    &quot;failedEntries&quot;: [
        {
            &quot;62d8f1cc4e57df7d13be5ef1&quot;: 400
        },
        {
            &quot;6245260aefcb7d2c3200f94f&quot;: 400
        },
        {
            &quot;62d8f2024e57df7d13be5f91&quot;: 500
        }
    ]
}</code></pre>
</div>
</div>
<p>Note: <code>failedEntries </code>is a list folderId update history fail with the response status for each letter</p></td>
<td><p>If response 200</p>
<p>→ end call</p>
<p>else</p>
<p>→ We re-call this API for list folders fail which is the status 500</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="051b8bf6-8805-4b8b-8475-268cd0792aa8" data-macro-name="status">ENHANCED</span></p></td>
</tr>
<tr>
<td>4</td>
<td><p>Update history for list letters removed</p></td>
<td></td>
<td><p>Loop to call API update remove history for each letter</p>
<p>if response 200 → continue</p>
<p>else → re-call for each letter</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="a0f0a19d-a416-441a-a9d9-bd36969f69ca" data-macro-name="status">OK</span></p></td>
</tr>
</tbody>
</table>

</div>

## 2. Restore

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
<th></th>
<th></th>
<th><p><strong>Decision with Kepler</strong></p></th>
<th><p><strong>Our implement</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Restore folder API</p>
<p><em><span>POST</span></em></p>
<p><em><span>localhost:12080/luz_docs_view_controller/api/:tenantid/trash/directories/{directoryId}/recovery</span></em></p></td>
<td><p>Team Kepler enhance to return</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a6e9bb03-daf2-4e53-b8cd-c83d7f45bb63" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  folderIds restored: [&quot;folderId&quot;],
  letterIds restored: [&quot;letterId&quot;]
}</code></pre>
</div>
</div></td>
<td><p>We get that list response from restore API to update history for docs/folders</p>
<ol>
<li><p>Call update history for list folder restored</p></li>
<li><p>Call update history for list letter restored</p></li>
</ol></td>
</tr>
<tr>
<td>2</td>
<td><p>Update history multiple letters restored</p>
<p><span>PATCH</span></p>
<p><a href="http://localhost:12080/luz_docs/api/:tenant-id/documents" class="external-link" rel="nofollow"><span>http://localhost:12080/luz_docs/api/:tenant-id/documents</span></a></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8ad02a9c-7f5e-48ac-a58e-d22c4d98f6c9" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>BODY
{
    &quot;entries&quot;: [&quot;63032511454beb7c4f0a7202&quot; ,&quot;62e7754ed5bbe9363a7f98e9&quot;],
    &quot;patchQuery&quot;: [
        {
            &quot;op&quot;: &quot;add&quot;,
            &quot;path&quot;: &quot;/storageHistoryEntries/-&quot;,
            &quot;value&quot;: [
                {
                    &quot;action&quot;: &quot;RESTORE&quot;,
                    &quot;sourceDirectoryId&quot;: &quot;&quot;,
                    &quot;destinationDirectoryIds&quot;: [
                        &quot;624526daefcb7d2c3200f9c9&quot;,
                        &quot;62d8f1cc4e57df7d13be5ef1&quot;
                    ],
                    &quot;atTime&quot;: &quot;2022-07-21T06:32:15.099106Z&quot;,
                    &quot;byUser&quot;: &quot;khoa.tang@axonactive.com&quot;,
                    &quot;modificationId&quot;: &quot;RESTORE_1658385135097&quot;,
                    &quot;fromStorageLocation&quot;: &quot;TRASH&quot;,
                    &quot;toStorageLocation&quot;: &quot;FOLDER&quot;
                }
            ]
        }
    ]
}</code></pre>
</div>
</div></td>
<td><p>If success</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="32dcb14b-3a5b-47eb-8afe-34cb0522327c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>1. response 200
2. {
    &quot;failedEntries&quot;: []
}</code></pre>
</div>
</div>
<p>Else</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="de44bbfa-7961-4a2b-ad43-3c4199a28fd1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>1. response 207
2. {
    &quot;failedEntries&quot;: [
        {
            &quot;62d8f1cc4e57df7d13be5ef1&quot;: 400
        },
        {
            &quot;6245260aefcb7d2c3200f94f&quot;: 400
        },
        {
            &quot;62d8f2024e57df7d13be5f91&quot;: 500
        }
    ]
}</code></pre>
</div>
</div>
<p>Note: <code>failedEntries </code>is a list letterId update history fail with the response status for each letter</p></td>
<td><p>If response 200</p>
<p>→ end call</p>
<p>else</p>
<p>→ We re-call this API for list letters fail which is the status 500</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Update history multiple folders restored</p>
<p><span>PATCH</span></p>
<p><a href="http://localhost:12080/luz_docs/api/:tenant-id/folders" class="external-link" rel="nofollow">http://localhost:12080/luz_docs/api/:tenant-id/folders</a></p></td>
<td><ol>
<li><p>Restore into folder</p>
<ol>
<li><p>Call update restore history for main folder</p></li>
<li><p>Call update restore history for all sub-folder</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="631d1bec-77b5-4f9d-ad6d-ab4d898c69c8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>If success return
  1. response 200
  2. {
      &quot;failedEntries&quot;: []
  }
&#10;else
  1. response 207
  2. {
      &quot;failedEntries&quot;: [
          {
              &quot;62d8f1cc4e57df7d13be5ef1&quot;: 400
          },
          {
              &quot;6245260aefcb7d2c3200f94f&quot;: 400
          },
          {
              &quot;62d8f2024e57df7d13be5f91&quot;: 500
          }
      ]
  }</code></pre>
</div>
</div></li>
</ol></li>
<li><p>Restore into root → do 1.b)</p></li>
</ol>
<p>Note: <code>failedEntries </code>is a list folderId update history fail with the response status for each letter</p></td>
<td><p>1.a)</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cd1dbc91-0adf-4a75-9611-afaa90458f27" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>If response 200 -&gt; end
else -&gt; re-try</code></pre>
</div>
</div>
<p>1.b)</p>
<p>We re-call this API for list folder fail which is the status 500</p></td>
</tr>
</tbody>
</table>

</div>

## 3. Allow users can Delete/Restore

- Just allow the admin user first

- The case allows a user can see all entries inside the folder → Discuss again if it’s really needed

## 4. Re-try solution

Branch: <a href="https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/LUZ-84004/research-retry-solution" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/LUZ-84004/research-retry-solution</a>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-146746 Use correct API's for delete, restore and their undo]]
- [[Research on bulk removal of access class]]
- [[Empty Trash APIs]]
- [[WIP Analyze subfolder search API (542ms)]]
- [[Add Remove security class for folder]]

%% ai-graph-end %%