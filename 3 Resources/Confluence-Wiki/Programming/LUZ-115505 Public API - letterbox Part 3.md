---
title: "[LUZ-115505] Public API - letterbox | Part 3"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47739404382/LUZ-115505+Public+API+-+letterbox+Part+3
space: "TS"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2024-04-10
attachments: 32
tags:
  - confluence
  - programming
  - space/ts
---

# [LUZ-115505] Public API - letterbox | Part 3

> [!info] Imported from Confluence
> Space **TS** · updated 2024-04-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47739404382/LUZ-115505+Public+API+-+letterbox+Part+3)
> Relevance 0.738 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47739404382_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-115505" macro-id="f6046fd1-e653-416f-87b9-cf997690c0a1" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-115505" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-115505</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

ENV: dev

Canton Bern tenant ID: `ac2bd24c-5a4b-463f-8401-e983adb53764`

<a href="https://axonivy.atlassian.net/wiki/spaces/_/pages/47741469292?atlOrigin=eyJpIjoiNWI0YTE0OGU3OWFkNGUyZGEwNWIyYzEzZjYxYjUzMzYiLCJwIjoiYyJ9" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/_/pages/47741469292?atlOrigin=eyJpIjoiNWI0YTE0OGU3OWFkNGUyZGEwNWIyYzEzZjYxYjUzMzYiLCJwIjoiYyJ9</a>

------------------------------------------------------------------------

# **How to send the letter to Carton Bern tenant**

Use the API: <a href="https://api-dev.klara.tech/docs#/ePost%20Communication%20Platform%20Delivery/post_epost_v2_deliveries" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/ePost%20Communication%20Platform%20Delivery/post_epost_v2_deliveries</a>

Authentication: Carton Bern tenant (<a href="mailto:tai.nguyen01@axonactive.com" class="external-link" rel="nofollow">tai.nguyen01@axonactive.com</a> - tenant id: `ac2bd24c-5a4b-463f-8401-e983adb53764`) use the access token or api key

