---
ai_hash: b0ae694da464514f
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.79
entities: []
relevance: 0.769
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49391239187/iLetter+current+backend+architecture
space: TS
status: reference
tags:
- confluence
- architecture
- space/ts
title: iLetter current backend architecture
topic: architecture
type: source
updated: 2026-05-12
---

# iLetter current backend architecture

> [!info] Imported from Confluence
> Space **TS** · updated 2026-05-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49391239187/iLetter+current+backend+architecture)
> Relevance 0.769 · topic `architecture`

# The flows

The iLetter feature has three main flows: two existing ones (previously called “Smart Letter”) and a new one in the web client to simplify composing an iLetter.

- Sender composes the iLetter in luz_epost <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" hasbody="false" macro-id="d8ce4c67-77aa-4fbf-bd67-1ec2fa236f77" macro-name="status">NEW\*</span>

- Sender sends the iLetter to recipients <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete conf-macro output-inline" hasbody="false" macro-id="aaade239-a46e-42d0-954d-42cdf0dcef67" macro-name="status">EXISTING</span>

- Recipient replies to the iLetter <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete conf-macro output-inline" hasbody="false" macro-id="fe65e83e-82f7-4527-a4bb-79545f679793" macro-name="status">EXISTING</span>

### 1. Sender composes the iLetter in `luz_epost`


![[49391239187-image-20260512-025105.png]]



When the sender starts composing a new iLetter, we call this a “Draft“.

Currently, this “Draft” is stored in the `documents` collection in the sender's MongoDB.

<div id="expander-1318429167" class="expand-container conf-macro output-block" hasbody="true" macro-id="317bd855-abf8-4b4f-bc67-ea1469eeff41" macro-name="expand">

<div id="expander-control-1318429167" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Example</span>

</div>

<div id="expander-content-1318429167" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7ffbce6a-48dd-4a7e-95ca-bd4515abb772" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
            "_id": "69fac6979a507a61a62a0d24",
            "_createdBy": "nploi@axongroupio.ch",
            "_createdDate": "2026-05-06T04:41:59.761Z",
            "_updatedBy": "nploi@axongroupio.ch",
            "_updatedDate": "2026-05-07T01:19:56.946Z",
            "_deletionStatus": "false",
            "_files": {},
            "_isBasicDocument": false,
            "_isBeingCreated": false,
            "_isEnriched": false,
            "_isLargeFile": false,
            "_isTemporaryDocument": false,
            "_versionNumber": 7,
            "folderIds": [],
            "name": "My own template",
            "documentType": "SMART_LETTER_DRAFT",
            "senderEndToEndId": "4afeab7c-9f29-4903-8bdd-0f41c9ba820b",
            "letterJson": "{\"id\":\"eb1365ae-5d6e-48bc-8c6e-24fa60a2793a\",\"title\":\"My own template\",\"sourceContentUpdatedAt\":\"2026-05-07T01:19:51.224Z\",\"brandKitId\":\"bk-post\",\"screens\":[{\"id\":\"cf2be73c-05e8-4dcd-abe0-5859b651aacc\",\"order\":0,\"mode\":\"scroll\",\"elements\":[{\"id\":\"0fb33eed-bd12-45fd-98e6-19888d280418\",\"type\":\"header\",\"content\":\"Header 1\"},{\"id\":\"92b9832b-f7e9-48fd-9dc9-ddfcd93c1581\",\"type\":\"paragraph\",\"content\":\"Paragraph text goes here.\"}],\"style\":{\"styleVariant\":\"balanced\"}},{\"id\":\"b6c16e9a-9f63-47df-926f-40213d94f18e\",\"order\":1,\"mode\":\"scroll\",\"role\":\"ending\",\"elements\":[{\"id\":\"63a7d4f0-f5cf-4e9c-a4f9-6787f1c95267\",\"type\":\"header\",\"content\":\"Thank you!\"},{\"id\":\"eb713263-28f2-452b-8bbf-a3b24645c9b6\",\"type\":\"paragraph\",\"content\":\"Your response has been received.\"}],\"style\":{\"styleVariant\":\"subtle\"}}],\"defaultLanguage\":\"en-US\",\"language\":\"en-US\"}",
            "firstScreenJson": "{\"id\":\"cf2be73c-05e8-4dcd-abe0-5859b651aacc\",\"order\":0,\"mode\":\"scroll\",\"elements\":[{\"id\":\"0fb33eed-bd12-45fd-98e6-19888d280418\",\"type\":\"header\",\"content\":\"Header 1\"},{\"id\":\"92b9832b-f7e9-48fd-9dc9-ddfcd93c1581\",\"type\":\"paragraph\",\"content\":\"Paragraph text goes here.\"}],\"style\":{\"styleVariant\":\"balanced\"}}",
            "brandKitId": "bk-post",
            "styleVariant": "balanced",
            "letterInfo": {
                "mediaType": "application/vnd.ch.klara.epost.smartletter.draft.v1+json"
            },
            "letterType": "informative",
            "documentReferenceDate": "2026-05-06",
            "enricherPriority": "DEFAULT",
            "_folders": [],
            "_link": {
                "self": "323d80fc-bb76-457d-9dd6-6c80c5eb8396/documents/69fac6979a507a61a62a0d24"
            }
        }
