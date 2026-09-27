---
ai_hash: 369a19cad675fc0e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 14
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49288937474/Self-test+termination+luz_ebill+and+Script+switch+to+primary+NWP
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Self-test termination luz_ebill, and Script switch to primary NWP
topic: programming
type: source
updated: 2026-04-03
---

# Self-test termination luz_ebill, and Script switch to primary NWP

> [!info] Imported from Confluence
> Space **TS** · updated 2026-04-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49288937474/Self-test+termination+luz_ebill+and+Script+switch+to+primary+NWP)
> Relevance 0.731 · topic `programming`

## I. Self test termination luz_ebill

luz_ebill will be terminated on 30.04.2026 (make simulate 31/03.2026 for dev)

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
<th><p><strong>No</strong></p></th>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Module</strong></p></th>
<th><p><strong>Steps Invoice</strong></p></th>
<th><p><strong>Recuring invoice</strong></p></th>
<th><p><strong>Info</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>New user start eBill onboarding to send eBill invoice to one api</p></td>
<td><p>luz_ebill_networkpartner</p></td>
<td><ul>
<li><p>Send eBill invoice to one api success

![[49288937474-check.png]]

</p></li>
</ul></td>
<td><ul>
<li><p>Send EBILL invoice successfully with FS (<code>klara:ebill:networkpartner</code>)

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-image-20260403-042306.png]]

![[49288937474-image-20260403-042334.png]]

</td>
<td><p>tenant: 761d8caa-47f1-4cc2-9d4a-91dcf121a621</p>
<p>name: EBILL M11</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>User already start eBill onboarding and send eBill invoice to one api</p></td>
<td><p>luz_ebill_networkpartner</p></td>
<td><ul>
<li><p>Send eBill invoice to one api success

![[49288937474-check.png]]

</p></li>
</ul></td>
<td><ul>
<li><p>Send EBILL invoice successfully with FS (<code>klara:ebill:networkpartner</code>)

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-image-20260403-041554.png]]

![[49288937474-image-20260403-041616.png]]

</td>
<td><p>tenant: <code>530a93f1-70d9-4282-b4f0-bf778b2cb4a9</code></p>
<p>name: QRIBAN Assign biller by primary 2</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>User already onboaring and send eBill Avalog(not start migrate to one api)</p></td>
<td><p>luz_ebill</p></td>
<td><ul>
<li><p>Invoice: See this banner at the Send invoice step when creating and sending an invoice.

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-edf07d76-5d9d-4408-90cb-99f9eef0956a#media-blob-url=true&id=aa5bd28c-fb16-4365-9.png]]


<ul>
<li><p>Update the Iban setting</p></li>
</ul></td>
<td><ul>
<li><p>Do not send EBILL because FS (<code>klara:ebill:networkpartner</code>) is not turned on 

![[49288937474-check.png]]

</p></li>
</ul></td>
<td><p>tenant: 8d1ea93f-dc03-4e6b-9197-b6977f08ce83</p>
<p>name: <code>EBILL Avalog3 not migrate</code></p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>User already onboaring and send eBill Avalog and migrate to send eBill invoice to one api( not switch to primary NWP)</p></td>
<td><p>luz_ebill, luz_ebill_networkpartner</p></td>
<td><ul>
<li><p>See this banner at the Send invoice step when creating and sending an invoice.

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-edf07d76-5d9d-4408-90cb-99f9eef0956a#media-blob-url=true&id=aa5bd28c-fb16-4365-9.png]]


<ul>
<li><p>Update the Iban setting</p></li>
</ul></td>
<td><ul>
<li><p>Do not send EBILL because Avalog was terminated<br />
FS (<code>klara:ebill:networkpartner</code>) is turned on 

![[49288937474-check.png]]

</p>

![[49288937474-image-20260403-035621.png]]

![[49288937474-image-20260403-040847.png]]


<ul>
<li><p>Send EBILL invoice successfully if change Date terminated (send invoice on 03.04.2026, change terminated to 10.04.2026)<br />
</p>

![[49288937474-image-20260403-045616.png]]

