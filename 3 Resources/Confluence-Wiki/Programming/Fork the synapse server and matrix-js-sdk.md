---
ai_hash: d99ebda7e7eef49e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48386277435/Fork+the+synapse+server+and+matrix-js-sdk
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Fork the synapse server and matrix-js-sdk
topic: programming
type: source
updated: 2025-03-13
---

# Fork the synapse server and matrix-js-sdk

> [!info] Imported from Confluence
> Space **TS** · updated 2025-03-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48386277435/Fork+the+synapse+server+and+matrix-js-sdk)
> Relevance 0.738 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="f6cb2ea8-1c7a-498a-a235-ab66bf60f1f8" macro-name="toc">

</div>

# Motivation

- When implementing the E2EE feature in the eCommunities web using `matrix-js-sdk`, we’re facing a lot of problems <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/48231055878/End-to-end+encryption+status+on+eCommunities+web#Known-issues%3A" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/TS/pages/48231055878/End-to-end+encryption+status+on+eCommunities+web#Known-issues%3A</a>

- The main reason is that we’re using an unsupported official authentication type (org.matrix.login.jwt), which is not allowed to perform User Interactive Auth (UIA—a special kind of authentication in the Matrix spec). Many API endpoints in the Matrix spec require UIA to use them.

- The upload cross-signing keys endpoint (<a href="https://spec.matrix.org/v1.13/client-server-api/#post_matrixclientv3keysdevice_signingupload" class="external-link" rel="nofollow">POST /_matrix/client/v3/keys/device_signing/upload</a> ) is one of them. UIA is not always required for this endpoint:

  - The first upload or upload the keys that exactly match the existing keys → not required

  - Other cases → UIA MUST be performed


![[48386277435-image-20250307-031144.png]]



→ Because of this, if the first setup fails for some reason, this user can not upload cross-signing keys to the server anymore

- Many features like Device Dehydration require the cross-signing keys to be uploaded on the server  
  → We can not use those features if the first-time setup fails

<div hasbody="true" macro-id="1e85d683-a265-4800-acc0-3d4c46231cc2" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

The official way to solve the above issue is to switch to OIDC (supported natively in Matrix 2.0). But we don’t know exactly when Matrix 2.0 will be available.

Right now, OIDC is a standalone service called <a href="https://github.com/element-hq/matrix-authentication-service" class="external-link" rel="nofollow">Matrix Authentication Service (MAS)</a>, but it’s not stable to use.

</div>

</div>

# Overview

We’re trying out an approach that modifies the Synapse code at the endpoint <a href="https://spec.matrix.org/v1.13/client-server-api/#post_matrixclientv3keysdevice_signingupload" class="external-link" rel="nofollow">POST /_matrix/client/v3/keys/device_signing/upload</a> to bypass the UIA until OICD is natively supported in Matrix 2.0


![[48386277435-image-20250307-034659.png]]



→ We can always upload the cross-signing keys without performing UIA

# Pros and cons

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Pros</strong></p></th>
<th><p><strong>Cons</strong></p></th>
</tr>
&#10;<tr>
<td><ul>
<li><p>Can solve the current E2EE issues that we’re facing</p></li>
<li><p>Allow reset of the E2EE state on server in case the E2EE setup gets wrong for some reason (user force disrupted init, network/database issue, coding error,…)</p></li>
</ul></td>
<td><ul>
<li><p><strong>Less security</strong>: Basically, we remove a security layer (UIA) so anyone with the valid Synapse access token can always call this endpoint</p></li>
<li><p><strong>Synapse’s internal state may be broken</strong>: We don’t 100% understand the implementation of Synapse. → Maybe we break something when bypassing the UIA (haven’t seen so far)</p></li>
<li><p><strong>Legal licenses? (Need to clarify)</strong><br />
-&gt; Right now we don’t care this point</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Fork the Synapse server and matrix-js-sdk for DEV]]
- [[Communities Privacy Concept – Working Page for PO and Engineering]]

%% ai-graph-end %%