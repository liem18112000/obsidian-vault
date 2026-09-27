---
title: "[CROSS-TEST] [LUZ-158644] Investigate and remove Print&Send user role (UI, backend, Public API — no migration)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49725702215/CROSS-TEST+LUZ-158644+Investigate+and+remove+Print+Send+user+role+UI+backend+Public+API+no+migration
space: "TS"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2026-09-07
attachments: 24
tags:
  - confluence
  - programming
  - space/ts
---

# [CROSS-TEST] [LUZ-158644] Investigate and remove Print&Send user role (UI, backend, Public API — no migration)

> [!info] Imported from Confluence
> Space **TS** · updated 2026-09-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49725702215/CROSS-TEST+LUZ-158644+Investigate+and+remove+Print+Send+user+role+UI+backend+Public+API+no+migration)
> Relevance 0.731 · topic `programming`

Related US: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49725702215_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-158644" macro-id="f13adb20-aa32-4d64-a240-ce18ac94e76a" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-158644" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-158644</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

**Scope.** Webclient 1 (`luz_components`) covers **both** KLARA and ePost — `print_and_send_basic` is one shared role, so the removal is a single line sitting outside the KLARA/ePost branch. Webclient 2 covers **ePost only** (`apps/luz-epost`); `apps/luz-klara` is out of scope for this story. The Public API change is limited to the printer-driver tenant lookup.

**Environments.** V1 KLARA `dev.klara.tech` · V1 ePost `dev.klara-epost.tech` · V2 ePost `client-dev.klara-epost.tech` · Public API dev.

<div hasbody="true" macro-id="5fa815fa-a064-430e-a9f7-65213bf0acfd" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**PO decisions and discovery outcomes that override a literal reading of the story description — test these, not the description.**

- **The block condition is not an only-role check.** The screen appears when the user has `PRINT_AND_SEND_BASIC` and does **not** have `COMPANY_ADMINISTRATOR`. A user holding `epost_letterbox + print_and_send_basic` **is** blocked; a company administrator holding the role is **exempt**, so the tenant can still fix itself.

- **Holders keep displaying the role.** Assignment and display are separate paths. Users and API keys that already hold Print & Send must still show it in the lists — only the assignable list loses it.

- **The PO narrowed the Public API scope** (comment 27.08.2026: no known API use of the role). Only the printer-driver tenant lookup stops honouring it; there is no global role filter, and the endpoint itself stays alive for the other roles.

- **No feature switch and no migration.** A rollback here is a revert, not a flag flip — which makes the sibling sub-task LUZ-158909 "Set release flag" obsolete as written.

- **Wording differs per app:** ePost uses the formal «Sie» and the name ePost, KLARA uses the informal «du» and the name KLARA. The title text is the same in both.

- **Open with the PO, not blocking:** LUZFB-4224 named only `app.epost.ch` and `client.epost.ch`, so covering KLARA in V1 is a deliberate widening of the requested scope.

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
<td><p>V1 ePost — Print &amp; Send no longer offered when creating or editing a user</p></td>
<td><ol>
<li><p>Log in to <code>dev.klara-epost.tech</code> as a company administrator.</p></li>
<li><p>Open User Management and click <strong>Add new user</strong>.</p></li>
<li><p>Read the role checkbox list.</p></li>
<li><p>Cancel, open an existing user for editing and read the role checkbox list again.</p></li>
</ol></td>
<td><p>Neither screen offers a "Print &amp; Send" checkbox.</p></td>
<td><p>As expected</p></td>
<td>

![[49725702215-image-20260904-095853.png]]

![[49725702215-image-20260904-095945.png]]

</td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td><p>V1 KLARA — role gone, remaining KLARA roles untouched</p></td>
<td><ol>
<li><p>Log in to <code>dev.klara.tech</code> as a company administrator.</p></li>
<li><p>Open User Management and click <strong>Add new user</strong>.</p></li>
<li><p>Read the role checkbox list.</p></li>
<li><p>Repeat on the edit-user screen.</p></li>
</ol></td>
<td><p>No "Print &amp; Send" checkbox on either screen. Every other KLARA role is still offered.</p></td>
<td><p>As expected</p></td>
<td>

