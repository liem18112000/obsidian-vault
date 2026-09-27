---
title: "Query API Reference"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2524828596/Query+API+Reference
space: "AI"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2021-01-21
attachments: 0
tags:
  - confluence
  - programming
  - space/ai
---

# Query API Reference

> [!info] Imported from Confluence
> Space **AI** · updated 2021-01-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2524828596/Query+API+Reference)
> Relevance 0.731 · topic `programming`

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2" macro-id="c087d2f0-ba8e-4ac5-9224-3a0b5d2840ae" macro-name="toc">

</div>

# Authentication

Query API uses the basic authentication scheme.

# Methods

## Get schema

Retrieve the schema of a specific data source.

#### Request

<div>

<table>
<tbody>
<tr>
<th>Method</th>
<td colspan="2"><strong>GET</strong></td>
</tr>
<tr>
<th>URL</th>
<td colspan="2"><strong>/schema/{sourceId}</strong></td>
</tr>
<tr>
<th>Headers</th>
<td>Authorization</td>
<td>Basic &lt;CREDENTIALS&gt;</td>
</tr>
<tr>
<th>Parameters</th>
<td>sourceId</td>
<td>The name of the data source to get the schema for</td>
</tr>
</tbody>
</table>

</div>

#### Response

##### Result

<div>

|          |           |                                      |
|----------|-----------|--------------------------------------|
| Property | Type      | Description                          |
| schema   | Field\[\] | The schema as a collection of fields |

</div>

##### Field

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Property</th>
<th>Type</th>
<th>Description</th>
</tr>
&#10;<tr>
<td>name</td>
<td>string</td>
<td>Unique identifier of the field</td>
</tr>
<tr>
<td>fieldType</td>
<td>string</td>
<td><p>The fields type</p>
<ul>
<li>PHYSICAL, describes real persisted data</li>
<li>LOGICAL, derived from other fields by a formula (cannot be used in a query)</li>
<li>VIRTUAL, computed by the server at query time</li>
</ul></td>
</tr>
<tr>
<td>dataType</td>
<td>string</td>
<td><p>The type of the fields data</p>
<ul>
<li><p>BOOLEAN</p></li>
<li><p>NUMBER</p></li>
<li><p>STRING</p></li>
</ul></td>
</tr>
<tr>
<td>semanticType</td>
<td>string</td>
<td><p>The semantic meaning of the field.</p>
<p>Examples:</p>
<ul>
<li>YEAR</li>
<li>MONTH</li>
<li>NUMBER</li>
<li>PERCENT</li>
<li>TEXT</li>
</ul>
<p>See <a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.query/src/master/com.axonivy.ai.query.common/src/main/java/com/axonivy/ai/query/common/SemanticType.java" class="external-link" rel="nofollow">SemanticType.java</a> for more information.</p></td>
</tr>
<tr>
<td>conceptType</td>
<td>string</td>
<td><p>The concept type of the field</p>
<ul>
<li>DIMENSION</li>
<li>METRIC</li>
</ul></td>
</tr>
<tr>
<td>aggregationType</td>
<td>string</td>
<td><p>Information about how to aggregate the field's value.</p>
<p>Examples:</p>
<ul>
<li>SUM</li>
<li>COUNT</li>
<li>MAX</li>
</ul>
<p>See <a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.query/src/master/com.axonivy.ai.query.common/src/main/java/com/axonivy/ai/query/common/AggregationType.java" class="external-link" rel="nofollow">AggregationType.java</a> for more information.</p></td>
</tr>
</tbody>
</table>

</div>

#### Example Response 

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td>Content-Type</td>
<td>application/json</td>
</tr>
<tr>
<td colspan="2"><p><code>{</code><br />
<code>    </code><code>"schema": [</code><code>{</code></p>
<p><code>        "name": "meta.ingestiontimestamp",</code><br />
<code>        </code><code>"fieldType": "PHYSICAL",</code><br />
<code>        </code><code>"dataType": "STRING",</code><br />
<code>        </code><code>"semanticType": "TEXT",</code><br />
<code>        </code><code>"conceptType": "DIMENSION",</code><br />
<code>        </code><code>"aggregationType": "NO_AGGREGATION"</code><br />
<code>    </code><code>}]</code><br />
<code>}</code></p></td>
</tr>
</tbody>
</table>

