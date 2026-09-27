---
ai_hash: 78b1ff71d68ee359
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 25
depth: 3
entities: []
relevance: 0.802
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49083154497/LUZ-146746+Use+correct+API+s+for+delete+restore+and+their+undo
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: LUZ-146746 Use correct API's for delete, restore and their undo
topic: programming
type: source
updated: 2026-01-28
---

# LUZ-146746 Use correct API's for delete, restore and their undo

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-01-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49083154497/LUZ-146746+Use+correct+API+s+for+delete+restore+and+their+undo)
> Relevance 0.802 · topic `programming`

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
<th><p><strong>Features</strong></p></th>
<th><p><strong>ePost app</strong></p></th>
<th><p><strong>WEB 1</strong></p></th>
<th><p><strong>UIB (current)</strong></p></th>
<th><p><strong>UIB - final decision</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Delete</p></td>
<td><p><strong>1. History entry:</strong></p>

![[49083154497-app_delete_history-20260127-044241.png]]


<p><strong>2. API:</strong></p>
<div id="expander-855485333" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f4a567ba-4879-4c3f-befd-1b5e7813884a" data-macro-name="expand">
<div id="expander-control-855485333" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-855485333" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> - the same <strong>Multiple</strong></p>
<p>2.2 <strong>Multiple</strong></p>
<p><a href="https://cloudlogging.app.goo.gl/nC57u9mFuiVM5whd9" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/nC57u9mFuiVM5whd9</a></p>
<ul>
<li><p>call <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/v2/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters?is-permanent=false</code></p>
<ul>
<li><p>call <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters/6923a93d1ad9f703ec8ec5e1?is-permanent=false</code></p></li>
</ul></li>
</ul>
</div>
</div>
<p>____________________________________________</p>
<p>➡️ ePost app uses delete multiple letters<span> <span class="inline-comment-marker" data-ref="a69e117d-d234-47fd-91ed-b50942e68b3c">(V2)</span></span></p>
<p><code>DELETE</code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//letters/delete_api_v2__company_tenant_id__letters" class="external-link" rel="nofollow">/api/v2/{company-tenant-id}/letters</a>?is-permanent=false</p></td>
<td><p><strong>1. History entry:</strong></p>

![[49083154497-client1_delete_history-20260127-062903.png]]


<p><strong>2. API:</strong></p>
<div id="expander-820208261" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="2fa60a58-0ca6-4b20-81c0-8e7f7f9e9199" data-macro-name="expand">
<div id="expander-control-820208261" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-820208261" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> <a href="https://cloudlogging.app.goo.gl/VB5fxt8UX1Du8rpk6" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/VB5fxt8UX1Du8rpk6</a></p>
<ul>
<li><p>call <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters/694b364a53f6ba303637bf6f?languageCode=en&amp;language=en</code></p></li>
</ul>
<p>2.2. <strong>Multiple</strong> <a href="https://cloudlogging.app.goo.gl/KXNt1Ei4gpdxfcXs7" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/KXNt1Ei4gpdxfcXs7</a></p>
<ul>
<li><p>call <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters?languageCode=en&amp;language=en&amp;is-permanent=false</code></p>
<ul>
<li><p>make multiple calls per letter <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters/694b364a53f6ba303637bf6f?is-permanent=false</code></p></li>
</ul></li>
</ul>
</div>
</div>
<p>_________________________________________</p>
<p>➡️ WEB 1 uses delete multiple letters</p>
<p><code>DELETE</code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//letters/delete_api__company_tenant_id__letters" class="external-link" rel="nofollow">/api/{company-tenant-id}/letters</a>?is-permanent=false</p></td>
<td><p><strong>1. History entry:</strong></p>

![[49083154497-uib_delete_history-20260127-070332.png]]


