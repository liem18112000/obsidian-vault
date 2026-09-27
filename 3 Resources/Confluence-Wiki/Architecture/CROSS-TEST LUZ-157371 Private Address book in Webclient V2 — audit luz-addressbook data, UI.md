---
title: "[CROSS-TEST] [LUZ-157371] Private Address book in Webclient V2 — audit luz-addressbook data, UI proposal (Phase 1), then implement (Phase 2)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49782325255/CROSS-TEST+LUZ-157371+Private+Address+book+in+Webclient+V2+audit+luz-addressbook+data+UI+proposal+Phase+1+then+implement+Phase+2
space: "TS"
topic: architecture
relevance: 0.755
depth: 2.49
updated: 2026-09-25
attachments: 9
tags:
  - confluence
  - architecture
  - space/ts
---

# [CROSS-TEST] [LUZ-157371] Private Address book in Webclient V2 — audit luz-addressbook data, UI proposal (Phase 1), then implement (Phase 2)

> [!info] Imported from Confluence
> Space **TS** · updated 2026-09-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49782325255/CROSS-TEST+LUZ-157371+Private+Address+book+in+Webclient+V2+audit+luz-addressbook+data+UI+proposal+Phase+1+then+implement+Phase+2)
> Relevance 0.755 · topic `architecture`

Related US: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_49782325255_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-157371" macro-id="953644ab-29d2-478c-bbc2-3e6ce3e7240c" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-157371" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-157371</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

Scope: Phase 2 of this story, the private-tenant Address book in Webclient V2. Business People Hub (<a href="https://axonivy.atlassian.net/browse/LUZ-154854" class="external-link" rel="nofollow">LUZ-154854</a>) is covered only as a regression check. Compose autocomplete (LUZ-127408) is out of scope.

Design: [Phase 1 proposal](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49767874562/Private+Address+book+in+Webclient+V2+Phase+1+proposal+LUZ-157371). The look & feel must match the current Webclient, not the proposal screenshots (PO, 18.09).

<div hasbody="true" macro-id="c9fb04fb-7305-48eb-a927-49e937c7b117" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

**PO decisions from the comments that change the written spec:**

- Send letter / Chat are **hidden**, not disabled (Myriam, 22.09).

- Hide the tab navigation when there is only one tab, for business and private (Myriam, 22.09).

- Phone-synced contacts: sync is one-way, so they should **not be editable or deletable** on web, and an info bar should explain why (Robin + Gianfranco, 23.09). This came after the implementation was done, so check it is in the build before judging row 14.

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
<td><p>Entry point: Address book in main nav (private)</p></td>
<td><ol>
<li><p>Log in to Webclient V2 as a private individual user.</p></li>
<li><p>Look at the main navigation.</p></li>
<li><p>Click the <strong>Address book</strong> entry.</p></li>
</ol></td>
<td><p>An <strong>Address book</strong> entry is in the main nav, and there is no CRM People Hub entry. Clicking it opens the contact list of the user's personal ePost contacts.</p></td>
<td><p>As expected</p></td>
<td>

![[49782325255-image-20260924-101639.png]]

</td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Business user unaffected (People Hub)</p></td>
<td><ol>
<li><p>Log in as a Business tenant user.</p></li>
<li><p>Open <strong>People</strong> in the main nav.</p></li>
<li><p>Check the columns and actions.</p></li>
</ol></td>
<td><p>People Hub works as before (LUZ-154854): CRM contacts, Tags column, Import and ePost matching are there. No Address book entry is shown.</p></td>
<td><p>As expected</p></td>
<td>

![[49782325255-image-20260924-101814.png]]

