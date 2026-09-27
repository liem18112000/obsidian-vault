---
ai_hash: 75f9983087870d29
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.786
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530450210/Perfomance+of+ePost+Mylife+Branded+folder+API
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Perfomance of ePost/Mylife Branded folder API
topic: programming
type: source
updated: 2021-06-24
---

# Perfomance of ePost/Mylife Branded folder API

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20530450210/Perfomance+of+ePost+Mylife+Branded+folder+API)
> Relevance 0.786 · topic `programming`

In klara there no any physical branded folder for the ePost/myLife mobile app It is calculated based on the branded letters.

for the calculations for the branded folder flow is as below


![[20530450210-brandedFolder_seq.png]]



  

### Use case 01

Time consuming for each API call  if user has more than 2k branded letters.

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>steps</th>
<th>time</th>
<th>log from GCP</th>
<th>note</th>
</tr>
&#10;<tr>
<td>1</td>
<td>3879 ms</td>
<td>luz-uri=GET /luz_mylife_epost_adapter/api/27d1afac-02eb-4f1f-9912-fe798df22383/archives/directories/branded HTTP/1.1 status-code=200 bytes-sent=569944 time-consuming=3879</td>
<td><p>totol of step1 to step 5</p>
<p>(this is whole branded response time)</p></td>
</tr>
<tr>
<td>2</td>
<td>443 ms</td>
<td>/luzsec/api/refreshtokens/tenants/27d1afac-02eb-4f1f-9912-fe798df22383 HTTP/1.1 status-code=201 bytes-sent=4016 time-consuming=433</td>
<td><br />
</td>
</tr>
<tr>
<td>3</td>
<td>3400 ms </td>
<td>luz-uri=GET /luz_docs_view_controller/api/27d1afac-02eb-4f1f-9912-fe798df22383/archives/directories/branded HTTP/1.1 status-code=200 bytes-sent=569944 time-consuming=3400</td>
<td>total time step4 + step5</td>
</tr>
<tr>
<td>4</td>
<td>3055 ms</td>
<td>/luz_docs/api/27d1afac-02eb-4f1f-9912-fe798df22383/documents HTTP/1.1 status-code=200 bytes-sent=5805918 time-consuming=3055</td>
<td><br />
</td>
</tr>
<tr>
<td>5</td>
<td>437 ms  * total no of sender tenant from step 3</td>
<td>luz-uri=GET /luztenant/api/sender-configurations/search?tenant-id=247ce978-a68d-4005-843c-4046c8e21c18&amp;company-id=1 HTTP/1.1 status-code=200 bytes-sent=78962 time-consuming=437</td>
<td>this call not happen if sender configuration already in the luz_docs_view_controller memory cache.</td>
</tr>
</tbody>
</table>

</div>

Question: How long do I have to wait on the apps (iOS / Android), so that branded folders view is loaded?

### Use case 02

Time consuming for each API call, if a user has 2 branded folders with 7 branded letters in branded folder A and 5 letters in branded folder B

  

Questions;

- How long do I have to wait on the apps (iOS / Android), so that branded folders view is loaded?

<div>

|  |  |  |
|----|----|----|
| Android | API call & display within 1 minute | API call & display more than 1 minute |
| attempt 1 | 2557ms | 3800ms |
| attempt 2 | 2327ms | 3838ms |
| attempt 3 | 2339ms | 4787ms |

</div>

- Where are the bottlenecks?

     

### Use case 03

Open letterbox with a total of 10 letters (7 from branded sender A and 5 from branded sender B and 3 unbranded)

Questions;

- How long do I have to wait on the apps (iOS / Android), so that branded folders view is loaded?

<div>

|  |  |  |
|----|----|----|
| Android | API call & display within 1 minute | API call & display more than 1 minute |
| attempt 1 | 2459ms | 3964ms |
| attempt 2 | 2298ms | 3776ms |
| attempt 3 | 2207ms | 3767ms |

</div>

- Where are the bottlenecks?

%% ai-graph-start %%

**Related notes:**
- [[Public API client performance analysis]]
- [[Optimus ePost myLife app(luz_mylife_epost_adapter) - API Response Performance Analysis]]
- [[eArchive request flow and log correlation (perf)]]
- [[eArchive performance — luz-epost-business-web calls the count API on every search]]
- [[Measure API luz-docs]]

%% ai-graph-end %%