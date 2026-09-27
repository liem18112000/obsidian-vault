---
ai_hash: c72436bbb4b6ccf2
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 8
depth: 2.67
entities: []
relevance: 0.81
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47889547477/Research+The+concept+to+update+the+status+of+delivery+instantly+after+all+documents+are+processed
space: HACKA
status: reference
tags:
- confluence
- architecture
- space/hacka
title: 'Research: The concept to update the status of delivery instantly after all
  documents are processed'
topic: architecture
type: source
updated: 2025-06-23
---

# Research: The concept to update the status of delivery instantly after all documents are processed

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-06-23 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47889547477/Research+The+concept+to+update+the+status+of+delivery+instantly+after+all+documents+are+processed)
> Relevance 0.81 · topic `architecture`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47889547477_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-120377" macro-id="734adbd6-207d-4ca9-b64e-2712a041b632" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-120377" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-120377</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

### **Issue**

Currently, when creating an asynchronous OneAPI delivery, it takes between 60 and 120 seconds after the processing of all the documents is finished to update the status of the delivery itself. This is way too long.  
Therefore we need to come up with a concept to instantly update the status of the delivery after all documents of the delivery were processed (i.e. \< 1 second).


![[47889547477-image-20240619-031615.png]]



The delivery status was updated to DELIVERED at 14:41:20. It took around 2 minutes and 15 seconds.

It took two pullings to finish the delivery status update, as you can see in the picture. At the first pulling time, the document was not delivered yet, so it kept waiting. It finished at the second polling.


![[47889547477-image-20240619-031742.png]]



### Overview


![[47889547477-Overview delivery status queue.png]]



[One API Flow diagram](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47721021871/One+API+Flow+diagram)


![[47889547477-image-20240619-033014.png]]




![[47889547477-image-20240619-033149.png]]



Document about the testing performance of PUB/SUB: [Test and release pub/sub](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47388165426/Test+and+release+pub+sub)

### Solution

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>How can we do it? (Steps)</strong></p></th>
<th><p><strong>Pros</strong></p></th>
<th><p><strong>Cons</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Reduce retry delay of Delivery Queue</p></td>
<td><p>Handle message faster</p></td>
<td><p>Not sure about the performance</p></td>
<td></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>Update database directly (We don’t use Pub/Sub)<br />
We need cron job instead of Delivery queue</p>
<ol>
<li><p>The last document sets the delivery status to delivered/delivered_with_error/error/...</p></li>
</ol></td>
<td><p>Faster</p></td>
<td><p>Impact the performance of DB</p>
<p>Delivery queue was remove. We use cron job instead of pub/sub.</p></td>
<td></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>Update database directly. We base on the last document.</p>
<ol>
<li><p>The last document sets the delivery status to delivered/delivered_with_error/error/...</p></li>
<li><p>The job checks if all documents are in a final status after 6 hours -&gt; and if not the job sets it to error/delivered_with_error</p></li>
</ol></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>Re-Design Delivery, Document and Recipient</p></td>
<td></td>
<td><p>Big effort</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

### Final solution

After discussing, we agreed to build the message when we know the status of the last document that was updated.


![[47889547477-Duplicate message delivery queue.png]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[5. How to extend modify ONE API delivery API Research]]
- [[Copy 5. How to extend modify ONE API delivery API Research]]
- [[Regular Load Test Performance Test of OneAPI]]
- [[ePost API (28.02.2023 - 13.03.2023)]]

%% ai-graph-end %%