![[49725702215-image-20260904-100434.png]]

![[49725702215-image-20260904-100513.png]]

</td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>3</td>
<td><p>V1 — Print &amp; Send no longer offered when creating an API key</p></td>
<td><ol>
<li><p>On <code>dev.klara-epost.tech</code>, open API key management and start creating a new API key.</p></li>
<li><p>Read the role list on the form.</p></li>
<li><p>Create the key with one of the remaining roles.</p></li>
<li><p>Repeat the same steps on <code>dev.klara.tech</code>.</p></li>
</ol></td>
<td><p>"Print &amp; Send" is not selectable on either app, the remaining roles are unchanged, and the key is created successfully.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-klara-case-3.mp4">remove-print-and-send-klara-case-3.mp4</a></span><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-epost-case-3.mp4">remove-print-and-send-epost-case-3.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>4</td>
<td><p>V1 — existing holders are still <em>displayed</em> with the role</p></td>
<td><ol>
<li><p>On <code>dev.klara-epost.tech</code>, open the User Management list.</p></li>
<li><p>Find a test user that already holds Print &amp; Send and read the role column.</p></li>
<li><p>Open the API key list and read the role column of a test key that already holds it.</p></li>
</ol></td>
<td><p>Both still render "Print &amp; Send" as a properly translated role name — not a raw CMS path, not a blank cell. The display path and its CMS key must be untouched by this change.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-4.mp4">remove-print-and-send-case-4.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>5</td>
<td><p>V1 — saving an existing holder strips the role (intended)</p></td>
<td><ol>
<li><p>On <code>dev.klara-epost.tech</code>, open a test user that holds Print &amp; Send plus at least one other role.</p></li>
<li><p>Change only the e-mail address.</p></li>
<li><p>Save, then reopen the user list.</p></li>
</ol></td>
<td><p>The save succeeds; Print &amp; Send is gone from that user afterwards while the other roles are kept. No preserve workaround is expected — this is the intended V1 behaviour, not a defect.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>6</td>
<td><p>V2 — Invite member dialog no longer offers the role</p></td>
<td><ol>
<li><p>Log in to <code>client-dev.klara-epost.tech</code> as a company administrator.</p></li>
<li><p>Open Organization → Team Members → <strong>Invite</strong>.</p></li>
<li><p>Choose the <strong>Custom</strong> permission type and read the checkbox list.</p></li>
</ol></td>
<td><p>There is no Print &amp; Send entry and no untranslated key left in its place.</p></td>
<td><p>As expected</p></td>
<td>

![[49725702215-image-20260904-102137.png]]

</td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>7</td>
<td><p>V2 — member Access Rights panel no longer lists the role</p></td>
<td><ol>
<li><p>On <code>client-dev.klara-epost.tech</code>, open Organization → Team Members.</p></li>
<li><p>Open a member and switch to the <strong>Access Rights</strong> tab.</p></li>
<li><p>Read the assignable rights.</p></li>
</ol></td>
<td><p>Print &amp; Send cannot be granted or re-granted from the panel.</p></td>
<td><p>As expected</p></td>
<td>

![[49725702215-image-20260904-102301.png]]

