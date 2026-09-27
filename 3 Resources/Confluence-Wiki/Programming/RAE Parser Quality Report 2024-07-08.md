---
ai_hash: 3a3c549d3dc08d39
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 49
depth: 2.53
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/47920742401/RAE+Parser+Quality+Report+2024-07-08
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: RAE Parser Quality Report 2024-07-08
topic: programming
type: source
updated: 2024-07-08
---

# RAE Parser Quality Report 2024-07-08

> [!info] Imported from Confluence
> Space **AI** · updated 2024-07-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/47920742401/RAE+Parser+Quality+Report+2024-07-08)
> Relevance 0.721 · topic `programming`

<div class="plugin-tabmeta-details conf-macro output-block" hasbody="true" macro-id="f3b432447941e841a387160edeb28b755b30c68e4bec9c91680d3fae347a8075" macro-name="details">

<div>

|                |                           |
|----------------|---------------------------|
| **Collection** | phase-03-cleaned/SNAPSHOT |
| **Size**       |                           |
| **Recall**     |                           |
| **Precision**  |                           |
| **Accuracy**   |                           |
| **f05**        |                           |

</div>

</div>

# Table of Contents

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="be8b4852-0b2e-4f28-b4df-4f4b2c297d66" macro-name="toc" numberedoutline="false" structure="list">

</div>

# Evaluation

Evaluation is of the “Address block only” parser -

# State

Tools at (500k, seed 1607)

App.Eval at `21b4892 ?`

Nlp at `c74b76a` ?

# Performance Overview

Evaluation of blockTokens (new annotatedBlockTokensRemaingExpected)

<div>

|        |               |        |        |               |           |            |
|--------|---------------|--------|--------|---------------|-----------|------------|
| **tp** | **<u>tn</u>** | **fp** | **fn** | **evaluated** | **error** | **missed** |
| 908    | 0             | 57     | 22     | 987           | 0         | 0          |

</div>

## 22 False Negative

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><code>CN</code></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Block text</strong></p></th>
</tr>
&#10;<tr>
<td><p><code>240201153000121</code></p>
<p><code>240201153000131</code></p>
<p><code>240201153000184</code></p>
<p><code>240201153000217</code></p>
<p><code>240202153000077</code></p>
<p><code>240202153000083</code></p>
<p><code>240206153000176</code></p>
<p><code>240206153000246</code></p>
<p><code>240212153000139</code></p>
<p><code>240212153000142</code></p>
<p><code>240214153000062</code></p>
<p><code>240214153000183</code></p></td>
<td><p>The house number has an additional letter</p>
<p>7 B</p>
<p>or</p>
<p>7B etc</p>
<p><code>240201153000184</code> (correct truth vs <code>240202153000077</code>?) also has the names of two persons in one line</p></td>
<td><p>Herr</p>
<p>Suwethan Vaitheeswaran</p>
<p>Gishalde 7 B</p>
<p>4663 Aarburg</p>
<p>Madame Monsieur</p>
<p>Charlotte Longson Neil Longson</p>
<p>Av. Eugène-Rambert 14B</p>
<p>1815 Clarens</p>

![[47920742401-Screenshot 2024-06-07 at 15.53.07.png]]

</td>
</tr>
<tr>
<td><p><code>240201153000132</code></p></td>
<td><p>The street name has no house number</p></td>
<td><p>Buenaventuras AG</p>
<p>Wishalde</p>
<p>6340 Baar</p></td>
</tr>
<tr>
<td><p><code>240201153000229</code></p></td>
<td><p>Does not look unusual</p></td>
<td><p>Herr</p>
<p>Renato Regli</p>
<p>Spitalstrasse 29</p>
<p>6004 Luzern</p></td>
</tr>
<tr>
<td><p><code>240202153000261</code></p></td>
<td></td>
<td><p>Herr Carlo Nessi</p>
<p>Seestrasse 272</p>
<p>8038 Zürich</p></td>
</tr>
<tr>
<td><p><code>240202153000292</code></p></td>
<td></td>
<td><p>Amden Wesen Tourismus</p>
<p>Dorfstrasse 22</p>
<p>8873 Amden</p></td>
</tr>
<tr>
<td><p><code>240206153000058</code></p></td>
<td><p>no dash between country code and postale-code</p></td>
<td><p>Herr</p>
<p>Ryan Brown</p>
<p>Langhaus 5</p>
<p>CH 5400 Baden</p></td>
</tr>
<tr>
<td><p><code>240206153000195</code></p></td>
<td><p>A long locality name?</p></td>
<td><p>Ugo Cavallo</p>
<p>Curtins 79</p>
<p>7522 La Punt-Chamues-ch</p></td>
</tr>
<tr>
<td><p><code>240206153000211</code></p></td>
<td><p>Capital letters organisation name</p></td>
<td><p>GELPELL AG</p>
<p>Kirchbergerstrasse 10</p>
<p>9534 Gähwil</p></td>
</tr>
<tr>
<td><p><code>240212153000081</code></p></td>
<td><p>Organisation name and person name in same line</p></td>
<td><p>Adventice Pharma, D. Tenthorey</p>
<p>Boulevard Saint-Martin 19</p>
<p>1800 Vevey</p></td>
</tr>
<tr>
<td><p><code>240212153000146</code></p></td>
<td><p>Locality name in the recipient line</p>
<p>and also a title</p></td>
<td><p>MLaw Marad Widmer, LL.M. (Genf)</p>
<p>Widmer Strategy Gmbh</p>
<p>Luegisland 2</p>
<p>8143 Stallikon</p></td>
</tr>
<tr>
<td><p><code>240214153000087</code></p></td>
<td><p>No streetname + house number</p></td>
<td><p>Betreibungsamt Sevelen</p>
<p>Rathaus</p>
<p>9475 Sevelen</p></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

# References

<div>

|  |  |
|----|----|
| Performance | <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="87e4c083-bcf5-461e-b0d2-e5c68782449b" macro-name="view-file"><a href="../_attachments/47920742401-performance.csv" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47920742401/performance.csv?version=1&amp;modificationDate=1720439433046&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/csv" data-has-thumbnail="true">

![[47920742401-performance.csv]]

</a></span> |
| Detailed Report |  |

</div>

%% ai-graph-start %%

**Related notes:**
- [[RAE Parser Quality Report 2024-06-06]]
- [[RAE Parser Quality Report 2024-05-03]]
- [[Global model evaluation]]
- [[Script 2022-03]]
- [[Test Keycloak - Public API]]

%% ai-graph-end %%