<p><strong>2. API:</strong></p>
<div id="expander-826189825" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="f4726df7-5292-4a68-ab20-ddf32f2ee4a6" data-macro-name="expand">
<div id="expander-control-826189825" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-826189825" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> - the same <strong>Multiple</strong></p>
<p>2.2 <strong>Multiple</strong> <a href="https://cloudlogging.app.goo.gl/bLWmBVetUhFUB1aN6" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/bLWmBVetUhFUB1aN6</a></p>
<ul>
<li><p>call <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters?is-permanent=false</code></p>
<ul>
<li><p>make multiple calls per letter <code>luz-uri=DELETE </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters/68fad2485e27ba11cd1aeb60?is-permanent=false</code></p></li>
</ul></li>
</ul>
</div>
</div>
<p>___________________________________________</p>
<p>➡️ UIB uses delete multiple letters</p>
<p><code>DELETE</code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//letters/delete_api__company_tenant_id__letters" class="external-link" rel="nofollow">/api/{company-tenant-id}/letters</a>?is-permanent=false</p></td>
<td><ul>
<li><p>Team choose to follow APP with v2</p>
<ul>
<li><p>Reason: v2 support for select all</p></li>
</ul></li>
</ul>
<p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a>Do you agree?</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Undo-delete</p></td>
<td><p><strong>1. History entry:</strong></p>

![[49083154497-app_undo_delete_history-20260127-111222.png]]


<p><strong>2. API:</strong></p>
<div id="expander-809365270" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7cf5e34a-f58f-418f-8551-41e58152e527" data-macro-name="expand">
<div id="expander-control-809365270" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-809365270" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> <a href="https://cloudlogging.app.goo.gl/ZCFn8vMhgdoC842u5" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/ZCFn8vMhgdoC842u5</a></p>
<ul>
<li><p>call <code>luz-uri=POST </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters/694b364a53f6ba303637bf6f/deletion-undoing</code></p></li>
</ul>
<p>2.2 <strong>Multiple</strong> 

![[49083154497-error.png]]

</p>
</div>
</div>
<p>____________________________________________</p>
<p>➡️ ePost app uses deletion-undoing single letter</p>
<p><code>POST </code><a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//letters/post_api__company_tenant_id__letters__id__deletion_undoing" class="external-link" rel="nofollow">/api/{company-tenant-id}/letters/{id}/deletion-undoing</a></p></td>
<td><p><strong>1. History entry:</strong></p>

![[49083154497-web1_undo_delete_history-20260127-111557.png]]


<p><strong>2. API:</strong></p>
<div id="expander-2125519214" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="daefed44-dd0a-4fdf-8f73-7e0c61d1632a" data-macro-name="expand">
<div id="expander-control-2125519214" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-2125519214" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong></p>
<p><a href="https://cloudlogging.app.goo.gl/5LUciWLroQfUt4qK7" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/5LUciWLroQfUt4qK7</a></p>
<ul>
<li><p>call <code>luz-uri=POST </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/letters/694b364a53f6ba303637bf6f/deletion-undoing?languageCode=en&amp;language=en</code></p></li>
</ul>
<p>2.2 <strong>Multiple</strong> 

![[49083154497-error.png]]

</p>
</div>
</div>
<p>_________________________________________</p>
<p>➡️ WEB 1 uses deletion-undoing single letter</p>
<p><code>POST </code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//letters/post_api__company_tenant_id__letters__id__deletion_undoing" class="external-link" rel="nofollow">/api/{company-tenant-id}/letters/{id}/deletion-undoing</a></p></td>
<td><p><strong>1. History entry:</strong></p>

![[49083154497-uib_undo_delete_history-20260127-111650.png]]


<p><strong>2. API:</strong></p>
<div id="expander-990756483" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="7244fa01-9536-4025-b6d5-34bf3820a097" data-macro-name="expand">
<div id="expander-control-990756483" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-990756483" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> - the same <strong>Multiple</strong></p>
<p>2.2 <strong>Multiple</strong> <a href="https://cloudlogging.app.goo.gl/G8KC1zSYW3sqrxi88" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/G8KC1zSYW3sqrxi88</a></p>
<ul>
<li><p>make multiple calls per letter</p>
<p><code>luz-uri=POST </code>/luz_docs_view_controller/api/290faa7c-f87f-4b5e-9e36-d599c8ea87f1<code>/trash/letters/recovery?hasRootStorage=false&amp;isStored=false</code></p></li>
</ul>
</div>
</div>
<p>___________________________________________</p>
<p>➡️ UIB uses recovery multiple letters</p>
<p><code>POST </code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//trash/post_api__company_tenant_id__trash_letters_recovery" class="external-link" rel="nofollow">/api/{company-tenant-id}/trash/letters/recovery</a></p></td>
<td><p><span class="inline-comment-marker" data-ref="9b811702-b104-4ee5-b2c1-51ee205b1e9e">create new deletion-undoing apis</span></p></td>
</tr>
<tr>
<td>3</td>
<td><p>Restore</p></td>
<td><ol>
<li><p><strong>History entry:</strong> restore to <code>eArchive</code></p></li>
</ol>