The payload:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3b434264-fd1e-4594-ac07-4cac1c30e995" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "fileMetadata": [
        {
            "deliveryChannelPreferences": ["DIGITAL"],
            "documentTitle": "test",
            "documentTypes": [
               "INVOICE"
             ],
            "documentDescription": "This is a test document",
            "origin": "eLetterbox API",
            "fileName": "invoice.pdf",
            "minimumLevelOfTrust": "BRONZE",
            "recipients": [
                {
                    "senderUserId": "ac2bd24c-5a4b-463f-8401-e983adb53764", // this is the tenant id of Carton Bern tenant in Klara
                    "unhashedCredentials": {
                        "participantId": "c95885f4-ac12-448e-b205-d3ed99a20a4a" // this is the tenant id of the individual from Carton Bern in Klara
                    }
                }
            ]
        }
    ]
}
```

</div>

</div>

------------------------------------------------------------------------

## **1. API get all deleted letters from Inbox and Storage (**<a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted</a>**)**

## 1.1 With normal user from epost

<div>

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Test steps</strong></p></th>
<th colspan="2"><p><strong>Test data</strong></p></th>
<th colspan="2"><p><strong>Expected result</strong></p></th>
<th colspan="2"><p><strong>Attachments &amp; Notes</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td rowspan="2"><p><strong>DEV</strong></p></td>
<td rowspan="2"><p><strong>STAGING</strong></p></td>
<td rowspan="2"><p><strong>DEV</strong></p></td>
<td rowspan="2"><p><strong>STAGING</strong></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p><strong>DEV</strong></p></td>
<td><p><strong>STAGING</strong></p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>Get deleted letters from Inbox and Storage <strong>without</strong> deleted folders<br />
</p></td>
<td><ol>
<li><p>Go to Inbox, delete <strong>1</strong> letter</p></li>
<li><p>Go to Storage, delete <strong>1</strong> document in the root folder (without belong to any parent folder)</p></li>
<li><p>Get deleted letters from Public API: <a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted" class="external-link" rel="nofollow">KLARA Public API</a></p></li>
<li><p>Compare the results from the response and the letters shown in the Trash.</p></li>
</ol></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>khoa.tang@axonactive.com</code></p></li>
<li><p><code>password</code>: <code>KhoaTP12345@</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>969b2b24-5ffe-4b7c-b1e2-a2a59fb1acb5</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>thientran010799@gmail.com</code></p></li>
<li><p><code>password</code>: <code>Aavn@1234</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>32b14b67-bb33-4887-a7fc-dd1cc1079945</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>The response returns <strong>2</strong> letters/documents with the correct metadata:</p>
<ul>
<li><p>1 letter from the Inbox 

![[47739404382-check.png]]

</p></li>
<li><p>1 document from the Storage 

![[47739404382-check.png]]

<br />
</p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>The response returns <strong>2</strong> letters/documents with the correct metadata:</p>
<ul>
<li><p>1 letter from the Inbox 

![[47739404382-check.png]]

</p></li>
<li><p>1 document from the Storage 

![[47739404382-check.png]]

</p></li>
</ul></li>
</ul></td>
<td><div id="expander-1311251333" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="754ba026-dad7-491c-a058-1c9292562126" data-macro-name="expand">
<div id="expander-control-1311251333" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API</span>
</div>
<div id="expander-content-1311251333" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="96fd2351-585f-4dd2-b7e4-bb1f37f846f4" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;63f5eb80c1e69b3f45d58092&quot;,
    &quot;letterTitle&quot;: &quot;billing và invoice nào và vào&quot;,
    &quot;fileName&quot;: &quot;billing va? invoice na?o va? va?o.pdf&quot;,
    &quot;senderParticipantId&quot;: null,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/63f5eb80c1e69b3f45d58092/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-02-22T10:16:32.270Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;636230120b2e1c1b1b1b1a3c&quot;,
    &quot;letterTitle&quot;: &quot;[Test Request original] Ind Com No-Scan EN&quot;,
    &quot;fileName&quot;: &quot;5Hi.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;8eaca665-be8e-43d4-af86-89184764dc3b&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: &quot;Ind Com No-Scan EN&quot;,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/636230120b2e1c1b1b1b1a3c/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-02-09T08:53:38.001Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1884993806" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f16f40b1-0d4d-4be0-9fde-5bd3027ee3e4" data-macro-name="expand">
<div id="expander-control-1884993806" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The letters in Trash</span>
</div>
<div id="expander-content-1884993806" class="expand-content expand-hidden">

![[47739404382-image-20240328-090858.png]]


</div>
</div></td>
<td><div id="expander-1368215175" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="2f3e3691-05fe-4b18-95bd-7938142c22b5" data-macro-name="expand">
<div id="expander-control-1368215175" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API</span>
</div>
<div id="expander-content-1368215175" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="bccc2792-145a-4181-80e8-90a2298aae71" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;64ef081d72b6ac26c1a3fbfd&quot;,
    &quot;letterTitle&quot;: &quot;net-doc&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: null,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef081d72b6ac26c1a3fbfd/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T09:13:01.590Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;64ef03e70963fa6f99b44cb5&quot;,
    &quot;letterTitle&quot;: &quot;Hmnet&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;c23b6e8a-217c-4628-916f-f3134a99db6f&quot;,
    &quot;senderUserId&quot;: &quot;qwe&quot;,
    &quot;senderCaseId&quot;: &quot;abc&quot;,
    &quot;senderEndToEndId&quot;: &quot;xyz&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef03e70963fa6f99b44cb5/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T08:55:02.975Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1503319772" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6381dbd5-bc3e-4b93-b9eb-4483147ad116" data-macro-name="expand">
<div id="expander-control-1503319772" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The letters in Trash</span>
</div>
<div id="expander-content-1503319772" class="expand-content expand-hidden">

![[47739404382-image-20240410-034407.png]]


</div>
</div></td>
</tr>
<tr>
<td>4</td>
<td><p>Get deleted letters from Inbox and Storage <strong>with</strong> deleted folders</p></td>
<td><ol>
<li><p>Go to Inbox, delete <strong>1</strong> letter</p></li>
<li><p>Go to Storage, delete a folder that contains a document (with multiple level of folder, <em>e.g: folder A &gt; folder B &gt; folder C &gt; document</em>)</p></li>
<li><p>Get deleted letters from Public API: <a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted" class="external-link" rel="nofollow">KLARA Public API</a></p></li>
<li><p>Compare the results from the response and the letters shown in the Trash.</p></li>
</ol>
<p><br />
</p></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>khoa.tang@axonactive.com</code></p></li>
<li><p><code>password</code>: <code>KhoaTP12345@</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>328d429e-9a98-40fe-aaf5-3a91f7296a9b</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>thientran010799@gmail.com</code></p></li>
<li><p><code>password</code>: <code>Aavn@1234</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>32b14b67-bb33-4887-a7fc-dd1cc1079945</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>The response returns <strong>2</strong> letters/documents with the correct metadata:</p>
<ul>
<li><p>1 letter from the Inbox 

![[47739404382-check.png]]

</p></li>
<li><p>1 document from the Storage 

![[47739404382-check.png]]

</p></li>
</ul></li>
</ul>
<p><br />
</p></td>
<td><ul>
<li><p>The response returns <strong>2</strong> letters/documents with the correct metadata:</p>
<ul>
<li><p>1 letter from the Inbox 

![[47739404382-check.png]]

</p></li>
<li><p>1 document from the Storage 

![[47739404382-check.png]]

</p></li>
</ul></li>
</ul></td>
<td><div id="expander-536419194" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="090f4bdf-a916-43cf-ba22-c6ee30f76a00" data-macro-name="expand">
<div id="expander-control-536419194" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API</span>
</div>
<div id="expander-content-536419194" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="48be788e-a13e-482e-b303-08226ead7215" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6474b2ccdeb62e6d7438d1b8&quot;,
    &quot;letterTitle&quot;: &quot;Update_Apr_23_DE&quot;,
    &quot;fileName&quot;: &quot;Update_Apr_23_DE.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;6eace38d-dad1-4487-8a30-814bd50ee6b1&quot;,
    &quot;senderUserId&quot;: &quot;3064&quot;,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6474b2ccdeb62e6d7438d1b8/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-05-29T14:12:28.207Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;64757aa1aee0900d55fd8207&quot;,
    &quot;letterTitle&quot;: &quot;Update_Apr_23_DE&quot;,
    &quot;fileName&quot;: &quot;Update_Apr_23_DE.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;6eace38d-dad1-4487-8a30-814bd50ee6b1&quot;,
    &quot;senderUserId&quot;: &quot;130&quot;,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/64757aa1aee0900d55fd8207/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-05-30T04:25:05.469Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1083685416" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="207e3a2b-4a92-43a8-a188-713c18da7ec5" data-macro-name="expand">
<div id="expander-control-1083685416" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The letters in Trash</span>
</div>
<div id="expander-content-1083685416" class="expand-content expand-hidden">

![[47739404382-image-20240328-101826.png]]


</div>
</div></td>
<td><div id="expander-1944113205" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="3f83a0b3-677c-4d58-bf94-9ccf06d2868c" data-macro-name="expand">
<div id="expander-control-1944113205" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API</span>
</div>
<div id="expander-content-1944113205" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b3f0619b-43b4-4b41-bfca-19a85afdfc0b" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;64ef081d72b6ac26c1a3fbfd&quot;,
    &quot;letterTitle&quot;: &quot;net-doc&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: null,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef081d72b6ac26c1a3fbfd/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T09:13:01.590Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;64ef03e70963fa6f99b44cb5&quot;,
    &quot;letterTitle&quot;: &quot;Hmnet&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;c23b6e8a-217c-4628-916f-f3134a99db6f&quot;,
    &quot;senderUserId&quot;: &quot;qwe&quot;,
    &quot;senderCaseId&quot;: &quot;abc&quot;,
    &quot;senderEndToEndId&quot;: &quot;xyz&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef03e70963fa6f99b44cb5/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T08:55:02.975Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1882378274" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d4daa37a-19a7-45b0-a284-e1aa538014be" data-macro-name="expand">
<div id="expander-control-1882378274" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The letters in Trash</span>
</div>
<div id="expander-content-1882378274" class="expand-content expand-hidden">

![[47739404382-image-20240410-040931.png]]


</div>
</div></td>
</tr>
<tr>
<td>5</td>
<td><p>Get deleted letters from Inbox and Storage <strong>with</strong> <code>sender-participant-id</code> filtering</p></td>
<td><ol>
<li><p>Go to Inbox, delete <strong>1</strong> letter that has the sender participant id.</p></li>
<li><p>Go to Storage, delete <strong>1</strong> document <em>in the root folder</em> (without belong to any parent folder) and without sender participant id.</p></li>
<li><p>Get deleted letters from Public API: <a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted" class="external-link" rel="nofollow">KLARA Public API</a>, filter with <code>sender-participant-id</code></p></li>
<li><p>Compare the results from the response and the letters shown in the Trash.</p></li>
</ol></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>khoa.tang@axonactive.com</code></p></li>
<li><p><code>password</code>: <code>KhoaTP12345@</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>969b2b24-5ffe-4b7c-b1e2-a2a59fb1acb5</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
<li><p><code>sender-participant-id</code>: <code>8eaca665-be8e-43d4-af86-89184764dc3b</code></p></li>
</ul></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>thientran010799@gmail.com</code></p></li>
<li><p><code>password</code>: <code>Aavn@1234</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>32b14b67-bb33-4887-a7fc-dd1cc1079945</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
<li><p><code>sender-participant-id</code>: <code>c23b6e8a-217c-4628-916f-f3134a99db6f</code></p></li>
</ul></td>
<td><ul>
<li><p>The response returns <strong>only 1</strong> letter with the given <code>sender-participant-id</code> 

![[47739404382-check.png]]

</p></li>
</ul></td>
<td><ul>
<li><p>The response returns <strong>only 1</strong> letter with the given <code>sender-participant-i</code> 

![[47739404382-check.png]]

</p></li>
</ul></td>
<td><div id="expander-926325312" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7ed35659-84b3-4d27-9478-f53ab3810aeb" data-macro-name="expand">
<div id="expander-control-926325312" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API withou sender-participant-id filtering</span>
</div>
<div id="expander-content-926325312" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d2487694-e430-4465-9e80-f06431ee9dc4" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;63f5eb80c1e69b3f45d58092&quot;,
    &quot;letterTitle&quot;: &quot;billing và invoice nào và vào&quot;,
    &quot;fileName&quot;: &quot;billing va? invoice na?o va? va?o.pdf&quot;,
    &quot;senderParticipantId&quot;: null,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/63f5eb80c1e69b3f45d58092/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-02-22T10:16:32.270Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;636230120b2e1c1b1b1b1a3c&quot;,
    &quot;letterTitle&quot;: &quot;[Test Request original] Ind Com No-Scan EN&quot;,
    &quot;fileName&quot;: &quot;5Hi.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;8eaca665-be8e-43d4-af86-89184764dc3b&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: &quot;Ind Com No-Scan EN&quot;,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/636230120b2e1c1b1b1b1a3c/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-02-09T08:53:38.001Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1204075389" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d2431e08-cc0d-4c64-bc63-abe9673abed0" data-macro-name="expand">
<div id="expander-control-1204075389" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API with sender-participant-id filtering</span>
</div>
<div id="expander-content-1204075389" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b33138db-811f-4d37-8ae9-8d53396dcf55" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;636230120b2e1c1b1b1b1a3c&quot;,
    &quot;letterTitle&quot;: &quot;[Test Request original] Ind Com No-Scan EN&quot;,
    &quot;fileName&quot;: &quot;5Hi.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;8eaca665-be8e-43d4-af86-89184764dc3b&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: &quot;Ind Com No-Scan EN&quot;,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/636230120b2e1c1b1b1b1a3c/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-02-09T08:53:38.001Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div></td>
<td><div id="expander-792173538" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7ae74ac2-2b6c-42ef-aa0f-3b59510abc0b" data-macro-name="expand">
<div id="expander-control-792173538" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API withou sender-participant-id filtering</span>
</div>
<div id="expander-content-792173538" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="809edc7c-42d4-47ca-9e33-98cd9ac0657a" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6616183db0bdef5f42f36957&quot;,
    &quot;letterTitle&quot;: &quot;testing&quot;,
    &quot;fileName&quot;: &quot;testing.pdf&quot;,
    &quot;senderParticipantId&quot;: null,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/6616183db0bdef5f42f36957/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-04-10T04:40:29.339Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;64ef03e70963fa6f99b44cb5&quot;,
    &quot;letterTitle&quot;: &quot;Hmnet&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;c23b6e8a-217c-4628-916f-f3134a99db6f&quot;,
    &quot;senderUserId&quot;: &quot;qwe&quot;,
    &quot;senderCaseId&quot;: &quot;abc&quot;,
    &quot;senderEndToEndId&quot;: &quot;xyz&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef03e70963fa6f99b44cb5/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T08:55:02.975Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1245276769" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d1d47b7c-ede6-4409-ad88-ed00c8b8771e" data-macro-name="expand">
<div id="expander-control-1245276769" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API with sender-participant-id filtering</span>
</div>
<div id="expander-content-1245276769" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="72d7deb7-0276-45c3-a62d-e9da14a121d2" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;64ef03e70963fa6f99b44cb5&quot;,
    &quot;letterTitle&quot;: &quot;Hmnet&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;c23b6e8a-217c-4628-916f-f3134a99db6f&quot;,
    &quot;senderUserId&quot;: &quot;qwe&quot;,
    &quot;senderCaseId&quot;: &quot;abc&quot;,
    &quot;senderEndToEndId&quot;: &quot;xyz&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef03e70963fa6f99b44cb5/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T08:55:02.975Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td>6</td>
<td><p>Get deleted letters when Trash is <strong>empty</strong></p></td>
<td><ol>
<li><p>Make sure Trash is empty</p></li>
<li><p>Get deleted letters from Public API: <a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted" class="external-link" rel="nofollow">KLARA Public API</a></p></li>
</ol></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>khoa.tang@axonactive.com</code></p></li>
<li><p><code>password</code>: <code>KhoaTP12345@</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>969b2b24-5ffe-4b7c-b1e2-a2a59fb1acb5</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>thientran010799@gmail.com</code></p></li>
<li><p><code>password</code>: <code>Aavn@1234</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>32b14b67-bb33-4887-a7fc-dd1cc1079945</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
</ul></td>
<td><ul>
<li><p>The response returns an empty JSON 

![[47739404382-check.png]]

</p></li>
</ul></td>
<td><ul>
<li><p>The response returns an empty JSON 

![[47739404382-check.png]]

</p></li>
</ul></td>
<td></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td><p>Get deleted letters from Inbox and Storage <strong>with</strong> <code>limit</code> and <code>offset</code> param</p></td>
<td><ol>
<li><p>Go to Inbox, delete <strong>4</strong> letters</p></li>
<li><p>Get deleted letters from Public API: <a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_deleted" class="external-link" rel="nofollow">KLARA Public API</a> with <code>limit</code> param is <code>2</code>, <code>offset</code> param is <code>1</code></p></li>
</ol></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>khoa.tang@axonactive.com</code></p></li>
<li><p><code>password</code>: <code>KhoaTP12345@</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>969b2b24-5ffe-4b7c-b1e2-a2a59fb1acb5</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
<li><p><code>limit</code>: <code>2</code></p></li>
<li><p><code>offset</code>: <code>1</code></p></li>
</ul></td>
<td><ul>
<li><p>Get access token:</p>
<ul>
<li><p><code>username</code>: <code>thientran010799@gmail.com</code></p></li>
<li><p><code>password</code>: <code>Aavn@1234</code></p></li>
<li><p><code>grant_type</code>: <code>password</code></p></li>
<li><p><code>tenant_id</code>: <code>32b14b67-bb33-4887-a7fc-dd1cc1079945</code></p></li>
<li><p><code>company_id</code>: <code>0</code></p></li>
</ul></li>
<li><p><code>limit</code>: <code>2</code></p></li>
<li><p><code>offset</code>: <code>1</code></p></li>
</ul></td>
<td><ul>
<li><p>The response returns 2 letters 

![[47739404382-check.png]]

</p></li>
</ul></td>
<td><ul>
<li><p>The response returns 2 letters 

![[47739404382-check.png]]

</p></li>
</ul></td>
<td><div id="expander-175442727" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="8cf45104-4277-4b13-81f4-3c2cade09366" data-macro-name="expand">
<div id="expander-control-175442727" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API</span>
</div>
<div id="expander-content-175442727" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="399ec58d-581d-4b64-bfc7-4fdc76eb2c19" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6464505bf8b58975abbd1f85&quot;,
    &quot;letterTitle&quot;: &quot;Update_Apr_23_DE&quot;,
    &quot;fileName&quot;: &quot;Update_Apr_23_DE.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;6eace38d-dad1-4487-8a30-814bd50ee6b1&quot;,
    &quot;senderUserId&quot;: &quot;21&quot;,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6464505bf8b58975abbd1f85/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-05-17T03:56:11.385Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;64747102deb62e6d74381c2c&quot;,
    &quot;letterTitle&quot;: &quot;Update_Apr_23_DE&quot;,
    &quot;fileName&quot;: &quot;Update_Apr_23_DE.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;6eace38d-dad1-4487-8a30-814bd50ee6b1&quot;,
    &quot;senderUserId&quot;: &quot;21&quot;,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/64747102deb62e6d74381c2c/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-05-29T09:31:46.356Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1941752609" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="e9cae250-7b5d-4f01-8ba3-071d4aed2b9d" data-macro-name="expand">
<div id="expander-control-1941752609" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The letters in Trash</span>
</div>
<div id="expander-content-1941752609" class="expand-content expand-hidden">

![[47739404382-image-20240329-014418.png]]


</div>
</div></td>
<td><div id="expander-1143754912" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="90a57fbb-9434-48a9-bbe5-38a01b796df8" data-macro-name="expand">
<div id="expander-control-1143754912" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The response from Public API</span>
</div>
<div id="expander-content-1143754912" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d583e177-8d70-4084-80ea-bf9cc66747fc" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;64f03f9dff84ac264bd6bd7f&quot;,
    &quot;letterTitle&quot;: &quot;Phu tests large file 24.08&quot;,
    &quot;fileName&quot;: &quot;1mb0925.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;7a9a6392-8a81-46ec-9c97-77bee4a7e00d&quot;,
    &quot;senderUserId&quot;: &quot;XXX1&quot;,
    &quot;senderCaseId&quot;: &quot;1&quot;,
    &quot;senderEndToEndId&quot;: &quot;1&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64f03f9dff84ac264bd6bd7f/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-31T07:22:05.820Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;,
    &quot;remainingDayToDelete&quot;: 30
  },
  {
    &quot;id&quot;: &quot;64ef03e70963fa6f99b44cb5&quot;,
    &quot;letterTitle&quot;: &quot;Hmnet&quot;,
    &quot;fileName&quot;: &quot;net-doc.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;c23b6e8a-217c-4628-916f-f3134a99db6f&quot;,
    &quot;senderUserId&quot;: &quot;qwe&quot;,
    &quot;senderCaseId&quot;: &quot;abc&quot;,
    &quot;senderEndToEndId&quot;: &quot;xyz&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev-staging.klara.tech/epost/v2/letters/64ef03e70963fa6f99b44cb5/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-08-30T08:55:02.975Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;,
    &quot;remainingDayToDelete&quot;: 30
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1695144600" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="8730932d-fe9d-46fb-baa5-8b9183b30f7f" data-macro-name="expand">
<div id="expander-control-1695144600" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">The letters in Trash</span>
</div>
<div id="expander-content-1695144600" class="expand-content expand-hidden">

![[47739404382-image-20240410-045116.png]]


</div>
</div></td>
</tr>
</tbody>
</table>

</div>

## 1.2 With Carton Bern user

How to authenticate: [How to integrate with ePost Keycloak](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47741469292/How+to+integrate+with+ePost+Keycloak)

###### *Note: all the case are tested in above table, so we just test the authentication flow here*

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
<th colspan="2"><p><strong>Test case</strong></p></th>
<th><p><strong>Expected result</strong></p></th>
<th><p><strong>Attachments</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td rowspan="2"><p>Carton Bern user</p>
<p>Test data:</p>
<ul>
<li><p>username: <a href="mailto:testuser5@dxc.com" class="external-link" rel="nofollow">testuser5@dxc.com</a></p></li>
<li><p>password: klara2024</p></li>
<li><p>tenantId: c95885f4-ac12-448e-b205-d3ed99a20a4a</p></li>
</ul></td>
<td><p>Get without <strong>sender-participant-id</strong></p></td>
<td><p>Return all document that is sent from Carton Bern tenant 

![[47739404382-check.png]]

</p>
<div id="expander-35073201" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f85b0e00-21c6-43c0-82ec-8bf871a3c615" data-macro-name="expand">
<div id="expander-control-35073201" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-35073201" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="877e6eae-a690-4c0e-8ac8-406972cf7f3c" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;,
      &quot;remainingDayToDelete&quot;: 30
    },
    {
      &quot;id&quot;: &quot;660f6f01f2aa243ee218caed&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;sample.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6f01f2aa243ee218caed/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:24:49.374Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;,
      &quot;remainingDayToDelete&quot;: 30
    },
    {
      &quot;id&quot;: &quot;660f6e7598ceb754b6d3842a&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6e7598ceb754b6d3842a/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:22:29.831Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;,
      &quot;remainingDayToDelete&quot;: 30
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1559327902" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="5eeee677-6e87-4bff-a6f5-087d9066225d" data-macro-name="expand">
<div id="expander-control-1559327902" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-1559327902" class="expand-content expand-hidden">

![[47739404382-image-20240405-074152.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>Get with <strong>sender-participant-id</strong> is exist: <code>ac2bd24c-5a4b-463f-8401-e983adb53764</code></p></td>
<td><p> Return all document that is sent from <strong>sender-participant-id</strong> 

![[47739404382-check.png]]

</p>
<div id="expander-1267740991" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="13d12f1c-2f91-4cca-9a61-bb98b0fedebe" data-macro-name="expand">
<div id="expander-control-1267740991" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1267740991" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5ba1f9df-d108-451b-b1c6-1b3c4e71b44e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;,
      &quot;remainingDayToDelete&quot;: 30
    },
    {
      &quot;id&quot;: &quot;660f6f01f2aa243ee218caed&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;sample.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6f01f2aa243ee218caed/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:24:49.374Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;,
      &quot;remainingDayToDelete&quot;: 30
    },
    {
      &quot;id&quot;: &quot;660f6e7598ceb754b6d3842a&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6e7598ceb754b6d3842a/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:22:29.831Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;,
      &quot;remainingDayToDelete&quot;: 30
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-793328935" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6ec69e0c-d2bf-45ee-8aec-09bd8c4577cc" data-macro-name="expand">
<div id="expander-control-793328935" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-793328935" class="expand-content expand-hidden">

![[47739404382-image-20240405-074248.png]]


</div>
</div></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## **2.API search letters (**<a href="https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_search" class="external-link" data-card-appearance="inline" rel="nofollow">https://api-dev.klara.tech/docs#/ePost%20Digital%20Letterbox/get_epost_v2_letters_search</a>**)**

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
<th colspan="2"><p><strong>Test case</strong></p></th>
<th><p><strong>Test</strong> <strong>steps</strong></p></th>
<th><p><strong>Expected</strong></p></th>
<th><p><strong>Staging</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td rowspan="12"><p>Carton Bern user</p>
<p>Test data:</p>
<ul>
<li><p>username: <a href="mailto:testuser5@dxc.com" class="external-link" rel="nofollow">testuser5@dxc.com</a></p></li>
<li><p>password: klara2024</p></li>
<li><p>tenantId: c95885f4-ac12-448e-b205-d3ed99a20a4a</p></li>
</ul></td>
<td><p>Search <strong>ALL</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: ALL</p></td>
<td><p>This tenant has 6 document</p>
<p>Should return all document of this tenant 

![[47739404382-check.png]]

</p>
<div id="expander-1467864420" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="b9074b23-419c-4fb9-a8d1-28977ab37886" data-macro-name="expand">
<div id="expander-control-1467864420" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1467864420" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b0721b65-5f46-41a6-a9a4-1db1c82e6688" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f5e265c03d32e96b39379&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f5e265c03d32e96b39379/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T02:12:54.573Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f6f01f2aa243ee218caed&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;sample.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6f01f2aa243ee218caed/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:24:49.374Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f6e7598ceb754b6d3842a&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6e7598ceb754b6d3842a/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:22:29.831Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1277980366" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1e9d780d-ae43-4fec-a104-3dd196c5820b" data-macro-name="expand">
<div id="expander-control-1277980366" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-1277980366" class="expand-content expand-hidden">

![[47739404382-image-20240405-034240.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>2</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: ALL</p>
<p>value: <strong>payslip</strong></p></td>
<td><p>Should return all document that have the document name, sender name or document content contain the text <strong>payslip</strong> 

![[47739404382-check.png]]

</p>
<div id="expander-1874634545" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f3f9d768-389d-4774-861f-2adc3b83b180" data-macro-name="expand">
<div id="expander-control-1874634545" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1874634545" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="de6c8850-1eaa-4bc9-84f7-b26b1ce6e0f0" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-290951447" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="9bc61ddb-6ab6-45ea-b0e2-9f19d664341e" data-macro-name="expand">
<div id="expander-control-290951447" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-290951447" class="expand-content expand-hidden">

![[47739404382-image-20240405-035232.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>With <strong>sender-participant-id</strong> is exist</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: ALL</p>
<p>sender-participant-id: ac2bd24c-5a4b-463f-8401-e983adb53764</p></td>
<td><p>Should return all document of <strong>sender-participant-id</strong> 

![[47739404382-check.png]]

</p>
<div id="expander-1886220500" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="4c380bff-0323-4c36-a78f-02327d49be33" data-macro-name="expand">
<div id="expander-control-1886220500" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1886220500" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9033ec27-63ce-4524-8444-aa42ee2c995e" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f5e265c03d32e96b39379&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f5e265c03d32e96b39379/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T02:12:54.573Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>With <strong>sender-participant-id</strong> is <strong>NOT</strong> exist</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: ALL</p>
<p>sender-participant-id: 3a43be87-ede2-4f90-9930-16666004de0a</p></td>
<td><p>Should return empty list 

![[47739404382-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p>Search <strong>INBOX</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: INBOX</p></td>
<td><p>Should return all document from INBOX only. 

![[47739404382-check.png]]

</p>
<div id="expander-482022990" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="e69ad116-7754-485f-88f8-b0e2c31945be" data-macro-name="expand">
<div id="expander-control-482022990" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-482022990" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f26ed0b5-cc6f-4454-92be-8bf0a44bf479" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f5e265c03d32e96b39379&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f5e265c03d32e96b39379/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T02:12:54.573Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1201798589" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="fd3b3c8a-f49f-4e44-a6a1-067869469387" data-macro-name="expand">
<div id="expander-control-1201798589" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-1201798589" class="expand-content expand-hidden">

![[47739404382-image-20240405-035133.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>6</td>
<td><p>Search <strong>INBOX</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: INBOX</p>
<p>value: <strong>another</strong></p></td>
<td><p>Should return all document in inbox with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-1891140287" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="4e1e1a2b-9703-475c-a3c1-6880373b27fa" data-macro-name="expand">
<div id="expander-control-1891140287" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1891140287" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5a6023af-015c-4592-8148-adfb27d85f56" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-944602331" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="ba463b9f-8b34-4adf-a03b-3c2fc1d4cb0b" data-macro-name="expand">
<div id="expander-control-944602331" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-944602331" class="expand-content expand-hidden">

![[47739404382-image-20240405-040036.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>7</td>
<td><p>Search <strong>STORAGE</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: STORAGE</p></td>
<td><p>Should return all document in storage 

![[47739404382-check.png]]

</p>
<div id="expander-605835003" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="814dbf44-6ad8-4e47-a991-f1f38914526d" data-macro-name="expand">
<div id="expander-control-605835003" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-605835003" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="172d48e8-e3ad-40b0-8e88-870cef78f1ee" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f6e7598ceb754b6d3842a&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6e7598ceb754b6d3842a/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:22:29.831Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f6f01f2aa243ee218caed&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;sample.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6f01f2aa243ee218caed/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:24:49.374Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-701244549" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f5d56094-9f6c-46a7-83a8-b902792b9c65" data-macro-name="expand">
<div id="expander-control-701244549" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-701244549" class="expand-content expand-hidden">

![[47739404382-image-20240405-040231.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>8</td>
<td><p>Search <strong>STORAGE</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: STORAGE</p>
<p>value: <strong>payslip</strong></p></td>
<td><p>Should return all document in storage with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-580010792" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1ae57213-0188-4376-8b5e-c9baf2562b28" data-macro-name="expand">
<div id="expander-control-580010792" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-580010792" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="0ca429ee-455a-4971-ac9b-ff3018d7fcc1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1560790773" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="92530122-5c72-4335-bb92-9462ed63f4f1" data-macro-name="expand">
<div id="expander-control-1560790773" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-1560790773" class="expand-content expand-hidden">

![[47739404382-image-20240405-040323.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>9</td>
<td><p>Search <strong>TRASH</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: TRASH</p></td>
<td><p>Should return all document in trash 

![[47739404382-check.png]]

</p>
<div id="expander-1684885141" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1bb82d77-53a1-4bcd-9ae6-9d9bed7af254" data-macro-name="expand">
<div id="expander-control-1684885141" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1684885141" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="07b1ddfd-cbed-4f65-b741-b2ca58b46bb7" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f6f01f2aa243ee218caed&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;sample.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6f01f2aa243ee218caed/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:24:49.374Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f6e7598ceb754b6d3842a&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6e7598ceb754b6d3842a/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:22:29.831Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-994352583" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="3fdd9e83-0f4a-47ad-acc8-4a9d338844ff" data-macro-name="expand">
<div id="expander-control-994352583" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-994352583" class="expand-content expand-hidden">

![[47739404382-image-20240405-040936.png]]


</div>
</div></td>
<td></td>
</tr>
<tr>
<td>10</td>
<td><p>Search <strong>TRASH</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: TRASH</p>
<p>value: <strong>terms</strong></p></td>
<td><p>Should return all document in trash with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-1786433458" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="cde38858-37c4-4fd7-a208-34f8678b6cad" data-macro-name="expand">
<div id="expander-control-1786433458" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1786433458" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="99e39f91-75dc-4d66-bbcb-022be10f23e5" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div></td>
<td></td>
</tr>
<tr>
<td>11</td>
<td><p>Search <strong>ALL</strong> location again</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: ALL</p></td>
<td><p>Should return all document of this tenant 

![[47739404382-check.png]]

</p>
<div id="expander-1323435011" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="fd37a3ce-810b-4118-98c1-4619ff40b1d1" data-macro-name="expand">
<div id="expander-control-1323435011" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1323435011" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="37037218-bff0-4dc1-9655-d39f4ed978f8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f727730b6c03d9982ee37&quot;,
      &quot;letterTitle&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;fileName&quot;: &quot;terms-1-copy.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f727730b6c03d9982ee37/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:35.133Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f6f01f2aa243ee218caed&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;sample.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6f01f2aa243ee218caed/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:24:49.374Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f6e7598ceb754b6d3842a&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f6e7598ceb754b6d3842a/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:22:29.831Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f5e265c03d32e96b39379&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;invoice.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f5e265c03d32e96b39379/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T02:12:54.573Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div></td>
<td></td>
</tr>
<tr>
<td>12</td>
<td><p>Search <strong>ALL</strong> location again</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 48</p>
<p>search-location: ALL</p>
<p>value: <strong>payslips</strong></p></td>
<td><p>Should return all document with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-395946347" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="3253948d-85b7-45cc-9eef-6950acda0e7a" data-macro-name="expand">
<div id="expander-control-395946347" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-395946347" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="079fab46-b25c-4d7c-a04b-0bfa774e2bf5" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f725e21825c15905cdebb&quot;,
      &quot;letterTitle&quot;: &quot;another payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f725e21825c15905cdebb/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:10.210Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    },
    {
      &quot;id&quot;: &quot;660f7255f2aa243ee218cb1e&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf file&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderUserId&quot;: &quot;ac2bd24c-5a4b-463f-8401-e983adb53764&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f7255f2aa243ee218cb1e/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T03:39:01.804Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div></td>
<td></td>
</tr>
<tr>
<td>13</td>
<td rowspan="13"><p>Normal Epost user</p>
<p>Test data:</p>
<ul>
<li><p>username: <a href="mailto:miracle_annguyen2@axongroupio.ch" class="external-link" rel="nofollow">miracle_annguyen2@axongroupio.ch</a></p></li>
<li><p>password: Aavn123456</p></li>
<li><p>tenantId: 3fc5f554-2c20-4b9c-8f11-a55c3a1b2b0a</p></li>
</ul></td>
<td><p>Search <strong>ALL</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 200</p>
<p>search-location: ALL</p></td>
<td><p>Should return all document of this tenant 

![[47739404382-check.png]]

</p>
<p><span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="5c33fb06-8e22-4637-8e27-5fa5155638ff" data-macro-name="view-file"><a href="../_attachments/47739404382-response_1712221194057.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47739404382/response_1712221194057.json?version=1&amp;modificationDate=1712221341987&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47739404382-response_1712221194057.json]]

</a></span></p>
<div id="expander-2047212171" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="98eddf07-f34e-40ca-ac78-3402fa8a7e29" data-macro-name="expand">
<div id="expander-control-2047212171" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-2047212171" class="expand-content expand-hidden">

![[47739404382-image-20240404-093059.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>14</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: ALL</p>
<p>value: <strong>Your appointment with us is confirmed.</strong></p></td>
<td><p>Should return all document with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-642555688" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="b2b69160-ce9d-4175-b157-92694d6c2af2" data-macro-name="expand">
<div id="expander-control-642555688" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-642555688" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="3b1f94fe-2f7a-4ae6-a2d3-e45cd4918267" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6603da9b3ac19a1907b69cf6&quot;,
    &quot;letterTitle&quot;: &quot;It&#39;s almost time.&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;75ff4b57-3f3c-476c-a28b-63fb39ee9939&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;6055&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6603da9b3ac19a1907b69cf6/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-03-27T08:36:43.306Z&quot;,
    &quot;documentMessage&quot;: &quot;Dear an, your appointment is taking place soon.\r\nWe&#39;re looking forward to seeing you at Wednesday, 27.03.2024 at 10:40 o&#39;clock at Lettensteg 10, 8037 Zürich, Switzerland 3400 Burgdorf.\r\n1.Miracle Company Team\r\nCan&#39;t make it? Cancel here: https://dev.klara.tech/s/1rmzq5js&quot;,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;6603cb010b247122fff74e24&quot;,
    &quot;letterTitle&quot;: &quot;Your appointment with us is confirmed.&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;82&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6603cb010b247122fff74e24/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-03-27T07:30:09.565Z&quot;,
    &quot;documentMessage&quot;: &quot;Mr epost 2, we&#39;re looking forward to seeing you at Wednesday, 27.03.2024 at 08:50 o&#39;clock. An - main company Team. Can&#39;t make it? Cancel here: https://dev.klara.tech/s/u590jjv5 use\r\naaaaMrzzzz aasdasBahnhofstrasse 123 8001 Zürich zxxzz use xxcczx zz\r\nzzzzz&quot;,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;6603ca2bd2c9bc5c04a76d15&quot;,
    &quot;letterTitle&quot;: &quot;Your appointment with us is confirmed.&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;75ff4b57-3f3c-476c-a28b-63fb39ee9939&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;6055&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6603ca2bd2c9bc5c04a76d15/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-03-27T07:26:35.296Z&quot;,
    &quot;documentMessage&quot;: &quot;testttttttttttttttttttttttt&amp;nbsp;Hello an,&amp;nbsp;&amp;nbsp;thank you very much for your booking.&amp;nbsp;Your appointment has been confirmed.We&#39;re looking forward to seeing you at Wednesday, 27.03.2024 at 10:40 o&#39;clock at Lettensteg 10, 8037 Zürich, Switzerland 3400 Burgdorf.We will remind you 1 hours before your appointment.1.Miracle Company TeamCan&#39;t make it?Cancel here: https://dev.klara.tech/s/r9dtmry8&quot;,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;65fd0836cc25590a1cb27342&quot;,
    &quot;letterTitle&quot;: &quot;It&#39;s almost time&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;81&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65fd0836cc25590a1cb27342/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-03-22T04:25:26.929Z&quot;,
    &quot;documentMessage&quot;: &quot;Mr epost 2, your appointment is taking place soon. See you on Friday, 22.03.2024 at 06:30 o&#39;clock. An - main company Team. Can&#39;t make it? Cancel here: https://dev.klara.tech/s/190koopo&quot;,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;65fd057aae7b050447eb889a&quot;,
    &quot;letterTitle&quot;: &quot;Your appointment with us is confirmed.&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;81&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65fd057aae7b050447eb889a/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-03-22T04:13:46.848Z&quot;,
    &quot;documentMessage&quot;: &quot;Mr epost 2, we&#39;re looking forward to seeing you at Friday, 22.03.2024 at 06:30 o&#39;clock. An - main company Team. Can&#39;t make it? Cancel here: https://dev.klara.tech/s/l5jejdb9 use\r\naaaaMrzzzz aasdasBahnhofstrasse 123 8001 Zürich zxxzz use xxcczx zz\r\nzzzzz&quot;,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-572692460" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="dac33537-c133-447b-9ad3-66bfc0711dc0" data-macro-name="expand">
<div id="expander-control-572692460" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-572692460" class="expand-content expand-hidden">

![[47739404382-image-20240404-094415.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>15</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: ALL</p>
<p>value: <strong>8001 Zürich</strong></p></td>
<td><p>Should return all document with filter text is from German 

![[47739404382-check.png]]

</p>
<div id="expander-1312420449" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1eb2fa15-47ed-44ed-9372-8599b9dc6929" data-macro-name="expand">
<div id="expander-control-1312420449" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1312420449" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="89c970db-56fe-4e14-bd6e-9bdd8b8676d1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;65d6fa0e87a66625796bfe02&quot;,
    &quot;letterTitle&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;fileName&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;1847c428-b80c-41c8-9230-ff1363494a96&quot;,
    &quot;senderCaseId&quot;: &quot;421593&quot;,
    &quot;senderEndToEndId&quot;: &quot;421593&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d6fa0e87a66625796bfe02/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-22T07:38:53.949Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;65d475abce0cb04ac2456fd3&quot;,
    &quot;letterTitle&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;fileName&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;f648c4de-2b00-403b-89f3-5a05fa00ff3e&quot;,
    &quot;senderCaseId&quot;: &quot;421383&quot;,
    &quot;senderEndToEndId&quot;: &quot;421383&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d475abce0cb04ac2456fd3/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-20T09:49:30.995Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-697357914" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="95e5c0f7-183e-4154-b408-66b564aa723f" data-macro-name="expand">
<div id="expander-control-697357914" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-697357914" class="expand-content expand-hidden">

![[47739404382-image-20240404-094357.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>16</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>With <strong>sender-participant-id</strong> is exist</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: ALL</p></td>
<td><p>Should return all document is sent from <strong>sender-participant-id</strong> 

![[47739404382-check.png]]

</p>
<div id="expander-1352386780" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="959a8d4b-a08d-4aea-bf71-acb1b3d052ee" data-macro-name="expand">
<div id="expander-control-1352386780" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1352386780" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="4b6e3965-1bcc-4d9d-b3aa-7b4f91d92e5d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
    {
      &quot;id&quot;: &quot;660f9d7dfd2a9a30ab43a976&quot;,
      &quot;letterTitle&quot;: &quot;payslips pdf&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
      &quot;senderUserId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f9d7dfd2a9a30ab43a976/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T06:43:09.826Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;UNREAD&quot;
    },
    {
      &quot;id&quot;: &quot;660f9d5d7d72a2255c6b30e5&quot;,
      &quot;letterTitle&quot;: &quot;test&quot;,
      &quot;fileName&quot;: &quot;payslips.pdf&quot;,
      &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
      &quot;senderUserId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
      &quot;senderCaseId&quot;: null,
      &quot;senderEndToEndId&quot;: null,
      &quot;documentTypes&quot;: [
        &quot;INVOICE&quot;
      ],
      &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/660f9d5d7d72a2255c6b30e5/content&quot;,
      &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
      &quot;receivedDateTime&quot;: &quot;2024-04-05T06:42:37.647Z&quot;,
      &quot;documentMessage&quot;: null,
      &quot;readStatus&quot;: &quot;READ&quot;
    }
  ]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-952163416" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="3b39707b-fdc3-4f84-9ea1-41733cc08491" data-macro-name="expand">
<div id="expander-control-952163416" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in GUI</span>
</div>
<div id="expander-content-952163416" class="expand-content expand-hidden">

![[47739404382-image-20240405-072554.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>17</td>
<td><p>Search <strong>ALL</strong> location</p>
<p>With <strong>sender-participant-id</strong> is <strong>NOT</strong> exist</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: ALL</p></td>
<td><p>Should return empty list 

![[47739404382-check.png]]

</p></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>18</td>
<td><p>Search <strong>INBOX</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: INBOX</p>
<p>value: <strong>ePost</strong></p></td>
<td><p>Should return all document with filter text and from INBOX only 

![[47739404382-check.png]]

</p>
<div id="expander-1756801620" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="4886b126-cd29-4157-b813-4316fa61fa39" data-macro-name="expand">
<div id="expander-control-1756801620" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1756801620" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="46d7cbb6-4485-4610-8af2-37eaed360548" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;65d6fa0e87a66625796bfe02&quot;,
    &quot;letterTitle&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;fileName&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;1847c428-b80c-41c8-9230-ff1363494a96&quot;,
    &quot;senderCaseId&quot;: &quot;421593&quot;,
    &quot;senderEndToEndId&quot;: &quot;421593&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d6fa0e87a66625796bfe02/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-22T07:38:53.949Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;65d475abce0cb04ac2456fd3&quot;,
    &quot;letterTitle&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;fileName&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;f648c4de-2b00-403b-89f3-5a05fa00ff3e&quot;,
    &quot;senderCaseId&quot;: &quot;421383&quot;,
    &quot;senderEndToEndId&quot;: &quot;421383&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d475abce0cb04ac2456fd3/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-20T09:49:30.995Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1840764658" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="10ed882d-39ba-495f-920c-33a92ae4f4aa" data-macro-name="expand">
<div id="expander-control-1840764658" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-1840764658" class="expand-content expand-hidden">

![[47739404382-image-20240404-095132.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>19</td>
<td><p>Search <strong>STORAGE</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: STORAGE</p></td>
<td><p>Should return all letter in STORAGE 

![[47739404382-check.png]]

</p>
<div id="expander-747293245" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="0701a6b1-3921-466f-b6c9-4c83c379c45a" data-macro-name="expand">
<div id="expander-control-747293245" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-747293245" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="4d5c1655-9845-4cd9-a2ac-cc2a117b210f" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6544621ef61c054742c38dca&quot;,
    &quot;letterTitle&quot;: &quot;Digital all the way!&quot;,
    &quot;fileName&quot;: &quot;ePost_scanning_service_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ef61c054742c38dca/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.050Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;6544621ff61c054742c38ddb&quot;,
    &quot;letterTitle&quot;: &quot;Welcome to ePost&quot;,
    &quot;fileName&quot;: &quot;ePost_welcome_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ff61c054742c38ddb/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.955Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-655018152" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="9272734e-2d2d-4bcb-be4c-0183448a008c" data-macro-name="expand">
<div id="expander-control-655018152" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-655018152" class="expand-content expand-hidden">

![[47739404382-image-20240404-102325.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>20</td>
<td><p>Search <strong>STORAGE</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: STORAGE</p>
<p>value: <strong>ePost</strong></p></td>
<td><p>Should return all document with filter text and from STORAGE only 

![[47739404382-check.png]]

</p>
<div id="expander-1979437126" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="693a254b-290a-4564-a14b-0b55fa48aa42" data-macro-name="expand">
<div id="expander-control-1979437126" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-1979437126" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="117e9345-0457-44d7-b589-97909f75c584" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6544621ef61c054742c38dca&quot;,
    &quot;letterTitle&quot;: &quot;Digital all the way!&quot;,
    &quot;fileName&quot;: &quot;ePost_scanning_service_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ef61c054742c38dca/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.050Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;6544621ff61c054742c38ddb&quot;,
    &quot;letterTitle&quot;: &quot;Welcome to ePost&quot;,
    &quot;fileName&quot;: &quot;ePost_welcome_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ff61c054742c38ddb/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.955Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1048232580" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="17961458-cd0e-4ed5-bca6-7ccb67e7d8ad" data-macro-name="expand">
<div id="expander-control-1048232580" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-1048232580" class="expand-content expand-hidden">

![[47739404382-image-20240404-095939.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>21</td>
<td><p>Search <strong>STORAGE</strong> location with the document inside the folder</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: STORAGE</p></td>
<td><p>Should return all document from STORAGE only with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-338154457" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="1b99f92e-2ca8-4e1c-84bf-2eb5763cb8e5" data-macro-name="expand">
<div id="expander-control-338154457" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-338154457" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ca39c30e-3f26-4e0e-ab01-b2b4ee3b5d14" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6544621ef61c054742c38dca&quot;,
    &quot;letterTitle&quot;: &quot;Digital all the way!&quot;,
    &quot;fileName&quot;: &quot;ePost_scanning_service_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ef61c054742c38dca/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.050Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;6544621ff61c054742c38ddb&quot;,
    &quot;letterTitle&quot;: &quot;Welcome to ePost&quot;,
    &quot;fileName&quot;: &quot;ePost_welcome_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ff61c054742c38ddb/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.955Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-2049950830" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="73f16aae-53c5-45ac-be91-7cfa3e95b794" data-macro-name="expand">
<div id="expander-control-2049950830" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-2049950830" class="expand-content expand-hidden">

![[47739404382-image-20240404-102813.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>22</td>
<td><p>Search <strong>TRASH</strong> location</p>
<p>Without filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: TRASH</p></td>
<td><p>Should return all document in TRASH folder 

![[47739404382-check.png]]

</p>
<div id="expander-920551166" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="4dca15ad-67cb-48b5-ac2c-2ae3d740bed0" data-macro-name="expand">
<div id="expander-control-920551166" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-920551166" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d3402fc0-4e38-4c3f-9520-b301c0b9506d" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;6603cb010b247122fff74e24&quot;,
    &quot;letterTitle&quot;: &quot;Your appointment with us is confirmed.&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;82&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6603cb010b247122fff74e24/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-03-27T07:30:09.565Z&quot;,
    &quot;documentMessage&quot;: &quot;Mr epost 2, we&#39;re looking forward to seeing you at Wednesday, 27.03.2024 at 08:50 o&#39;clock. An - main company Team. Can&#39;t make it? Cancel here: https://dev.klara.tech/s/u590jjv5 use\r\naaaaMrzzzz aasdasBahnhofstrasse 123 8001 Zürich zxxzz use xxcczx zz\r\nzzzzz&quot;,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;65b87448d90c1b51764bb16f&quot;,
    &quot;letterTitle&quot;: &quot;Your appointment with us is confirmed.&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;78&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65b87448d90c1b51764bb16f/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-01-30T04:00:08.767Z&quot;,
    &quot;documentMessage&quot;: &quot;Mr epost 2, we&#39;re looking forward to seeing you at Tuesday, 30.01.2024 at 05:00 o&#39;clock. An - main company Team. Can&#39;t make it? Cancel here: https://dev.klara.tech/s/51b6zlko&quot;,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;656d7a005b829b2baad272c7&quot;,
    &quot;letterTitle&quot;: &quot;Il est presque temps&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;daa4d101-2a7f-4c30-b3ef-8d63ebf96dd9&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;569&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/656d7a005b829b2baad272c7/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-12-04T07:04:32.716Z&quot;,
    &quot;documentMessage&quot;: &quot;Bonjour Monsieur england, Votre rendez-vous aura lieu prochainement.\r\nNous nous réjouissons de vous rencontrer à lundi, 04.12.2023 à 09:00 heures à Gotthardstrasse 26 6300 Zug.\r\nÉquipe SHAREKEY Swiss AG\r\nVous ne pouvez pas vous présenter? Annuler ici: https://dev.klara.tech/s/ndn3ic20&quot;,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-10032600" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6dc6d685-9fa1-4a2c-9402-c60f1ab2df69" data-macro-name="expand">
<div id="expander-control-10032600" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-10032600" class="expand-content expand-hidden">

![[47739404382-image-20240404-100925.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>23</td>
<td><p>Search <strong>TRASH</strong> location</p>
<p>With filter</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: TRASH</p>
<p>value: <strong>est</strong></p></td>
<td><p>Should return all document in TRASH folder with filter text 

![[47739404382-check.png]]

</p>
<div id="expander-443305539" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d131bed1-4b62-4bce-a0b2-ea9e6f093d61" data-macro-name="expand">
<div id="expander-control-443305539" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-443305539" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="dd3b86bb-c7b5-4dda-9df4-3217b4bcc299" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;656d7a005b829b2baad272c7&quot;,
    &quot;letterTitle&quot;: &quot;Il est presque temps&quot;,
    &quot;fileName&quot;: &quot;booking_notification.html&quot;,
    &quot;senderParticipantId&quot;: &quot;daa4d101-2a7f-4c30-b3ef-8d63ebf96dd9&quot;,
    &quot;senderUserId&quot;: &quot;ONLINE_BOOKING&quot;,
    &quot;senderCaseId&quot;: &quot;569&quot;,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/656d7a005b829b2baad272c7/content&quot;,
    &quot;letterType&quot;: &quot;SIMPLE_SHORT_MESSAGE&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-12-04T07:04:32.716Z&quot;,
    &quot;documentMessage&quot;: &quot;Bonjour Monsieur england, Votre rendez-vous aura lieu prochainement.\r\nNous nous réjouissons de vous rencontrer à lundi, 04.12.2023 à 09:00 heures à Gotthardstrasse 26 6300 Zug.\r\nÉquipe SHAREKEY Swiss AG\r\nVous ne pouvez pas vous présenter? Annuler ici: https://dev.klara.tech/s/ndn3ic20&quot;,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>24</td>
<td><p>Search <strong>ALL</strong> location again</p>
<p>With filter text</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: ALL</p>
<p>value: <strong>8001 Zürich</strong></p></td>
<td><p>Should return all document 

![[47739404382-check.png]]

</p>
<div id="expander-90602945" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="62c224e7-8e35-4225-8c5e-578e14e76d5d" data-macro-name="expand">
<div id="expander-control-90602945" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-90602945" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cf38c737-3469-46ae-9cf3-63ab9ba0518b" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;65d6fa0e87a66625796bfe02&quot;,
    &quot;letterTitle&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;fileName&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;1847c428-b80c-41c8-9230-ff1363494a96&quot;,
    &quot;senderCaseId&quot;: &quot;421593&quot;,
    &quot;senderEndToEndId&quot;: &quot;421593&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d6fa0e87a66625796bfe02/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-22T07:38:53.949Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;65d475abce0cb04ac2456fd3&quot;,
    &quot;letterTitle&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;fileName&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;f648c4de-2b00-403b-89f3-5a05fa00ff3e&quot;,
    &quot;senderCaseId&quot;: &quot;421383&quot;,
    &quot;senderEndToEndId&quot;: &quot;421383&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d475abce0cb04ac2456fd3/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-20T09:49:30.995Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-845214189" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="89469ce7-a748-4663-af07-31d9bc6629b8" data-macro-name="expand">
<div id="expander-control-845214189" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-845214189" class="expand-content expand-hidden">

![[47739404382-image-20240404-094357.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
<tr>
<td>25</td>
<td><p>Search <strong>ALL</strong> location again</p></td>
<td><p>Offset: 0</p>
<p>Limit: 5</p>
<p>search-location: ALL</p>
<p>value: <strong>ePost</strong></p></td>
<td><p>Should return all document with filter text 

![[47739404382-check.png]]

</p>
<p>Return 4 document 

![[47739404382-check.png]]

</p>
<div id="expander-526158601" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="a116db26-b016-4e02-bf1c-959d94532d2e" data-macro-name="expand">
<div id="expander-control-526158601" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">response</span>
</div>
<div id="expander-content-526158601" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="69fc41f8-b5d2-447d-9fb1-0ecd8b7d234a" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>[
  {
    &quot;id&quot;: &quot;65d6fa0e87a66625796bfe02&quot;,
    &quot;letterTitle&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;fileName&quot;: &quot;Second Reminder_2024-02-22_08-38-32.742.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;1847c428-b80c-41c8-9230-ff1363494a96&quot;,
    &quot;senderCaseId&quot;: &quot;421593&quot;,
    &quot;senderEndToEndId&quot;: &quot;421593&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d6fa0e87a66625796bfe02/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-22T07:38:53.949Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;65d475abce0cb04ac2456fd3&quot;,
    &quot;letterTitle&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;fileName&quot;: &quot;Friendly reminder_2024-02-20_10-47-33.080.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;487e529f-9f54-42e3-a078-4b28ed77cf29&quot;,
    &quot;senderUserId&quot;: &quot;f648c4de-2b00-403b-89f3-5a05fa00ff3e&quot;,
    &quot;senderCaseId&quot;: &quot;421383&quot;,
    &quot;senderEndToEndId&quot;: &quot;421383&quot;,
    &quot;documentTypes&quot;: [
      &quot;INVOICE&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/65d475abce0cb04ac2456fd3/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2024-02-20T09:49:30.995Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;READ&quot;
  },
  {
    &quot;id&quot;: &quot;6544621ff61c054742c38ddb&quot;,
    &quot;letterTitle&quot;: &quot;Welcome to ePost&quot;,
    &quot;fileName&quot;: &quot;ePost_welcome_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ff61c054742c38ddb/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.955Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  },
  {
    &quot;id&quot;: &quot;6544621ef61c054742c38dca&quot;,
    &quot;letterTitle&quot;: &quot;Digital all the way!&quot;,
    &quot;fileName&quot;: &quot;ePost_scanning_service_en.pdf&quot;,
    &quot;senderParticipantId&quot;: &quot;247ce978-a68d-4005-843c-4046c8e21c18&quot;,
    &quot;senderUserId&quot;: null,
    &quot;senderCaseId&quot;: null,
    &quot;senderEndToEndId&quot;: null,
    &quot;documentTypes&quot;: [
      &quot;contract&quot;
    ],
    &quot;letterContentReference&quot;: &quot;https://api-dev.klara.tech/epost/v2/letters/6544621ef61c054742c38dca/content&quot;,
    &quot;letterType&quot;: &quot;CLASSIC_LETTER&quot;,
    &quot;receivedDateTime&quot;: &quot;2023-11-03T02:59:42.050Z&quot;,
    &quot;documentMessage&quot;: null,
    &quot;readStatus&quot;: &quot;UNREAD&quot;
  }
]</code></pre>
</div>
</div>
</div>
</div>
<div id="expander-1711083121" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="b716057e-d757-4e52-b716-c00cd846d049" data-macro-name="expand">
<div id="expander-control-1711083121" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">result in webclient</span>
</div>
<div id="expander-content-1711083121" class="expand-content expand-hidden">

![[47739404382-image-20240404-094444.png]]


</div>
</div></td>
<td><p>

![[47739404382-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>
