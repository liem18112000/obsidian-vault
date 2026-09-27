---
ai_hash: 563d41c497e316ca
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 2.32
entities: []
relevance: 0.76
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47212695342/Architecture+design+for+micro+moments
space: HACKA
status: reference
tags:
- confluence
- architecture
- space/hacka
title: Architecture design for micro moments
topic: architecture
type: source
updated: 2022-11-14
---

# Architecture design for micro moments

> [!info] Imported from Confluence
> Space **HACKA** · updated 2022-11-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47212695342/Architecture+design+for+micro+moments)
> Relevance 0.76 · topic `architecture`

### Related user story: <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47212695342_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-88281" macro-id="07afc1ad-74a8-4d37-9cf9-410164407beb" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-88281" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-88281</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

### \*Please note that this system design is yet to be finalized version

## 1/ What is micro moments:

This is a marketing term.

- From customer perspective: when people reflexively turn to a device to act on a need to learn something, do something, discover something, watch something, or buy something.

- From developers perspective: micro moments are events, that will create IVY task, when the user click on the tasks, they will get consulting/an offer form esurance (3rd party)

## 2/ Problem statement:

- How to design a micro moment architecture that is simple and extensible.

- Luz-insurance will handle business logic and determine which event is a micro moment.

- These micro moments will appear on the new insurance dash board and on Klara tasks.

- The user click on task will get:

  - 1\. Consulting from esurance (3rd party)

  - 2\. Get offer via esurance (3rd party)  

## 3/ Use cases example:

- For new employee added:

  - From luz-webclient or public-api, an API will be called to luz-compensation, after adding successfully, luz-compensation will create an event.

- For employee retirement about to reached

  - A scheduler will happen ,when the scheduler scan through luz-compensation and find out that employee A will reach retirement age in 5 year, an event will be created.

- These two events will be published onto the **event queue**, then those events will be consumed by luz-insurance, and an ivy task will be created.

## 4/ Solution design:


![[47212695342-Micro moments idea architect.png]]



## 5/ MVP version:

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Micro moments for MVP version</strong></p></th>
<th><p><strong>Architecture type</strong></p></th>
<th><p><strong>Possible relevant module(could be wrong)</strong></p></th>
</tr>
&#10;<tr>
<td><ul>
<li><p>1/ Insurances added (“Onboarding”)</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>Unknown</p></td>
</tr>
<tr>
<td><ul>
<li><p>2/ Address of company changed</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>3/ New branch added</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>4/ Legal form of company changed</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>5/ Employee added (First employee)</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>6/ Employee added (Second or additional employee)</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>7/ Employee left the company</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>8/ Salary changed significantly (&gt;25%) / Threshold for BVG/UVGO-Maximum reached</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>9/ Employee reached retirement age -5 years</p></li>
</ul></td>
<td><p>Scheduler</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>10/ Obligatory insurance was not added</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>11/ Insurance is about to expire</p></li>
</ul></td>
<td><p>Scheduler</p></td>
<td><p>luz-compensation</p></td>
</tr>
<tr>
<td><ul>
<li><p>12/ Company was founded recently (Founding date max XX months/years in the past)</p></li>
</ul></td>
<td><p>Event driven</p></td>
<td><p>luz-store</p></td>
</tr>
</tbody>
</table>

</div>

- Case 12: The required condition for this micro moment:

1/ The company is subscribed to luz-insurance

2/ Its registration date compare to founding date is less than a (to be determined) amount of time

## 6/ Implementation suggestion:

- luz-insurance will decide which event should be a micro moment.

- For event queue, we currently have google pub/sub as an implementation as some of its benefits are;

  - It is equally effective as messaging-oriented middleware for service integration or as a queue

  - It supports retry again if request fail or timeout

  - It can be judged on its performance in three aspects: scalability, availability, and latency.

References: [Message queue with Google Pub/Sub](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47000092971/Message+queue+with+Google+Pub+Sub)

<a href="https://cloud.google.com/pubsub/docs/overview" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/pubsub/docs/overview</a>

- For scheduler, our current architecture is Kubernetes cron job, It is suggested in the meeting that we will also apply this architect for micro moments.

%% ai-graph-start %%

**Related notes:**
- [[Architecture Design]]
- [[Architecture]]
- [[Copy 4. Architecture for delivering eLetter after email verified]]
- [[LUZ-75886 Part 1 Implement real-time API updates]]
- [[Business concept for frontend]]

%% ai-graph-end %%