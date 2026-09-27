---
ai_hash: 1d68494dbc69d880
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.53
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47746679688/Performance+pain+points
space: FUT
status: reference
tags:
- confluence
- programming
- space/fut
title: Performance pain points
topic: programming
type: source
updated: 2024-04-09
---

# Performance pain points

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-04-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47746679688/Performance+pain+points)
> Relevance 0.721 · topic `programming`

<span class="confluence-embedded-file-wrapper image-center-wrapper"><img src="https://axonivy.atlassian.net/wiki/download/attachments/47741468790/module%20overview.png?version=19&amp;modificationDate=1712730826119&amp;cacheVersion=1&amp;api=v2" class="confluence-embedded-image image-center" loading="lazy" data-image-src="https://axonivy.atlassian.net/wiki/download/attachments/47741468790/module%20overview.png?version=19&amp;modificationDate=1712730826119&amp;cacheVersion=1&amp;api=v2" data-base-url="https://axonivy.atlassian.net/wiki" /></span>

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
<th><p><strong>Functionality</strong></p></th>
<th></th>
<th><p><strong>Performance problem possibility</strong></p></th>
<th><p><strong>Solution</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Upload documents/metadata</p></td>
<td></td>
<td><p>

![[47746679688-error.png]]

 - The upload functionality implemented using <strong>async</strong> mechanism of Typescript/Javascript and running Using Node.js. Uploading is I/O operation and this operation is non-blocking. It means that it can support thousand of concurrent requests.</p>
<p>any performance issue for the process of handling upload documents? - <strong>todo</strong></p></td>
<td></td>
<td><p>On dev: there only one pod for <code>luz-epc-api </code>and 3 pod for <code>luz-epc-api-no-storage</code> -&gt; Any problem with only one pod for <code>luz-epc-api</code> (consider in scalability)<br />

![[47746679688-image-20240403-074516.png]]

</td>
</tr>
<tr>
<td><p>Publishing messages from API to topics</p></td>
<td></td>
<td><p>

![[47746679688-error.png]]

 - Async <strong></strong></p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Receiving rest API call and publishing messages</p></td>
<td></td>
<td><p>

![[47746679688-error.png]]

 - Async</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>delivery service (luz-epc-services)</p></td>
<td></td>
<td><p>☑️ - This service does several tasks in sequences related to file (metadata). Then, with this approach performance issue can happen.</p>

![[47746679688-image-20240404-091606.png]]

</td>
<td><p>discuss with Yan to see if any step can be done concurrently/parallel.</p></td>
<td><p>idea: concerns that this service has several sub service/tasks run in sequence. Then, may it causing performance pain point?</p></td>
</tr>
<tr>
<td><p>calculate-cost service</p></td>
<td></td>
<td rowspan="17"><ul>
<li><p>

![[47746679688-error.png]]

 Download many time in one service</p></li>
<li><p>☑️ Download again in other services</p></li>
<li><p>

![[47746679688-error.png]]

 N+1: Download parts in different requests</p></li>
<li><p>❔ Datastore: Persistent Disk</p></li>
</ul></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>convert metadata service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>copy sftp service</p></td>
<td></td>
<td></td>
<td><p>Step 1. Download pdf (throgh API)</p>
<p>Step 2. Upload to sftp</p></td>
</tr>
<tr>
<td><p>xml2json service</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Dowload through API</p></li>
<li><p>Convert to json</p></li>
<li><p>Call API to update message with new json</p></li>
</ul></td>
</tr>
<tr>
<td><p>digital delivery service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>extract text service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>extract xmp service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>preprocess service</p></td>
<td></td>
<td></td>
<td><p>(Not used)</p></td>
</tr>
<tr>
<td><p>preview service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>scan qr service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>send ebill service</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Download pdf and send</p></li>
</ul></td>
</tr>
<tr>
<td><p>send postmark service</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Download pdf and metadata → send</p></li>
</ul></td>
</tr>
<tr>
<td><p>validate pdf service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>embeded font service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>hash message service</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Download attachment</p></li>
</ul></td>
</tr>
<tr>
<td><p>spoke service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>supplement pages service</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>merge metadata service</p></td>
<td><p>N: Not manipulate files</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Call API of luz-epc-api to merge (No download)</p></li>
</ul></td>
</tr>
<tr>
<td><p>print service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Get number of page and sheet from metadata and update message to luz-epc-api</p></li>
</ul></td>
</tr>
<tr>
<td><p>validate metadata service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Extract info (e.g. pages, sheets, etc.) from message and validate</p></li>
</ul></td>
</tr>
<tr>
<td><p>wait attachment service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Call API in luz-epc-api to search for attachment id. No download file</p></li>
</ul></td>
</tr>
<tr>
<td><p>has attachment service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td><ul>
<li><p>Check info in the message/event</p></li>
</ul></td>
</tr>
<tr>
<td><p>notify message status changed service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>queue processor service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>validate ebill recipient service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>wait service</p></td>
<td><p>N</p></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

# Concerns

- 4 Instances of luz-epc enough?

- Lib performance E.g. xml to json?

- InputStream:

  - For Java services: The lib of `httpclient5-fluent` currently reads the whole content of a responding file into memory and return as an ByteArrayInputStream 

![[47746679688-image-20240408-043545.png]]

 

![[47746679688-image-20240408-043038.png]]

 . Then, it might cause the memory issue

    - Suggestion: Use lib that support download file as FileInputStream (e.g. Jersey Client)

  - <span class="inline-comment-marker" ref="3b576a1c-6e60-4aa2-a4dd-340b98cd7f27">Downloading file in Javascript services</span>: Download the whole content 

![[47746679688-image-20240408-045208.png]]

 → It might cause the memory issues

- Configuration for stream optimal?

  - Not able to configure stream buffer 

![[47746679688-image-20240408-065453.png]]

 

![[47746679688-image-20240408-065526.png]]

 

![[47746679688-image-20240408-065626.png]]



- Ask Yan: if some steps have performance pain point?

%% ai-graph-start %%

**Related notes:**
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[Measure API luz-docs]]
- [[Public API client performance analysis]]
- [[EPC API - Load Test]]
- [[New architecture for documentStatistic]]

%% ai-graph-end %%