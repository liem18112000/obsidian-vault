---
ai_hash: 640aad24e1cfdfe4
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 20
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49699291139/CROSS-TEST+LUZ-159442+Implement+real+ZIP+download+for+eArchive+folders
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: '[CROSS-TEST] [LUZ-159442] Implement real ZIP download for eArchive folders'
topic: programming
type: source
updated: 2026-08-27
---

# [CROSS-TEST] [LUZ-159442] Implement real ZIP download for eArchive folders

> [!info] Imported from Confluence
> Space **TS** · updated 2026-08-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49699291139/CROSS-TEST+LUZ-159442+Implement+real+ZIP+download+for+eArchive+folders)
> Relevance 0.786 · topic `programming`

Related US: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49699291139_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-159442" macro-id="1de6acd7-d3c5-4900-8caf-60e97394e397" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-159442" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-159442</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

Scope: both **business** (LUZ-159559) and **private** (LUZ-159798) tenants are in scope of this one story — no sibling clone ticket. Regression neighbour: <a href="https://axonivy.atlassian.net/browse/LUZ-159132" class="external-link" rel="nofollow">LUZ-159132</a> (eArchive frontend enhancements part 2), where this defect was found.

<div hasbody="true" macro-id="66538b36-f6b2-445b-9d36-d2bdee57bc01" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**PO decisions from the comment thread — these override the written description:** (1) The subtree is capped at **100 documents**; over the cap the download is refused with an error toast — this cap is not in the original spec. (2) The ZIP must **preserve the nested folder structure** (Web1 V1 parity), not a flat list. (3) Downloading a folder **from the Trash** was raised as "nice to have" and **has been implemented** — it is tested here. (4) The action menu for **branded folders** is deliberately **skipped**; a follow-up story will add it after go-live — so "no menu on a branded folder" is the expected result, not a defect.

</div>

</div>

## **1. TEST REPORT**

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Test steps</strong></p></th>
<th><p><strong>Expected result</strong></p></th>
<th><p><strong>Actual</strong></p></th>
<th><p><strong>Attachment</strong></p></th>
<th><p><strong>Status</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Download from the nav tree context menu</p></td>
<td><ol>
<li><p>Login with Business tenant + eArchive.</p></li>
<li><p>In the left nav sidebar, right-click (or open the "…" menu of) a user folder holding 3–5 documents → <strong>Download</strong>.</p></li>
<li><p>Open the saved file with a ZIP tool.</p></li>
</ol></td>
<td><p>A real, openable ZIP is saved (not a few-byte text placeholder).</p>
<p>It contains the folder's documents, each of which opens correctly. The file is named after the folder, e.g. <code>Invoices.zip</code>.</p></td>
<td><p>Downloaded successfully and can be opened</p></td>
<td><div id="expander-102413191" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="198d912f-2bf2-4883-8988-c868f1ab56f4" data-macro-name="expand">
<div id="expander-control-102413191" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Screenshot</span>
</div>
<div id="expander-content-102413191" class="expand-content expand-hidden">

![[49699291139-image-20260826-095914.png]]


</div>
</div>
<div id="expander-1421337879" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d0e7162f-61ad-4efb-a272-4bf31c13ffef" data-macro-name="expand">
<div id="expander-control-1421337879" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1421337879" class="expand-content expand-hidden">

![[49699291139-image-20260826-095954.png]]


