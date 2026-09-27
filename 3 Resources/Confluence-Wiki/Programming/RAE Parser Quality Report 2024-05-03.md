---
ai_hash: e0ebed9e2dcde46e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.73
entities: []
relevance: 0.762
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/47799566715/RAE+Parser+Quality+Report+2024-05-03
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: RAE Parser Quality Report 2024-05-03
topic: programming
type: source
updated: 2024-05-03
---

# RAE Parser Quality Report 2024-05-03

> [!info] Imported from Confluence
> Space **AI** · updated 2024-05-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47799566715/RAE+Parser+Quality+Report+2024-05-03)
> Relevance 0.762 · topic `programming`

# Training

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Dataset Seed</strong></p></th>
<td><p>1607</p></td>
</tr>
<tr>
<th><p><strong>Dataset Size</strong></p></th>
<td><p>500k</p></td>
</tr>
<tr>
<th><p><strong>Dataset Granularity</strong></p></th>
<td><p>LOW</p></td>
</tr>
<tr>
<th><p><strong>Features</strong></p></th>
<td><p>custom</p>
<ul>
<li><p>window{ 5, 5 } of token class</p></li>
<li><p>window{ 5, 5 } of token</p></li>
<li><p>definition</p></li>
<li><p>previous map</p></li>
<li><p>trigram name</p></li>
<li><p>character n-gram [ 3, 5 ]</p></li>
<li><p>sentence{ begin, !end }</p></li>
</ul></td>
</tr>
<tr>
<th><p><strong>Cut-Off</strong></p></th>
<td><p>3</p></td>
</tr>
<tr>
<th><p><strong>Iterations</strong></p></th>
<td><p>100</p></td>
</tr>
<tr>
<th><p><strong>Tolerance</strong></p></th>
<td><p>0.00005</p></td>
</tr>
<tr>
<th><p><strong>Algorithm</strong></p></th>
<td><p>Perceptron</p></td>
</tr>
</tbody>
</table>

</div>

M1 = 25min

Stats: (12320463/12325336) 0.9996046355247435

# Evaluation

<div>

|                         |     |
|-------------------------|-----|
| **Dataset Seed**        | 93  |
| **Dataset Size**        | 1k  |
| **Dataset Granularity** | LOW |

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="553d23a0-e23c-4536-ad9e-7e353ca2019b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
   TOTAL: precision:   97.16%;  recall:   89.37%; F1:   93.10%.
     BOX: precision:   99.70%;  recall:   99.10%; F1:   99.40%. [target: 334; tp: 331; fp:   1]
     ADR: precision:   97.17%;  recall:   96.02%; F1:   96.59%. [target: 679; tp: 652; fp:  19]
     ACT: precision:   97.60%;  recall:   92.13%; F1:   94.79%. [target: 1322; tp: 1218; fp:  30]
     POC: precision:  100.00%;  recall:   89.81%; F1:   94.63%. [target: 844; tp: 758; fp:   0]
     LOC: precision:   94.40%;  recall:   81.87%; F1:   87.69%. [target: 844; tp: 691; fp:  41]
     AOT: precision:   80.00%;  recall:   56.41%; F1:   66.17%. [target:  78; tp:  44; fp:  11]
     COT: precision:   81.58%;  recall:   46.27%; F1:   59.05%. [target:  67; tp:  31; fp:   7]
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Global model evaluation]]
- [[RAE Parser Quality Report 2024-06-06]]
- [[RAE Parser Quality Report 2024-07-08]]
- [[Trigram Search — Performance-Env Benchmark]]
- [[Trigram Index — Size-Reduction Options]]

%% ai-graph-end %%