![[49083154497-app_restore_history-20260127-105301.png]]


<ol start="2">
<li><p><strong>API:</strong></p></li>
</ol>
<div id="expander-321452878" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="334b6634-01c8-4b12-bf19-c589eeead902" data-macro-name="expand">
<div id="expander-control-321452878" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-321452878" class="expand-content expand-hidden">
<p>2.1 <strong>Single</strong> <a href="https://cloudlogging.app.goo.gl/ADx1TvBhUkEsy86PA" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/ADx1TvBhUkEsy86PA</a></p>
<ul>
<li><p>call <code>luz-uri=POST </code><span class="inline-comment-marker" data-ref="2ec58f71-f1d3-4f16-b051-bea5959ef252">/luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14</span><span class="inline-comment-marker" data-ref="2ec58f71-f1d3-4f16-b051-bea5959ef252"><code>/trash/letters/6859f3c40a064e2487a6483f/recovery?hasRootStorage=true</code></span></p></li>
</ul>
<p>2.2 <strong>Multiple</strong></p>
<ul>
<li><p>/luz_docs_view_controller/api/v2/4656a299-90d2-49f9-9472-9ab27e9dfd14/trash/recovery?hasRootStorage=false</p>
<ul>
<li><p>luz-uri=POST /luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14/trash/letters/6974157959c692067ca33466/recovery?hasRootStorage=false&amp;isStored=false</p></li>
<li><p>luz-uri=POST /luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14/trash/letters/68aa71dcd3bd367b3de0e9a0/recovery?hasRootStorage=false&amp;isStored=false</p></li>
</ul></li>
</ul>
</div>
</div>
<p>____________________________________________</p>
<p>➡️ ePost app uses recovery single letter</p>
<p><code>POST </code><a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//trash/post_api__company_tenant_id__trash_letters_recovery" class="external-link" rel="nofollow">/api/{company-tenant-id}/trash/letters/recovery</a>?hasRootStorage=true</p>
<p><br />
➡️ ePost app uses recovery multiple letter:</p>
<p><code>POST </code><a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//trash/post_api__company_tenant_id__trash_letters_recovery" class="external-link" rel="nofollow">/api/v2/{company-tenant-id}/trash/recovery</a>?hasRootStorage=true</p></td>
<td><ol>
<li><p><strong>History entry:</strong></p></li>
</ol>

![[49083154497-web1_restore_history-20260127-104115.png]]


<ol start="2">
<li><p><strong>API:</strong></p></li>
</ol>
<div id="expander-1970787893" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="55fc3df5-5900-4017-aaa4-eb784b5ca7df" data-macro-name="expand">
<div id="expander-control-1970787893" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-1970787893" class="expand-content expand-hidden">
<p>2.1 <strong>Single</strong> <a href="https://cloudlogging.app.goo.gl/B2SGwyrrJimDRZVV6" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/B2SGwyrrJimDRZVV6</a></p>
<ul>
<li><p>call <code>luz-uri=POST </code>/luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14<code>/trash/letters/6859f3c40a064e2487a6483f/recovery?languageCode=en&amp;language=en&amp;</code><span class="inline-comment-marker" data-ref="9f98dcc0-a46a-4252-8655-482cf1ac2666"><code>hasRootStorage=false</code></span></p></li>
</ul>
<p>2.2 <strong>Multiple</strong> 

![[49083154497-error.png]]

</p>
</div>
</div>
<p>_________________________________________</p>
<p>➡️ WEB 1 uses recovery single letter</p>
<p><code>POST </code><a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//trash/post_api__company_tenant_id__trash_letters_recovery" class="external-link" rel="nofollow">/api/{company-tenant-id}/trash/letters/recovery</a>?hasRootStorage=false<br />
</p></td>
<td><ol>
<li><p><strong>History entry:</strong></p></li>
</ol>

![[49083154497-uib_restore_history-20260127-103549.png]]


