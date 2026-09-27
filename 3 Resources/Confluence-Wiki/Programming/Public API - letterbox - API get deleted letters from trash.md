---
ai_hash: 23cee743ecec3972
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 18
depth: 3
entities: []
relevance: 0.842
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47728951400/Public+API+-+letterbox+-+API+get+deleted+letters+from+trash
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Public API - letterbox - API get deleted letters from trash
topic: programming
type: source
updated: 2024-03-20
---

# Public API - letterbox - API get deleted letters from trash

> [!info] Imported from Confluence
> Space **TS** · updated 2024-03-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47728951400/Public+API+-+letterbox+-+API+get+deleted+letters+from+trash)
> Relevance 0.842 · topic `programming`

**Related US:** <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47728951400_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-115504" macro-id="48d7c4ed-dd60-4feb-9b28-5ef5fd90f28e" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-115504" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-115504</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

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
<th><p><strong>Description</strong></p></th>
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Expectation</strong></p></th>
<th><p><strong>Status</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Valid Request</p></td>
<td><ol>
<li><p>Send a GET request to the API endpoint for retrieving deleted letters.</p></li>
<li><p>Verify the response status code (should be 200 OK).</p></li>
<li><p>Check if the response contains a list of deleted letters.</p></li>
</ol></td>
<td><p>The API returns a valid list of deleted letters.</p></td>
<td><p>

![[47728951400-check.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Invalid Authentication</p></td>
<td><ol>
<li><p>Send a GET request with invalid authentication credentials (e.g., missing access token or incorrect API key).</p></li>
<li><p>Verify the response status code (should be 401 Unauthorized).</p></li>
</ol></td>
<td><p>The API consistently returns a “401 Unauthorized” status code.</p></td>
<td><p>

![[47728951400-check.png]]

</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Missing Parameters</p></td>
<td><ol>
<li><p>Send a GET request without specifying offset or limit parameters</p></li>
<li><p>Verify the response status code (should be 200 OK ).</p></li>
</ol></td>
<td><p>the API handles missing offset and limit by using default value and return 200 OK</p></td>
<td><p>

![[47728951400-check.png]]

</p></td>
</tr>
<tr>
<td>4</td>
<td><p>Pagination</p></td>
<td><ol>
<li><p>Send a GET request with pagination parameters (e.g., offset, limit )</p></li>
<li><p>Verify the response status code (should be 200 OK).</p></li>
<li><p>Check if the response contains the expected subset of letters.</p></li>
</ol></td>
<td><p>The API supports pagination and returns the correct subset of data.</p></td>
<td><p>

![[47728951400-check.png]]

</p></td>
</tr>
<tr>
<td>5</td>
<td><p>Filtering by ParticipantId</p></td>
<td><ol>
<li><p>Send a GET request with a specific participantId(e.g., <code>29eec563-04f8-4121-8d4e-77f050314172</code>)</p></li>
<li><p>Verify the response status code (should be 200 OK).</p></li>
<li><p>Check if the response contains only deleted letters associated with that user.</p></li>
</ol></td>
<td><p>The API correctly filters letters based on the specified participantId.</p></td>
<td><p>

![[47728951400-check.png]]

</p></td>
</tr>
<tr>
<td>6</td>
<td><p>Malformed Data</p></td>
<td><ol>
<li><p>Send a GET request with malformed data (e.g., letter-types=classic_letter).</p></li>
<li><p>Verify the response status code (should be 400 Bad Request).</p></li>
</ol></td>
<td><p>The API properly rejects and responds to malformed input.</p>
<p>Return 404</p></td>
<td><p>

![[47728951400-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Public API - letterbox - change letter status to read unread]]
- [[LUZ-115505 Public API - letterbox Part 3]]
- [[Empty Trash APIs]]
- [[LUZ-146746 Use correct API's for delete, restore and their undo]]
- [[Error handling for delete and undo]]

%% ai-graph-end %%