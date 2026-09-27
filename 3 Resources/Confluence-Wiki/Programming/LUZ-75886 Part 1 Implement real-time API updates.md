---
ai_hash: aef01164e3e97f4e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.57
entities: []
relevance: 0.746
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47201222918/LUZ-75886+Part+1+Implement+real-time+API+updates
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: LUZ-75886 [Part 1] Implement real-time API updates
topic: programming
type: source
updated: 2022-10-26
---

# LUZ-75886 [Part 1] Implement real-time API updates

> [!info] Imported from Confluence
> Space **TS** · updated 2022-10-26 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47201222918/LUZ-75886+Part+1+Implement+real-time+API+updates)
> Relevance 0.746 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47201222918_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-75886" macro-id="1c9cf45e-8977-4e25-b00a-21649fc6ad2a" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-75886" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-75886</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

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
<th colspan="4"><p><strong>Test case</strong></p></th>
<th><p><strong>Expectation</strong></p></th>
<th><p><strong>Observe behaviour</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td rowspan="10"><p>Google side</p></td>
<td rowspan="8"><p>Create booking</p></td>
<td rowspan="4"><p>will “All staff” (All resource)</p></td>
<td><p>simple service</p></td>
<td rowspan="8"><ol>
<li><p>Appointment appears in Booking Calendar</p></li>
<li><p>Receive notification email from Google</p></li>
<li><p>Receive notification from KLARA to end customer</p>
<ol>
<li><p>Email</p></li>
<li><p>Mobile</p></li>
<li><p>Both</p></li>
</ol></li>
<li><p>Receive notification (email) from KLARA to shop owner</p></li>
<li><p>Customer’s information is updated in CRM</p></li>
<li><p>Booking info in SBC must be “Created by Customer from Google“</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>2</td>
<td><p>simple service has step</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>3</td>
<td><p>variant</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>4</td>
<td><p>variant has step</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>5</td>
<td rowspan="4"><p>with a specific staff (resource)</p></td>
<td><p>simple service</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>6</td>
<td><p>simple service has step</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>7</td>
<td><p>variant</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>8</td>
<td><p>variant has step</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>9</td>
<td><p>Update booking</p></td>
<td colspan="2" rowspan="2"></td>
<td><ol>
<li><p>Receive email from Google</p></li>
<li><p>Duration will be updated in RwG</p></li>
<li><p>Duration will be updated in Klara</p></li>
<li><p>Availability will be updated</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-error.png]]

 → <span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47201222918_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-88015" data-macro-id="c78f9442-7ee3-4ff5-b5ce-cfa887adba8f" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-88015" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-88015</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>10</td>
<td><p>Cancel booking</p></td>
<td><ol>
<li><p>Receive email from Google</p></li>
<li><p>Appointment will be disappeared in Klara</p></li>
<li><p>Availability will be updated</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>11</td>
<td rowspan="7"><p>Klara side</p></td>
<td rowspan="4"><p>Create booking</p></td>
<td colspan="2"><p>simple service</p></td>
<td rowspan="4"><ol>
<li><p>Availability will be updated on Google</p></li>
<li><p>Google will prompt “Sorry, this time is no longer available“ if we click on time slot is not available anymore</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>12</td>
<td colspan="2"><p>simple service has step</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>13</td>
<td colspan="2"><p>variant</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>14</td>
<td colspan="2"><p>variant has step</p></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>15</td>
<td><p>Update booking</p></td>
<td colspan="2" rowspan="3"></td>
<td><ol>
<li><p>Receive email from Google</p></li>
<li><p>Duration will be updated in Klara</p></li>
<li><p>Duration will be updated in RwG</p></li>
<li><p>Availability will be updated</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>16</td>
<td><p>Mark as no-show</p></td>
<td><ol>
<li><p>Availability will be updated</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>17</td>
<td><p>Cancel booking which is booked from RwG</p></td>
<td><ol>
<li><p>Receive email from Google</p></li>
<li><p>Appointment will be disappeared in Klara</p></li>
<li><p>Availability will be updated</p></li>
</ol></td>
<td><ol>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
<li><p>

![[47201222918-check.png]]

</p></li>
</ol></td>
</tr>
<tr>
<td>18</td>
<td></td>
<td><p>The services have durations equals 5minute</p></td>
<td></td>
<td></td>
<td></td>
<td>

![[47201222918-image-20221021-081226.png]]

![[47201222918-image-20221021-081237.png]]

![[47201222918-image-20221021-081355.png]]

</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-109076 Public API - Create update new tenant (implementation)]]
- [[LUZ-79226 PayPal in Online Shop - Adaption KLARA Pay - Gateway API]]
- [[Test Keycloak - Public API]]
- [[LUZ-59727 - CTI Interface implementation]]
- [[LUZ-81908 - Implement test mode without send to the Rhine server]]

%% ai-graph-end %%