</td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>8</td>
<td><p>V2 — API key creation dialog no longer offers the role</p></td>
<td><ol>
<li><p>On <code>client-dev.klara-epost.tech</code>, open Organization → API Keys.</p></li>
<li><p>Click <strong>Create</strong> and choose the <strong>Custom</strong> permission type.</p></li>
<li><p>Read the checkbox list, then create a key with SmartSend.</p></li>
</ol></td>
<td><p>Only ePost Letterbox and SmartSend are offered, and the key is created successfully with the selected role.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-8.mp4">remove-print-and-send-case-8.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>9</td>
<td><p>V2 — existing members still <em>displays</em> Print &amp; Send</p></td>
<td><ol>
<li><p>On <code>client-dev.klara-epost.tech</code>, open Organization → Team Members and read the Access Rights column of a test member that holds the role.</p></li>
<li><p>Keep the browser console open while doing so.</p></li>
</ol></td>
<td><p>Both rows still show the translated label "Print &amp; Send". No missing-translation placeholder and no console error — the role must still be present in the <code>Role</code> enum.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-9.mp4">remove-print-and-send-case-9.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>10</td>
<td><p>V2 — editing a member that still holds the role does not fail server-side</p></td>
<td><ol>
<li><p>On <code>client-dev.klara-epost.tech</code>, open a test member that holds Print &amp; Send.</p></li>
<li><p>In the Access Rights panel, additionally enable SmartSend.</p></li>
<li><p>Save and watch the network tab.</p></li>
<li><p>Reload the Team Members list.</p></li>
</ol></td>
<td><p>The update succeeds — no 400 from the member update call — and the row afterwards shows SmartSend alongside the retained Print &amp; Send. This is the regression guard for keeping the role in the <code>Role</code> enum used by server-side validation.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>11</td>
<td><p>Login block — every non-admin holder is blocked</p></td>
<td><ol>
<li><p>Prepare a test user: with <code>print_and_send_basic</code>.</p></li>
<li><p>Log in on <code>dev.klara-epost.tech</code> (V1).</p></li>
<li><p>Log in on <code>client-dev.klara-epost.tech</code> (V2).</p></li>
<li><p>Log in on <code>dev.klara.tech</code> (V1 KLARA).</p></li>
</ol></td>
<td><p>These logins lands on the blocking screen. No part of the application is reachable behind the screen.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-11.mp4">remove-print-and-send-case-11.mp4</a></span><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-11-a.mp4">remove-print-and-send-case-11-a.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>12</td>
<td><p>Login block — administrators exempt, non-holders unaffected</p></td>
<td><ol>
<li><p>Log in as a test user holding <code>company_administrator + print_and_send_basic</code>, on V1 and then on V2.</p></li>
<li><p>Navigate to User Management / Team Members and open a user for editing.</p></li>
<li><p>Log in as an ordinary test user that does not hold Print &amp; Send at all, on V1 and then on V2.</p></li>
</ol></td>
<td><p>Neither user ever sees the blocking screen. The non-holder's login is completely unchanged.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-12.mp4">remove-print-and-send-case-12.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>13</td>
<td><p>Block screen shape and OK behaviour</p></td>
<td><ol>
<li><p>Log in as a Print &amp; Send-only test user until the blocking screen appears, first on V1 then on V2.</p></li>
<li><p>Look for any way out: an X in the corner, a second button, a click outside the panel, the Esc key, the browser back button.</p></li>
<li><p>Press <strong>OK</strong>.</p></li>
<li><p>Reopen the application URL in the same browser tab.</p></li>
</ol></td>
<td><p>Exactly one button, labelled OK — no X, no second button — and none of the dismiss attempts reveals the application behind it. OK signs the user out and returns them to the login screen; reopening the app asks for credentials again, i.e. the Keycloak session was genuinely destroyed rather than only navigated away from.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>14</td>
<td><p>Deep link into an inner page is still blocked</p></td>
<td><ol>
<li><p>Log in as a Print &amp; Send-only test user on <code>client-dev.klara-epost.tech</code>.</p></li>
<li><p>From the blocking screen, type <code>/en/unified-inbox</code> directly into the address bar.</p></li>
<li><p>Repeat on V1 with a direct URL to an inner page such as user management.</p></li>
</ol></td>
<td><p>Both attempts land back on the blocking screen. No inner page renders, not even for a moment before a redirect.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>15</td>
<td><p>Recovery — user regains access after the admin fixes the role</p></td>
<td><ol>
<li><p>As a company administrator, open the blocked test user and remove Print &amp; Send, assigning ePost Letterbox instead.</p></li>
<li><p>Save.</p></li>
<li><p>Log in as that user again on the same client.</p></li>
<li><p>Run this once on V1 and once on V2.</p></li>
</ol></td>
<td><p>The user reaches the application normally — no blocking screen — and the newly assigned role works. This is the only remediation path, since the story ships no migration.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>16</td>
<td><p>Block screen wording matches the app (KLARA «du» vs ePost «Sie»)</p></td>
<td><ol>
<li><p>Trigger the blocking screen on <code>dev.klara-epost.tech</code> and read the message text.</p></li>
<li><p>Trigger it on <code>dev.klara.tech</code> and read the message text.</p></li>
<li><p>Trigger it on <code>client-dev.klara-epost.tech</code> and read the message text.</p></li>
</ol></td>
<td><p>Both ePost clients show the formal «Sie» wording and name ePost; KLARA shows the informal «du» wording and names KLARA. The role name «Print&amp;Send» is spelled out in the text on every variant, and no screen shows a raw CMS path or translation key instead of a sentence.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>17</td>
<td><p>i18n — block screen in DE / EN / FR / IT</p></td>
<td><ol>
<li><p>Switch the language to DE and trigger the blocking screen.</p></li>
<li><p>Read the title, the message and the button label.</p></li>
<li><p>Repeat for EN, FR and IT.</p></li>
<li><p>Repeat the whole set on the other client.</p></li>
</ol></td>
<td><p>Title, message and the OK button are fully translated in all four locales on both clients — no fallback to English, no missing-key placeholder, and no text clipped by the layout.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-17.mp4">remove-print-and-send-case-17.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>18</td>
<td><p>i18n — role lists in DE / EN / FR / IT after the removal</p></td>
<td><ol>
<li><p>On <code>client-dev.klara-epost.tech</code>, open the Invite dialog in DE and read the role list.</p></li>
<li><p>Read the Access Rights column of a member that still holds Print &amp; Send.</p></li>
<li><p>Repeat both for EN, FR and IT.</p></li>
</ol></td>
<td><p>In every locale the assignable list shows only ePost Letterbox and SmartSend, while the existing holder's row still shows a properly translated "Print &amp; Send" label.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49725702215-remove-print-and-send-case-18.mp4">remove-print-and-send-case-18.mp4</a></span></td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>19</td>
<td><p>Public API — printer-driver lookup rejects a Print &amp; Send-only key</p></td>
<td><ol>
<li><p>Take a test API key whose role is <code>print_and_send_basic</code>.</p></li>
<li><p>Call <code>GET /core/latest/printer-driver/tenants</code> with it.</p></li>
<li><p>Read the status code and the response body.</p></li>
</ol></td>
<td><p>HTTP 403 — a clean forbidden response, not a 500 and not a 200 with an empty tenant list.</p></td>
<td><p>As expected</p></td>
<td>