</div>

## Get data

Retrieve data from a specific data source.

#### Request

<div>

<table>
<tbody>
<tr>
<th>Method</th>
<td colspan="2"><strong>GET</strong></td>
</tr>
<tr>
<th>URL</th>
<td colspan="2"><strong>/data/{sourceId}</strong></td>
</tr>
<tr>
<th>Headers</th>
<td>Authorization</td>
<td>Basic &lt;CREDENTIALS&gt;</td>
</tr>
<tr>
<th>Parameters</th>
<td>sourceId</td>
<td>The name of the data source to get the schema for</td>
</tr>
<tr>
<th rowspan="2">Query</th>
<td>field</td>
<td>The name of the field to retrieve. This parameter can be used zero or more times</td>
</tr>
<tr>
<td>filter</td>
<td>A filter expression to apply in order to reduce the amount of data returned</td>
</tr>
</tbody>
</table>

</div>

#### Example Request

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="38b2758f-e4e0-496a-8810-cdbddf6625d9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
GET /data/klara.lake.core.User?field=meta.batchid&field=meta.id&filter=((not)((eq)(meta.dataquality)('COMPLETE')))
```

</div>

</div>

#### Response

##### Result

<div>

|          |           |                                      |
|----------|-----------|--------------------------------------|
| Property | Type      | Description                          |
| schema   | Field\[\] | The schema as a collection of fields |
| rows     | Row\[\]   | A list of data rows                  |

</div>

##### Row

<div>

|          |           |                                          |
|----------|-----------|------------------------------------------|
| Property | Type      | Description                              |
| value    | value\[\] | A list of values according to the schema |

</div>

#### Example Response 

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td>Content-Type</td>
<td>application/json</td>
</tr>
<tr>
<td colspan="2"><p><code>{</code><br />
<code>    </code><code>"schema": [</code><code>{</code></p>
<p><code>        "name": "meta.ingestiontimestamp",</code><br />
<code>        </code><code>"fieldType": "PHYSICAL",</code><br />
<code>        </code><code>"dataType": "STRING",</code><br />
<code>        </code><code>"semanticType": "TEXT",</code><br />
<code>        </code><code>"conceptType": "DIMENSION",</code><br />
<code>        </code><code>"aggregationType": "NO_AGGREGATION"</code><br />
<code>    </code><code>}],</code><br />
<code>    </code><code>"rows": [</code><code>{</code><br />
<code>        </code><code>"values": [</code><br />
<code>            </code><code>"2020-08-26T04:14:23.009Z"</code><br />
<code>        ]</code><br />
<code>    }]</code><br />
<code>}</code></p></td>
</tr>
</tbody>
</table>

</div>

# Filter Expression

A filter represents a boolean expression that describes how to refine a set of data.

## Syntax

The syntax of the filter expression looks as follows (BNF)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6c2870eb-16f9-49df-9177-69494571b7f1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<filter> ::= <node>
<node>   ::= <atom> | <list>
<atom>   ::= '(' <symbol> ')'
<list>   ::= '(' 1 * <node> ')'
<symbol> ::= 'NOT' | 'AND' | 'OR' | 'EQ' | 'GE' | 'GT' | 'LE' | 'LT' | 'LE' | 'NE' | <field>
<field>  ::= 1 * ( <alpha-lowercase> | '.' | '_' )
```

</div>

</div>

## Examples

<div>

|  |  |
|----|----|
| Expression | Description |
| `((EQ)(batchId)('abc-def-ghi'))` | The value of field 'batchId' must be equal to 'abc-def-ghi' |
| `((NOT)((EQ)(batchid)('abc-def-ghi')))` | The value of field 'batchId' must not be equal to 'abc-def-ghi' |
| `((AND)((GE)(eventdate)('2019-01-20'))((LE)(eventdate)('2019-01-31')))` | The value of field 'eventDate' must be between '2019-01-20' (inclusive) and '2019-01-31' (inclusive). |

</div>