</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Download from the Storage list row and tile</p></td>
<td><ol>
<li><p>On the same folder, use the <strong>row kebab "…" → Download</strong>. Repeat via the <strong>tile view</strong> folder menu → Download.</p></li>
</ol></td>
<td><p>Both menus offer Download, and both produce the identical ZIP as the tree entry point — same contents, same filename. No entry point is missing the item.</p></td>
<td rowspan="5"><p>Ditto</p></td>
<td><div id="expander-730573633" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="dbca55a0-e36b-4e47-a64b-728090573ca0" data-macro-name="expand">
<div id="expander-control-730573633" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">List view</span>
</div>
<div id="expander-content-730573633" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_TLJRBX8zFT.mp4">chrome_TLJRBX8zFT.mp4</a></span>
</div>
</div>
<div id="expander-819029944" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="dacec5a0-2df4-46b1-a01c-29c6e930544a" data-macro-name="expand">
<div id="expander-control-819029944" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Tile view</span>
</div>
<div id="expander-content-819029944" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_5fF7WmFOMe.mp4">chrome_5fF7WmFOMe.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Download from a Recent-screen folder tile</p></td>
<td><ol>
<li><p>Open the Recent screen.</p></li>
<li><p>On a top-level folder tile in the Folders section, open its menu → <strong>Download</strong>.</p></li>
</ol></td>
<td><p>The download is available and yields the same valid ZIP as the other surfaces.</p></td>
<td><div id="expander-2064379106" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="0539e3f7-a902-4138-9a8d-cfdfd77d76fa" data-macro-name="expand">
<div id="expander-control-2064379106" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Recent tab</span>
</div>
<div id="expander-content-2064379106" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_yUgqJR1rTJ.mp4">chrome_yUgqJR1rTJ.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>4</td>
<td><p>Nested folders — content and structure</p></td>
<td><p>Build folder A → subfolder B → subfolder C, with documents in A, B and C. Download <strong>A</strong> and inspect the archive's internal paths.</p></td>
<td><p>Documents from A, B <em>and</em> C are all present, and the archive <strong>reproduces the folder hierarchy</strong> (A/B/… paths inside the ZIP) — not one flat list. PO decision: nested structure is required.</p></td>
<td><div id="expander-1524683238" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="4e483bbd-5063-468c-9ef6-5d8d8b64737f" data-macro-name="expand">
<div id="expander-control-1524683238" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Nested folders</span>
</div>
<div id="expander-content-1524683238" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_KOjYY3wYBH.mp4">chrome_KOjYY3wYBH.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>5</td>
<td><p>Folder holding exactly one document</p></td>
<td><p>Create a folder with a single document → Download.</p></td>
<td><p>Still saved as <code>&lt;folderName&gt;.zip</code> containing that one document — <em>not</em> the bare document file. (Differs on purpose from selecting one document in the list, which downloads directly.)</p></td>
<td><div id="expander-226537385" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="329619ff-7150-4b1e-b5fb-60473bec2df1" data-macro-name="expand">
<div id="expander-control-226537385" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-226537385" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_xJcX1yjQiI.mp4">chrome_xJcX1yjQiI.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>6</td>
<td><p>Download a folder from the Trash</p></td>
<td><ol>
<li><p>Move a folder containing documents (incl. a subfolder) to the Trash.</p></li>
<li><p>Open the Trash screen, open the trashed folder's menu → <strong>Download</strong>.</p></li>
</ol></td>
<td><p>Download <strong>is</strong> offered in the Trash (Rename and Move are not) and produces a valid ZIP with the trashed folder's documents, structure preserved. The folder stays in the Trash — downloading does not restore or purge it.</p></td>
<td><div id="expander-2076266314" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6c626371-d9c2-44c1-9230-faf53afa3b5f" data-macro-name="expand">
<div id="expander-control-2076266314" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-2076266314" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_itDQQSe5xy.mp4">chrome_itDQQSe5xy.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>7</td>
<td><p>Empty folder</p></td>
<td><p>Create a folder with no documents (and no documents anywhere in its subtree) → Download.</p></td>
<td><p><strong>No file is saved at all.</strong> An error toast appears: "This folder has no documents to download." No 0-byte or few-byte .zip lands in the Downloads folder.</p></td>
<td><p>As expectation</p></td>
<td><div id="expander-965743224" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="21162057-cf6f-4864-8851-02129fd734c6" data-macro-name="expand">
<div id="expander-control-965743224" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-965743224" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_xWNx0hLl4z.mp4">chrome_xWNx0hLl4z.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>8</td>
<td><p>Subtree over the 100-document cap</p></td>
<td><p>Prepare a folder whose subtree holds <strong>more than 100</strong> documents (spread across subfolders) → Download.</p></td>
<td><p>No ZIP is saved. Error toast: "This folder and its subfolders contain more than <strong>100</strong> documents and cannot be downloaded at once. Open the folder and download the documents you need individually."</p></td>
<td></td>
<td><div id="expander-222827907" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="61aaa5b8-a6b3-431d-ba31-6f218ff733e4" data-macro-name="expand">
<div id="expander-control-222827907" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-222827907" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_Zd9nNPtQBw.mp4">chrome_Zd9nNPtQBw.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>9</td>
<td><p>Boundary: exactly 100 documents</p></td>
<td><p>Folder subtree with exactly <strong>100</strong> documents → Download.</p></td>
<td><p>Download proceeds normally, and the ZIP contains all 100 documents — the cap rejects only <em>above</em> the limit, no off-by-one.</p></td>
<td></td>
<td><div id="expander-1067949695" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="652fb5df-ae1c-4474-bcc9-cf11efb05838" data-macro-name="expand">
<div id="expander-control-1067949695" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1067949695" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_l7rhr0wLHK.mp4">chrome_l7rhr0wLHK.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>10</td>
<td><p>Second download while one is running</p></td>
<td><ol>
<li><p>Start a folder download of a large folder</p></li>
<li><p>While the progress panel is still visible, trigger Download again (same or another folder, or a multi-document selection).</p></li>
</ol></td>
<td><p>The second click is refused with the toast "A download is already running. Please wait for it to finish." The first download continues unaffected and completes normally — the click is not silently swallowed.</p></td>
<td></td>
<td><div id="expander-1182628115" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="4ff7f1e9-0a7d-48f4-b00d-9d510cf928e3" data-macro-name="expand">
<div id="expander-control-1182628115" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1182628115" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_3w8CxL0DRE.mp4">chrome_3w8CxL0DRE.mp4</a></span>
</div>
</div>
<div data-hasbody="true" data-macro-id="70f965f9-90d0-4000-9245-e8e469be74e1" data-macro-name="note">
<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>
<div>
<p>If we reload the page while zipping file, the previous request zipping and downloading will be killed</p>
</div>
</div>
<div id="expander-1561529333" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="89e8511a-992d-47c6-b386-a57181ba1298" data-macro-name="expand">
<div id="expander-control-1561529333" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Example</span>
</div>
<div id="expander-content-1561529333" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_ZWat0LgoW9.mp4">chrome_ZWat0LgoW9.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>11</td>
<td><p>Backend failure while preparing / streaming</p></td>
<td><p>Force a failure (DevTools → Network offline mid-transfer, or block the <code>/api/earchive/downloads/…</code> request) → Download a folder.</p></td>
<td><p>Error toast "Couldn't prepare the download. Please try again." The progress panel closes. <strong>No corrupt or truncated .zip is left behind.</strong> Retrying afterwards with the network restored succeeds.</p></td>
<td></td>
<td><div id="expander-1091128349" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="b44bf1a4-276f-4cee-8b56-8e2b006829ce" data-macro-name="expand">
<div id="expander-control-1091128349" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1091128349" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_VA0zXjVgG1.mp4">chrome_VA0zXjVgG1.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>12</td>
<td><p>Partial drop — some documents unavailable</p></td>
<td><ol>
<li><p>Open 2 browsers with 2 different accounts of the same company</p></li>
<li><p>User 1: Download a folder with many documents</p></li>
<li><p>User 2: Delete some documents in a second session</p></li>
</ol></td>
<td><p>The ZIP is still saved successfully</p></td>
<td></td>
<td><div id="expander-1161534625" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6dcd3cb0-92da-4bb8-96ba-225775e90202" data-macro-name="expand">
<div id="expander-control-1161534625" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1161534625" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_XFAOhzSkxa.mp4">chrome_XFAOhzSkxa.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>13</td>
<td><p>Filename sanitization</p></td>
<td><ol>
<li><p>Create folders named with characters a filesystem rejects, e.g. <code>Q1/Q2: Rechnungen?</code>, and one with a trailing space or dot.</p></li>
<li><p>Download each.</p></li>
</ol></td>
<td><p>The saved filename replaces illegal characters with _ (e.g., <code>Q1_Q2_ Rechnungen_.zip</code>), has no trailing dot/space, and opens on Windows without a rename. Folder name in the app itself is unchanged.</p></td>
<td></td>
<td><div id="expander-345677232" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="92bb6a35-4721-499e-a725-7b7f7cc69a98" data-macro-name="expand">
<div id="expander-control-345677232" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-345677232" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_vYDWobfoGH.mp4">chrome_vYDWobfoGH.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>14</td>
<td><p>Branded folder — no action menu (by PO decision)</p></td>
<td><p>Right-click / open "…" on a branded folder in the tree.</p></td>
<td><p>Still <strong>no</strong> action menu and therefore no Download — this is the agreed behaviour for now (follow-up story after go-live), not a defect. Raise it only if a menu <em>does</em> appear.</p></td>
<td><p>No action for Branded folder</p></td>
<td><div id="expander-1346212156" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="174b68ea-8ba7-469c-b81f-f810f246cfec" data-macro-name="expand">
<div id="expander-control-1346212156" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1346212156" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_39aLfdLHxX.mp4">chrome_39aLfdLHxX.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>15</td>
<td><p>Private tenant parity</p></td>
<td><p>Log in as a <strong>private</strong> tenant and repeat the core cases: tree download, Ablage list/tile download, nested folder, empty folder, over-cap, Trash folder.</p></td>
<td><p>Identical behaviour to the business tenant — valid nested ZIP, same toasts, same cap. Private tenants are explicitly in scope per the PO.</p></td>
<td><p>Private works similar with Business</p></td>
<td><div id="expander-1130941888" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f4901845-f276-44b2-89f3-4d597bfb7a25" data-macro-name="expand">
<div id="expander-control-1130941888" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1130941888" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_Er6fHxBlSz.mp4">chrome_Er6fHxBlSz.mp4</a></span>
</div>
</div>
<div data-hasbody="true" data-macro-id="180da75b-6278-47de-b14a-6279970a04dd" data-macro-name="info">
<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>
<div>
<p>Trash has not implemented yet</p>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>16</td>
<td><p>Subscription / grace-period handling</p></td>
<td><p>On a tenant in the grace period and one without an eArchive subscription, open a folder's action menu and attempt Download.</p></td>
<td><p>Folder Download follows the same enabled/disabled + disabled-reason handling as the other folder actions (rename/move/delete) in that state — it is not a new, ungated way out of the restriction.</p></td>
<td><ul>
<li><p>Could only download folder</p></li>
</ul>
<ul>
<li><p>Other actions are disabled</p></li>
</ul></td>
<td></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
<tr>
<td>17</td>
<td><p>Regression — document downloads unchanged</p></td>
<td><p>Download a <strong>single</strong> document from the list, then a <strong>multi-selection</strong> of documents via the bulk action bar. Also check the folder tree/Ablage after each folder download and watch the browser console throughout.</p></td>
<td><p>Single document still downloads directly (no ZIP); multi-selection still produces the bulk ZIP as before. The folder tree, folder counts, and document list are unchanged by a download</p></td>
<td></td>
<td><div id="expander-1558558622" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="346b8ec2-4130-458b-934b-3f7d054b5720" data-macro-name="expand">
<div id="expander-control-1558558622" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Result</span>
</div>
<div id="expander-content-1558558622" class="expand-content expand-hidden">
<span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49699291139-chrome_rnISqSMlRn.mp4">chrome_rnISqSMlRn.mp4</a></span>
</div>
</div></td>
<td><p>

