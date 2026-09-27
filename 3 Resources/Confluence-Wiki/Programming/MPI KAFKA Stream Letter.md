---
title: "MPI KAFKA Stream Letter"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/47923527851/MPI+KAFKA+Stream+Letter
space: "AI"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2024-10-09
attachments: 1
tags:
  - confluence
  - programming
  - space/ai
---

# MPI KAFKA Stream Letter

> [!info] Imported from Confluence
> Space **AI** · updated 2024-10-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47923527851/MPI+KAFKA+Stream+Letter)
> Relevance 0.738 · topic `programming`

According to Frangi Aurelio, IT11.2 \<<a href="mailto:aurelio.frangi@post.ch" class="external-link" rel="nofollow">aurelio.frangi@post.ch</a>\>, Swiss Post could provide us with a tailored KAFKA topic, having a single message per forwarded ePost letter.

REST API seems to be the Confluent REST Proxy API

<a href="https://docs.confluent.io/platform/current/kafka-rest/api.html" class="external-link" data-card-appearance="block" rel="nofollow">https://docs.confluent.io/platform/current/kafka-rest/api.html</a>

![[47923527851-SwissPost-KafkaStreams.png]]



# Required Information

## NSA Registration ID

For each ePost Customer, we create an NSA (Nachsende Auftrag → Forward Delivery Order). Each NSA has a unique identifier.

<span style="background-color: rgb(211,241,167);">If we would know the NSA registration key, as part of the received letter message, we would be able to make a 100% match inside the tenant directory.</span>

<div hasbody="true" macro-id="baca1976-cd41-472f-8afd-97b971d41170" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

Be aware that one Forward Delivery Order nevertheless can belong to multiple addresses and even to different tenants.

</div>

</div>

# Service Levels

## Servicezeit

<div hasbody="true" macro-id="2abf4f83-9220-4fbd-99b4-b3bafd6764e1" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

In welchem Zeitraum muss die Leistung mindestens verfügbar sein?

</div>

</div>

<div>

<table>
<tbody>
<tr>
<th><p><strong>Level 1</strong></p></th>
<td><p>Mo - Fr</p></td>
<td><p>07:30 - 17:00</p></td>
<td><p>9.5 h</p></td>
</tr>
<tr>
<th rowspan="2"><p><strong>Level 2</strong></p></th>
<td><p>Mo - Fr</p></td>
<td><p>07:00 - 19:00</p></td>
<td><p>12 h</p></td>
</tr>
<tr>
<td><p>Sa</p></td>
<td><p>07:30 - 13:00</p></td>
<td><p>5.5 h</p></td>
</tr>
<tr>
<th><p><strong>Level 3</strong></p></th>
<td><p>Mo - Fr</p></td>
<td><p>06:00 - 23:00</p></td>
<td><p>17 h</p></td>
</tr>
<tr>
<th rowspan="2"><p><strong>Level 4</strong></p></th>
<td><p>Mo - Fr</p></td>
<td><p>06:00 - 23:00</p></td>
<td><p>17 h</p></td>
</tr>
<tr>
<td><p>Sa</p></td>
<td><p>07:00 - 18:00</p></td>
<td><p>11 h</p></td>
</tr>
<tr>
<th><p><strong>Level 5</strong></p></th>
<td><p>Mo - So</p></td>
<td><p>00:00 - 24:00</p></td>
<td><p>24 h</p></td>
</tr>
</tbody>
</table>

</div>

## Ausfallzeit

<div hasbody="true" macro-id="8552657f-bb66-45cf-a7a5-54aa794c869b" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Was ist die maximal tolerierbare Ausfallzeit innerhalb der Servicezeit?

</div>

</div>

<div>

|              |                          |                               |
|--------------|--------------------------|-------------------------------|
| **Klasse D** | Standard                 | mehr als 3 Tage (best Effort) |
| **Klasse C** | Zeitkritisch             | bis 3 Tage                    |
| **Klasse B** | Erhöhte Zeitkritikalität | bis 12 Stunden                |
| **Klasse A** | Höchste Zeitkritikalität | bis 4 Stunden                 |

</div>

## Datenverlustzeit

<div hasbody="true" macro-id="1c1c4fb2-18a4-4861-98c0-ac913ff63207" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

Was ist die maximal tolerierbare Zeit innerhalb derer die Daten wiederhergestellt werden müssen?

</div>

</div>

<div>

|              |          |                                  |
|--------------|----------|----------------------------------|
| **Klasse D** | Standard | mehr als 8 Stunden (best Effort) |
| **Klasse C** |          |                                  |
| **Klasse B** |          |                                  |
| **Klasse A** |          |                                  |

</div>