</li>
</ul></li>
</ul></td>
<td><p>tenant: bdb9f12b-b2ef-4ce5-9720-604cd6eeb905</p>
<p>name: EBILL Avalog1</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><ol>
<li><p>User already onboaring and send eBill Avalog and migrate to send eBill invoice to one api</p></li>
<li><p>Trigger script switch to primary NWP</p></li>
</ol>
<p><strong>Note</strong>: After a long, long discussion: How can we simulate flow <a href="https://axonivy.atlassian.net/wiki/people/605414fe66c87900683d008b?ref=confluence" class="confluence-userlink user-mention" data-account-id="605414fe66c87900683d008b" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Gianfranco Gaio</a></p>
<ol>
<li><p>Create EBILL on Primary NWP, send EBILL invoice via one API</p></li>
<li><p>Inform SIX to switch, then SIX will set <code>billRecipientSubscriptionStatus</code> to <code>NOT_ALLOWED</code></p></li>
<li><p>Trigger api to update with <code>billRecipientSubscriptionStatus</code> is <code>ALLOWED</code></p></li>
</ol>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="21d60279-243c-4516-ae35-af04552742ca" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>List tenant create primary NWP to simulate switch to: 
CH8531000026246206313 CHE-228.504.768 EBILL M1  BIID0000132299 c06e3c88-8f7e-4643-a21b-97f1fbe78894 
CH2530002585994891271 CHE-115.401.584 EBILL M2  BIID0000132298 3c6660b7-90ea-4784-a4ea-1f70f13cc552 
CH4130790793262232223 CHE-377.401.559 EBILL M3  BIID0000132297 5efdc4cd-514f-4abb-83be-a14187540708 
CH9731000471555361298 CHE-413.225.510 EBILL M4  BIID0000132296 16a4d461-0937-4bb9-a4d7-21c6412be8da 
CH8030002533023637160 CHE-446.322.373 EBILL M5  BIID0000132295 c7c14b8a-5c42-48a7-8d8a-64c19fb71f3a 
CH4430003886786252075 CHE-335.887.819 EBILL M6  BIID0000132294 b5e04d5e-bfba-4f93-8a60-7aae193aba7b 
CH4430790435662554805 CHE-181.742.264 EBILL M7  BIID0000132293 0940cc31-cd09-4b7d-a9a7-0deb1d6f37e7 
CH8831000029265385870 CHE-420.397.688 EBILL M8  BIID0000132292 79184e1f-d75e-4336-900c-f1b51a831a2e 
CH6230000123547434161 CHE-353.456.096 EBILL M9  BIID0000132291 594bfad6-b6d6-42e9-8fde-c84975565a3b 
CH0230790463310299497 CHE-113.746.440 EBILL M10 BIID0000132290 2b1bc3ce-a6dc-4830-ae63-cddb51e143dd</code></pre>
</div>
</div></td>
<td></td>
<td><ul>
<li><p>Send invoice to one api success

![[49288937474-check.png]]

</p></li>
</ul></td>
<td><ul>
<li><p>Send EBILL invoice successfully with FS (<code>klara:ebill:networkpartner</code>)

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-image-20260403-070126.png]]

</td>
<td><p>tenant:</p>
<p><code>c06e3c88-8f7e-4643-a21b-97f1fbe78894</code></p>
<p><code>3c6660b7-90ea-4784-a4ea-1f70f13cc552</code></p></td>
</tr>
</tbody>
</table>

</div>

## II. Script switch to primary

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
<th><p><strong>Cron job</strong></p></th>
<th><p><strong>Module switch to primary NWP</strong></p></th>
</tr>
&#10;<tr>
<td><p>tenants switch to primary</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="b1297722-43a2-4359-8304-2659f22c94b6" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>c06e3c88-8f7e-4643-a21b-97f1fbe78894 EBILL M1
3c6660b7-90ea-4784-a4ea-1f70f13cc552 EBILL M2</code></pre>
</div>
</div></td>
<td><ul>
<li><p>Trigger script switch to primary NWP: <code>switch-primary-nwp-job</code>

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-image-20260402-083822.png]]

![[49288937474-image-20260402-083859.png]]


<p><br />
</p>
<p><br />
</p></td>
<td><ul>
<li><p>Result <code>switch-primary-nwp-job</code>

![[49288937474-check.png]]

</p></li>
</ul>

![[49288937474-image-20260402-083740.png]]


<ul>
<li><p>Result <code>luz-ebill-networkpartner</code></p>

![[49288937474-image-20260402-084429.png]]

</li>
</ul></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Preview Delivery-Prices API - ForcedOnboading]]
- [[Invoice Run V2UAT - Update latest Stimulsoft template - Execution]]
- [[Invoice Run V2UAT - Update latest Stimulsoft template]]
- [[LUZ-110826 Public API - Widget subscription by activation code]]
- [[CROSS-TEST LUZ-158644 Investigate and remove Print&Send user role (UI, backend, Public API — no]]

%% ai-graph-end %%