<ol start="2">
<li><p><strong>API:</strong></p></li>
</ol>
<div id="expander-973631798" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="0772d682-11b2-469d-9b08-09753dd1fad1" data-macro-name="expand">
<div id="expander-control-973631798" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-973631798" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> - the same <strong>Multiple</strong></p>
<p>2.2 <strong>Multiple</strong> <a href="https://cloudlogging.app.goo.gl/oRJejW9SyEBsAgTJ9" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/oRJejW9SyEBsAgTJ9</a></p>
<ul>
<li><p>make multiple calls per letter <code>luz-uri=POST </code>/luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14<code>/trash/letters/recovery?hasRootStorage=true</code></p></li>
</ul>
</div>
</div>
<p>___________________________________________</p>
<p>➡️ UIB uses recovery multiple letters</p>
<p><code>POST </code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//trash/post_api__company_tenant_id__trash_letters_recovery?hasRootStorage=true" class="external-link" rel="nofollow">/api/{company-tenant-id}/trash/letters/recovery</a>?hasRootStorage=true</p></td>
<td><ul>
<li><p>Epost App can <strong>ONLY</strong> restore to eArchive</p></li>
<li><p>Web1 can restore back the original place<br />
</p></li>
</ul>
<p>→ <span class="inline-comment-marker" data-ref="eb2e9ffd-8e6b-4f70-be0e-53267ba8d7b7"><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a></span><span class="inline-comment-marker" data-ref="eb2e9ffd-8e6b-4f70-be0e-53267ba8d7b7"> could you please confirm on business side?</span></p>
<p><strong>NOTE:</strong></p>
<p>for restore letter we have 3 different</p>
<p>api call on 3 platform.<br />
<strong>WEB1:</strong> <strong>call recovery single letter</strong><br />
- response: an Object LetterStorageModification (use for undo-recovery)</p>
<p><strong><span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">Epost App:</span></strong><span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd"> </span><strong><span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">call recovery letters V2</span></strong><br />
<span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">Response: 200 OK no entity.</span><br />
<span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">But can </span><strong><span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">ONLY</span></strong><span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd"> restore to eArchive.</span><br />
<br />
<strong><span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">UIB: call loop on recovery letters V1.</span></strong><br />
<span class="inline-comment-marker" data-ref="ec9bf887-6cc8-415e-84bb-da1a2eaa9bcd">Response: 200 OK no entity.</span></p></td>
</tr>
<tr>
<td>4</td>
<td><p>Undo-restore</p></td>
<td><p>

![[49083154497-error.png]]

</p></td>
<td><ol>
<li><p><strong>History entry:</strong></p></li>
</ol>

![[49083154497-image-20260127-101407.png]]


<ol start="2">
<li><p><strong>API:</strong></p></li>
</ol>
<div id="expander-470892495" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="9b0a70b4-5f66-48c1-917f-3e56efa6ea0e" data-macro-name="expand">
<div id="expander-control-470892495" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-470892495" class="expand-content expand-hidden">
<p>2.1 <strong>Single</strong></p>
<p><a href="https://cloudlogging.app.goo.gl/NUfGptkyEWtSLYnK8" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/NUfGptkyEWtSLYnK8</a></p>
<ul>
<li><p>call <code>luz-uri=POST </code>/luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14<code>/trash/letters/69786c17b196e65f12781402/undo-recovery?languageCode=en&amp;language=en</code></p></li>
</ul>
<p>2.2 <strong>Multiple</strong> 

![[49083154497-error.png]]

</p>
</div>
</div>
<p><br />
_________________________________________</p>
<p>➡️ WEB 1 uses undo-recovery single letter</p>
<p><code>POST </code><a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//trash/post_api__company_tenant_id__trash_letters__letterId__undo_recovery" class="external-link" rel="nofollow">/api/{company-tenant-id}/trash/letters/{letterId}/undo-recovery</a></p></td>
<td><ol>
<li><p><strong>History entry:</strong></p></li>
</ol>

![[49083154497-uib_undo_restore_history-20260127-110958.png]]