```

</div>

</div>

</div>

</div>

### 2. Sender triggers sending the iLetter to recipients


![[49391239187-image-20260512-025542.png]]



When the sender triggers sending an iLetter, the “Draft“ will be converted to a correct payload to send to the OneAPI (`POST /luz_eletter/api/v2/:tenantId/synchronization-deliveries`) with the `"mediaType": "application/vnd.ch.klara.epost.smartletter.v1+json"` in metadata

### 3. The recipient replies to the iLetter


![[49391239187-image-20260512-025726.png]]



The recipients can interact with the iLetter by submitting their response using the API

`POST /{tenantId}/smart-letters/{document-id}/replies`

# Problem?

### 1. Big JSON string inside document `metadata`

The current iLetter is stored as a big JSON string in the `metadata` of the document.

<div id="expander-1167708903" class="expand-container conf-macro output-block" hasbody="true" macro-id="e834caae-f5cf-4054-9e06-1b12257fd41f" macro-name="expand">

<div id="expander-control-1167708903" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Example</span>

</div>

<div id="expander-content-1167708903" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e3a56b9b-32b0-4530-8f64-34d4b19dadbd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
            "_id": "69fac6979a507a61a62a0d24",
            "_createdBy": "nploi@axongroupio.ch",
            "_createdDate": "2026-05-06T04:41:59.761Z",
            "_updatedBy": "nploi@axongroupio.ch",
            "_updatedDate": "2026-05-07T01:19:56.946Z",
            "_deletionStatus": "false",
            "_files": {},
            "_isBasicDocument": false,
            "_isBeingCreated": false,
            "_isEnriched": false,
            "_isLargeFile": false,
            "_isTemporaryDocument": false,
            "_versionNumber": 7,
            "folderIds": [],
            "name": "My own template",
            "documentType": "SMART_LETTER_DRAFT",
            "senderEndToEndId": "4afeab7c-9f29-4903-8bdd-0f41c9ba820b",
            "letterJson": "{\"id\":\"eb1365ae-5d6e-48bc-8c6e-24fa60a2793a\",\"title\":\"My own template\",\"sourceContentUpdatedAt\":\"2026-05-07T01:19:51.224Z\",\"brandKitId\":\"bk-post\",\"screens\":[{\"id\":\"cf2be73c-05e8-4dcd-abe0-5859b651aacc\",\"order\":0,\"mode\":\"scroll\",\"elements\":[{\"id\":\"0fb33eed-bd12-45fd-98e6-19888d280418\",\"type\":\"header\",\"content\":\"Header 1\"},{\"id\":\"92b9832b-f7e9-48fd-9dc9-ddfcd93c1581\",\"type\":\"paragraph\",\"content\":\"Paragraph text goes here.\"}],\"style\":{\"styleVariant\":\"balanced\"}},{\"id\":\"b6c16e9a-9f63-47df-926f-40213d94f18e\",\"order\":1,\"mode\":\"scroll\",\"role\":\"ending\",\"elements\":[{\"id\":\"63a7d4f0-f5cf-4e9c-a4f9-6787f1c95267\",\"type\":\"header\",\"content\":\"Thank you!\"},{\"id\":\"eb713263-28f2-452b-8bbf-a3b24645c9b6\",\"type\":\"paragraph\",\"content\":\"Your response has been received.\"}],\"style\":{\"styleVariant\":\"subtle\"}}],\"defaultLanguage\":\"en-US\",\"language\":\"en-US\"}",
            "firstScreenJson": "{\"id\":\"cf2be73c-05e8-4dcd-abe0-5859b651aacc\",\"order\":0,\"mode\":\"scroll\",\"elements\":[{\"id\":\"0fb33eed-bd12-45fd-98e6-19888d280418\",\"type\":\"header\",\"content\":\"Header 1\"},{\"id\":\"92b9832b-f7e9-48fd-9dc9-ddfcd93c1581\",\"type\":\"paragraph\",\"content\":\"Paragraph text goes here.\"}],\"style\":{\"styleVariant\":\"balanced\"}}",
            "brandKitId": "bk-post",
            "styleVariant": "balanced",
            "letterInfo": {
                "mediaType": "application/vnd.ch.klara.epost.smartletter.draft.v1+json"
            },
            "letterType": "informative",
            "documentReferenceDate": "2026-05-06",
            "enricherPriority": "DEFAULT",
            "_folders": [],
            "_link": {
                "self": "323d80fc-bb76-457d-9dd6-6c80c5eb8396/documents/69fac6979a507a61a62a0d24"
            }
        }
```

</div>

</div>

</div>

</div>

→ That would lead to a huge performance issue when a letter contains multiple pages and data mapping inside.

### 2. The media is stored as an inline base64 string directly inside the letter

→ Need to find a better approach

%% ai-graph-start %%

**Related notes:**
- [[OneAPI Architecture overview]]
- [[Copy 4. Architecture for delivering eLetter after email verified]]
- [[4. Architecture for delivering eLetter after email verified]]
- [[Architecture]]
- [[History Message Format Reference eArchive 1.0 vs eArchive 2.0]]

%% ai-graph-end %%