![[49725702215-image-20260907-030320.png]]

</td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>20</td>
<td><p>Public API — printer-driver lookup still works for the remaining roles</p></td>
<td><ol>
<li><p>Take a test API key holding <code>company_administrator</code>.</p></li>
<li><p>Call <code>GET /core/latest/printer-driver/tenants</code> with it.</p></li>
<li><p>Repeat with a key holding <code>trusted_user</code>.</p></li>
</ol></td>
<td><p>Both return HTTP 200 with the expected tenant list. The endpoint was not retired — only the one role was dropped from the lookup.</p></td>
<td><p>As expected</p></td>
<td>

![[49725702215-image-20260907-030853.png]]

</td>
<td><p>

![[49725702215-check.png]]

</p></td>
</tr>
<tr>
<td>21</td>
<td><p>Public API — other endpoints unaffected for a key that carries the role</p></td>
<td><ol>
<li><p>Take a test API key holding <code>epost_letterbox + print_and_send_basic</code>.</p></li>
<li><p>Call a normal Public API endpoint covered by that key's widget subscription, for example an eLetter delivery call.</p></li>
<li><p>Read the status code.</p></li>
</ol></td>
<td><p>The call behaves exactly as it did before the change. The role is not blocked globally, so a key that happens to carry it keeps working on every endpoint other than the printer-driver lookup.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49725702215-check.png]]

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