<ol start="2">
<li><p><strong>API:</strong></p></li>
</ol>
<div id="expander-422359142" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="aeabfead-6f21-41b3-8164-c3a981b3d187" data-macro-name="expand">
<div id="expander-control-422359142" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">log &amp; info</span>
</div>
<div id="expander-content-422359142" class="expand-content expand-hidden">
<p>2.1. <strong>Single</strong> - the same <strong>Multiple</strong></p>
<p>2.2 <strong>Multiple</strong> <a href="https://cloudlogging.app.goo.gl/H1k7aFyJuSN99VL8A" class="external-link" rel="nofollow">https://cloudlogging.app.goo.gl/H1k7aFyJuSN99VL8A</a></p>
<ul>
<li><p>make multiple calls per letter</p>
<p><code>luz-uri=DELETE </code>/luz_docs_view_controller/api/4656a299-90d2-49f9-9472-9ab27e9dfd14<code>/letters?is-permanent=false</code></p></li>
</ul>
</div>
</div>
<p>___________________________________________</p>
<p>➡️ UIB uses delete multiple letters</p>
<p><code>DELETE</code> <a href="https://devportal.klara.ch/catalog/default/api/luz_docs_view_controller/definition#//letters/delete_api__company_tenant_id__letters" class="external-link" rel="nofollow">/api/{company-tenant-id}/letters</a>?is-permanent=false</p></td>
<td><ul>
<li><p>create new undo-recovery apis</p></li>
<li><p>Team: <strong>will</strong> <strong>follow web1</strong></p></li>
</ul>
<p><a href="https://axonivy.atlassian.net/wiki/people/557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:1c1de854-1bab-4cef-bf34-e9c1578b30f2" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Michael Dänzer</a></p>
<p><span class="inline-comment-marker" data-ref="22c0e16d-e1be-4309-a99a-198f623c0c08">Do you agree?</span></p>
<p><strong>NOTE:</strong><br />
For undo-recovery we have 3 different behavior between 3 platform:</p>
<p><strong>WEB1: undo-recovery for single letter</strong><br />
Request send an Object LetterStorageModification</p>
<p><strong>Epost App: not support</strong> 

![[49083154497-forbidden.png]]

</p>
<p><strong>UIB: should support recovery multiple lettes</strong><br />
we need to call loop api undo-recovery<br />
but we don’t have Object LetterStorageModification 

![[49083154497-help_16.png]]

</p></td>
</tr>
<tr>
<td>5</td>
<td><p>hard delete</p></td>
<td><p>support on eArchive/trash folder</p>
<p>API trigger from APP to luz-docs-view-controller through luz-mylife-epost-adapter</p>
<p>confirmed from Optimus:</p>
<p>delete a letter</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="aab13341-9479-4870-a6be-60f2b5c1c61b" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>@DELETE /luz_docs_view_controller/api/{{tenant-id}}/letters/{{letter-id}}?is-permanent=true</code></pre>
</div>
</div>
<p>delete multi letters → this api also support for folders</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f1c06125-c00c-4056-8476-c42273d9b1d0" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>@DELETE /luz_docs_view_controller/api/{{tenant-id}}/storage?is-permanent=true
&#10;payload: { &quot;letterIds&quot;: [&quot;letter-123&quot;]}</code></pre>
</div>
</div>
<p>Team’s comment:</p>
<p>as double checked there are another API to delete multi letters, it’s also reuse the same service like the “storage api” above → we will use this since only need to care about the letter (for more info, we put in the comment <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49083154497_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-145466" data-macro-id="4921a068-1982-4e7b-b51b-4b5547b6095d" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-145466" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-145466</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> )</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8b1800df-9c79-4b90-9613-85a2a6157446" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>@DELETE /luz_docs_view_controller/api/{{tenant-id}}/letters?is-permanent=true
payload: [&quot;letterId1&quot;, &quot;letterId2&quot;, &quot;letterId3&quot;]</code></pre>
</div>
</div></td>
<td><p>do not support in UI</p>
<p>luz-docs will trigger eletter deletion for those document has been deleted 30 days or more</p>
<p>→ use query to delete eletter</p></td>
<td><p>API trigger to luz-docs-view-controller through luz-unified-inbox</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e9136739-f764-45d1-9427-e5c899634186" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>@DELETE /luz_docs_view_controller/api/{{tenant-id}}/letters/{{letter-id}}?is-permanent=true</code></pre>
</div>
</div></td>
<td><p>luz-unified-inbox will adapt for the multi-letters case to consume correct API</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Enhancements for API Delete and Restore]]
- [[LUZ-115505 Public API - letterbox Part 3]]
- [[Empty Trash APIs]]
- [[UIB LUZ-146496 - eLetter inline edit]]
- [[Public API - letterbox - API get deleted letters from trash]]

%% ai-graph-end %%