![[49699291139-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

## **2. CODE REVIEW REPORT**

<div>

<table>
<tbody>
<tr>
<th><p><strong>No.</strong></p></th>
<th><p><strong>REVIEW LOGIC</strong></p></th>
<th><p><strong>Passed?</strong></p></th>
<th><p><strong>Explanation (text or captured image)</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p><strong>Have covered JUnit tests?</strong> (check possible cases are coverage by JUnit test)</p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p><strong>Have no side-effect from the changes?</strong> (check other places that call to this)</p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p><strong>Handling errors is correct?</strong> (check NPE, try/catch, validate...)</p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p><strong>No duplicated code?</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p><strong>Attach jenkins build result in PR</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>6</p></td>
<td><p><strong>Checking impact with integration test</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>7</p></td>
<td><p><strong>Function is correct purpose (no need to split function). Datatype is correct</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td colspan="4"><p><strong>REVIEW PERFORMANCE ISSUES</strong></p></td>
</tr>
<tr>
<td><p>8</p></td>
<td><p><strong>No N + 1 issue?</strong> (Check DB &amp; API calls)</p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>9</p></td>
<td><p><strong>No duplicated calls</strong> (Check DB &amp; API, method calls)</p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>10</p></td>
<td><p><strong>Can use caching?</strong> (Check the data, resource can be cached to improve performance)</p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>11</p></td>
<td><p><strong>Check correction of using bean scopes</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td colspan="4"><p><strong>REVIEW CODING CONVENTION</strong></p></td>
</tr>
<tr>
<td><p>12</p></td>
<td><p><strong>Followed naming convention</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>13</p></td>
<td><p><strong>Classes/methods are well organized?</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>14</p></td>
<td><p><strong>Class/method could be refactored?</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
<tr>
<td><p>15</p></td>
<td><p><strong>Have java-doc for complex class/method/parameter/api?</strong></p></td>
<td><p>OK / NOT OK</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[ePost Zip-Import - staging test-suite results - 19-08-2026]]
- [[CROSS-TEST LUZ-142507 Implement Analyze API Integration (Phase 1) Part 2]]
- [[Analytics Analyze API call when accessing eArchive]]
- [[Proof of concept Export and download storage]]
- [[eArchive Performance — Detail Overview]]

%% ai-graph-end %%