</td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Contacts match the mobile app</p></td>
<td><ol>
<li><p>On the ePost mobile app (same private account), open Profile → Contact book and note the contacts, including some synced from the phone.</p></li>
<li><p>Open the Address book on web.</p></li>
<li><p>Compare the two lists.</p></li>
</ol></td>
<td><p>Web shows the same set of contacts as mobile, including phone-synced (DEVICE_SYNC) contacts, with the same names and values.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>4</td>
<td><p>List layout and columns</p></td>
<td><ol>
<li><p>Open the Address book as a private user.</p></li>
<li><p>Check the table columns and compare with People Hub for a business user.</p></li>
</ol></td>
<td><p>Same table, layout and look &amp; feel as the current Webclient People Hub, minus the CRM-only column (no Tags). The <strong>On ePost</strong> column is shown.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>5</td>
<td><p>Search by name, phone, email</p></td>
<td><ol>
<li><p>Open the Address book.</p></li>
<li><p>Search by part of a contact's name.</p></li>
<li><p>Search by a phone number.</p></li>
<li><p>Search by an email address.</p></li>
</ol></td>
<td><p>Each search returns the matching contacts only. Clearing the search shows the full list again.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49782325255-chrome_9B2z2DTD6Y.mp4">chrome_9B2z2DTD6Y.mp4</a></span></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>6</td>
<td><p>Search by address shows the explicit no-results message</p></td>
<td><ol>
<li><p>Open the Address book.</p></li>
<li><p>Search for a street, postcode or town that exists on a contact but not in its name/phone/email.</p></li>
</ol></td>
<td><p>No results are shown, and the no-results message says explicitly that search only covers name, phone and email.</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49782325255-chrome_uBULboq7NA.mp4">chrome_uBULboq7NA.mp4</a></span></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>7</td>
<td><p>Sort by name, same order as mobile</p></td>
<td><ol>
<li><p>Open the Address book with 20+ contacts.</p></li>
<li><p>Sort by name ascending, then descending.</p></li>
<li><p>Compare the ascending order with the mobile Contact book.</p></li>
</ol></td>
<td><p>Contacts are sorted by name in both directions, and the order matches mobile. No contacts stick to the top out of order (the issue PO saw with 3 contacts on 23.09).</p></td>
<td><p>As expected</p></td>
<td><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49782325255-chrome_OEu6mYgPEB.mp4">chrome_OEu6mYgPEB.mp4</a></span></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>8</td>
<td><p>Many contacts load fully</p></td>
<td><ol>
<li><p>Use an account with more contacts than fit on one page.</p></li>
<li><p>Scroll or page through the list to the end.</p></li>
</ol></td>
<td><p>All contacts can be reached, with no duplicates and no missing entries. The total matches mobile.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>9</td>
<td><p>Create contact with several emails/phones/addresses</p></td>
<td><ol>
<li><p>Click <strong>Add contact</strong>.</p></li>
<li><p>Enter a name, 2 emails, 2 phone numbers and 2 addresses.</p></li>
<li><p>Save.</p></li>
<li><p>Reload the page.</p></li>
<li><p>Open the mobile Contact book.</p></li>
</ol></td>
<td><p>The contact is in the list right after save, without a manual refresh, and all values are still there after reload. It also appears on mobile with the same values.</p></td>
<td><p>As expected</p></td>
<td rowspan="2"><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49782325255-chrome_tSiqjzDDzu.mp4">chrome_tSiqjzDDzu.mp4</a></span></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>10</td>
<td><p>New contact matched against ePost users</p></td>
<td><ol>
<li><p>Add a contact using the email or phone of an existing ePost user.</p></li>
<li><p>Save.</p></li>
<li><p>Check the On ePost column and hover the badge.</p></li>
<li><p>On mobile, open the contact and start a chat.</p></li>
</ol></td>
<td><p>The contact shows the On ePost badge, and hovering shows the matched-user detail, the same as People Hub. On mobile the contact is matched and a chat can be started.</p></td>
<td><p>As expected</p></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>11</td>
<td><p>Edit a web-created contact</p></td>
<td><ol>
<li><p>Open a contact created on web (USER_INPUT).</p></li>
<li><p>Edit its name and add a phone number.</p></li>
<li><p>Save.</p></li>
<li><p>Reload the page, then check on mobile.</p></li>
</ol></td>
<td><p>The changes show right away, persist after reload, and are visible on mobile.</p></td>
<td><p>As expected</p></td>
<td rowspan="2"><span class="confluence-embedded-file-wrapper image-center-wrapper confluence-embedded-manual-size"><a href="../_attachments/49782325255-chrome_NtkiY5OQZ0.mp4">chrome_NtkiY5OQZ0.mp4</a></span></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>12</td>
<td><p>Delete a web-created contact</p></td>
<td><ol>
<li><p>Open a contact created on web.</p></li>
<li><p>Delete it and confirm.</p></li>
<li><p>Reload the page, then check on mobile.</p></li>
</ol></td>
<td><p>The contact disappears from the list without a manual refresh, stays gone after reload, and is also gone on mobile.</p></td>
<td><p>As expected</p></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>13</td>
<td><p>Phone-synced contact cannot be edited or deleted</p></td>
<td><ol>
<li><p>On mobile, sync the phone contacts.</p></li>
<li><p>On web, open one of the phone-synced contacts (DEVICE_SYNC).</p></li>
<li><p>Look for Edit and Delete.</p></li>
</ol></td>
<td><p>Edit and Delete are not offered. An info bar explains that the contact cannot be edited or deleted because it is stored in the mobile phonebook. <em>(Latest PO direction from 23.09; see the note above.)</em></p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>14</td>
<td><p>Contact detail side panel</p></td>
<td><ol>
<li><p>Click a contact with several emails, phones and addresses and an ePost match.</p></li>
</ol></td>
<td><p>The side panel shows every value (not just the first of each) and the read-only On ePost status.</p></td>
<td><p>As expected</p></td>
<td>

![[49782325255-image-20260925-015042.png]]

</td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>15</td>
<td><p>Send letter and Chat hidden</p></td>
<td><ol>
<li><p>Open a contact's detail panel and row actions as a private user.</p></li>
</ol></td>
<td><p><strong>Send letter</strong> and <strong>Chat</strong> are hidden, not disabled. PO decision: they are mobile-only.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>16</td>
<td><p>Empty state</p></td>
<td><ol>
<li><p>Log in as a private user with no contacts (new account, no phone sync).</p></li>
</ol></td>
<td><p>The empty-state screen is shown with an <strong>Add contact</strong> action, not an empty table.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

</p></td>
</tr>
<tr>
<td>17</td>
<td><p>i18n DE / EN / FR / IT</p></td>
<td><ol>
<li><p>Switch the language to DE, EN, FR and IT in turn.</p></li>
<li><p>Check the nav label, column headers, add/edit form, empty state, no-results message and the synced-contact info bar.</p></li>
</ol></td>
<td><p>Everything is translated in each language, and the nav label reads Kontaktbuch / Address book (and FR/IT equivalents). No raw translation keys show.</p></td>
<td><p>As expected</p></td>
<td></td>
<td><p>

![[49782325255-check.png]]

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
