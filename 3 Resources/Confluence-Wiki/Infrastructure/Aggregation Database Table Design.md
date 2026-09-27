---
title: "Aggregation Database Table Design"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513302619/Aggregation+Database+Table+Design
space: "LUZ"
topic: infra
relevance: 0.755
depth: 2.7
updated: 2020-11-06
attachments: 0
tags:
  - confluence
  - infra
  - space/luz
---

# Aggregation Database Table Design

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-11-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20513302619/Aggregation+Database+Table+Design)
> Relevance 0.755 · topic `infra`

### 1) How can we make sure, that the aggregated data set is up to date?

- <span class="inline-comment-marker" ref="88f3c57e-4b79-4cde-a718-bf6ca96175e4">There will be google cloud pub/sub approach to call in aggregation module(luz_online) when companies and posts data insert or update or delete.</span>
- E.g when user will create the new company, first company will be created in appropriate module in Klara and then that module call the google cloud pub/sub to insert the company record by using create API. Same for the posts details.

### 2) Who will trigger; luz_online will pull it or each module push it to luz_online? - or does it make sense to have both approaches?

- We recommend that Klara module will push the companies and posts data to google cloud pub/sub  when they create, update, delete it. (Note : Respective Klara module need to implement google cloud pub/sub. )
- We will implement google cloud pub/sub listener in luz_online module.

### 3) How are we going to handle error cases? E.g. data is not delivered to aggregation storage?

- We will use default google cloud pub/sub error logs and handlers.
- Klara module need to handle when google cloud pub/sub return error. 

### 4) Database Table/<span class="legacy-color-text-blue3">Design</span>:

Post(News/Event/Deal):

<div>

<table style="width: 100.0%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th style="width: 17.8636%">Fields</th>
<th style="width: 22.3519%">Type(size)</th>
<th style="width: 26.93%">comments</th>
<th style="width: 32.7648%">Description </th>
</tr>
&#10;<tr>
<td style="width: 17.8636%"><span class="legacy-color-text-default">id</span></td>
<td style="width: 22.3519%">BIGINT(20)</td>
<td style="width: 26.93%">auto increment, primary key, unsign, NOT NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><span class="legacy-color-text-default">image_url</span></td>
<td style="width: 22.3519%">TEXT</td>
<td style="width: 26.93%">NOT NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><span class="legacy-color-text-default">title_de</span></td>
<td style="width: 22.3519%"><span>VARCHAR</span>(64)</td>
<td style="width: 26.93%">NOT NULL, index</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">title_en</span></td>
<td><span>VARCHAR</span>(64)</td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">title_fr</span></td>
<td><span>VARCHAR</span>(64)</td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">title_it</span></td>
<td><span>VARCHAR</span>(64)</td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><p><span>lead_text_de</span></p></td>
<td style="width: 22.3519%"><p><span>VARCHAR(255)</span></p></td>
<td style="width: 26.93%">NOT NULL, index</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td><span>lead_text_en</span></td>
<td><span>VARCHAR(255)</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span>lead_text_fr</span></td>
<td><span>VARCHAR(255)</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span>lead_text_it</span></td>
<td><span>VARCHAR(255)</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><p><span>content_de</span></p></td>
<td style="width: 22.3519%"><span>TEXT</span></td>
<td style="width: 26.93%">index</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td><span>content_en</span></td>
<td><span>TEXT</span></td>
<td>index</td>
<td><br />
</td>
</tr>
<tr>
<td><span>content_fr</span></td>
<td><span>TEXT</span></td>
<td>index</td>
<td><br />
</td>
</tr>
<tr>
<td><span>content_it</span></td>
<td><span>TEXT</span></td>
<td>index</td>
<td><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><span class="legacy-color-text-blue3">website</span></td>
<td style="width: 22.3519%"><span>VARCHAR</span>(255)</td>
<td style="width: 26.93%"><br />
</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%">start_date</td>
<td style="width: 22.3519%"><span>TIMESTAMP</span></td>
<td style="width: 26.93%">NOT NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%">end_date</td>
<td style="width: 22.3519%"><span>TIMESTAMP</span></td>
<td style="width: 26.93%">NOT NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%">post_id</td>
<td style="width: 22.3519%">BIGINT(20)</td>
<td style="width: 26.93%">NOT NULL, index</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%">tenant_id</td>
<td style="width: 22.3519%"><span>VARCHAR</span>(255)</td>
<td style="width: 26.93%">NOT NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td>company_id</td>
<td>BIGINT(20)</td>
<td>NOT NULL</td>
<td><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><p><span>post_type</span></p></td>
<td style="width: 22.3519%"><span>VARCHAR(16)</span></td>
<td style="width: 26.93%">NOT NULL, index</td>
<td style="width: 32.7648%"><p><span class="legacy-color-text-default"> </span><span>NEWS</span><span class="legacy-color-text-default">, </span><span>EVENT</span><span class="legacy-color-text-default">, </span><span>REGIO_DEAL</span></p></td>
</tr>
<tr>
<td style="width: 17.8636%">event_type</td>
<td style="width: 22.3519%"><span>VARCHAR(16)</span></td>
<td style="width: 26.93%">NOT NULL</td>
<td style="width: 32.7648%"><p><span>SINGLE</span><span class="legacy-color-text-default">(</span><span>"single"</span><span class="legacy-color-text-default">), </span><span>RANGE</span><span class="legacy-color-text-default">(</span><span>"range"</span><span class="legacy-color-text-default">),</span><br />
<span class="legacy-color-text-default"> </span><span>MULTIPLE</span><span class="legacy-color-text-default">(</span><span>"multiple"</span><span class="legacy-color-text-default">)</span></p></td>
</tr>
<tr>
<td style="width: 17.8636%"><p><span>category_id</span></p></td>
<td style="width: 22.3519%"><p><span>INT(4)</span></p></td>
<td style="width: 26.93%"><br />
</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%">latitude</td>
<td style="width: 22.3519%"><span>DOUBLE</span></td>
<td style="width: 26.93%"><br />
</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%">longitude</td>
<td style="width: 22.3519%"><span>DOUBLE</span></td>
<td style="width: 26.93%"><br />
</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><span class="legacy-color-text-default"><span class="legacy-color-text-blue3">company_</span>logo_url</span></td>
<td style="width: 22.3519%">TEXT</td>
<td style="width: 26.93%">NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td style="width: 17.8636%"><span class="legacy-color-text-default"><span class="legacy-color-text-blue3">company</span>_name</span></td>
<td style="width: 22.3519%"><span>VARCHAR(255)</span></td>
<td style="width: 26.93%">NOT NULL</td>
<td style="width: 32.7648%"><br />
</td>
</tr>
<tr>
<td colspan="4" style="width: 99.9103%"><br />
</td>
</tr>
</tbody>
</table>

</div>

event_period table:

<div>

<table style="width: 61.312%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>Fields</th>
<th>Type(size)</th>
<th>comments</th>
<th>Description </th>
</tr>
&#10;<tr>
<td><span class="legacy-color-text-default">id</span></td>
<td>BIGINT(20)</td>
<td>auto increment, primary key, unsign</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">post_Id</span></td>
<td>BIGINT(20)</td>
<td><p><br />
</p></td>
<td>Klara post_id</td>
</tr>
<tr>
<td>start_date</td>
<td><p><span>TIMESTAMP</span></p></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>end_date</td>
<td><p><span>TIMESTAMP</span></p></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><p>single_date</p></td>
<td><span>TIMESTAMP</span></td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

Company Table:

<div>

<table style="width: 61.5643%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>Fields</th>
<th>Type(size)</th>
<th>comments</th>
<th>Description </th>
</tr>
&#10;<tr>
<td><span class="legacy-color-text-default">id</span></td>
<td>BIGINT(20)</td>
<td>auto increment, primary key, unsign, NOT NULL</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">logo_url</span></td>
<td>TEXT</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>banner_url</td>
<td>TEXT</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">name</span></td>
<td><span>VARCHAR(255)</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><p><span class="legacy-color-text-default">description_de</span></p></td>
<td><p><span>TEXT</span></p></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">description_en</span></td>
<td><span>TEXT</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">description_fr</span></td>
<td><span>TEXT</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-default">description_it</span></td>
<td><span>TEXT</span></td>
<td>NOT NULL, index</td>
<td><br />
</td>
</tr>
<tr>
<td><span class="legacy-color-text-blue3">website</span></td>
<td><span>VARCHAR</span>(255)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><span>category_id</span></td>
<td><p><span>INT(4)</span></p></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>tenant_id</td>
<td><span>VARCHAR</span>(255)</td>
<td>NOT NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>company_id</td>
<td>BIGINT(20)</td>
<td>NOT NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>latitude</td>
<td><span>DOUBLE</span></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>longitude</td>
<td><span>DOUBLE</span></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>address_lines</td>
<td><span>VARCHAR</span>(255)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>city</td>
<td><span>VARCHAR</span>(255)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>state</td>
<td><span>VARCHAR</span>(255)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>postal_code</td>
<td><span>VARCHAR</span>(255)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>country</td>
<td><span>VARCHAR</span>(255)</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>has_stampcard</td>
<td>BOOLEAN</td>
<td>DEFAULT FALSE</td>
<td><br />
</td>
</tr>
<tr>
<td colspan="4"><br />
</td>
</tr>
</tbody>
</table>

</div>

### Open questions:

<div>

<table style="width: 79.2101%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>No.</th>
<th>Questions</th>
<th>Answer </th>
<th>Updated on</th>
</tr>
&#10;<tr>
<td>1.</td>
<td>Currently regions are not available in Klara, Do we still want to store region details in aggregate tables?</td>
<td><div class="content-wrapper">
<p><a href="https://axonivy.atlassian.net/wiki/people/5e4b8f695a495e0c91a81329?ref=confluence" class="confluence-userlink user-mention" data-account-id="5e4b8f695a495e0c91a81329" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">krunal</a> <a href="https://axonivy.atlassian.net/wiki/people/5e4b8f643011ed0c8f8acb85?ref=confluence" class="confluence-userlink user-mention" data-account-id="5e4b8f643011ed0c8f8acb85" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Pratik Patel</a> <a href="https://axonivy.atlassian.net/wiki/people/557058:2c2ae6f3-cacb-4b43-9f56-7ae97cbfe0dd?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:2c2ae6f3-cacb-4b43-9f56-7ae97cbfe0dd" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">bhavin_avdevs (Unlicensed)</a> <a href="https://axonivy.atlassian.net/wiki/people/5e4b8f643011ed0c8f8acb85?ref=confluence" class="confluence-userlink user-mention" data-account-id="5e4b8f643011ed0c8f8acb85" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Pratik Patel</a> <a href="https://axonivy.atlassian.net/wiki/people/557058:e9646052-695a-4abb-a642-ca1c00918951?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:e9646052-695a-4abb-a642-ca1c00918951" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Dhaval Patel (Unlicensed)</a> <a href="https://axonivy.atlassian.net/wiki/people/5b442bedbdb143114a16b233?ref=confluence" class="confluence-userlink user-mention" data-account-id="5b442bedbdb143114a16b233" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Shailendra Gohil (Unlicensed)</a> no, regions are not necessary.</p>
</div></td>
<td><div class="content-wrapper">
<p>15.09.2020 <a href="https://axonivy.atlassian.net/wiki/people/5a096f417174496061892bb5?ref=confluence" class="confluence-userlink user-mention" data-account-id="5a096f417174496061892bb5" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Robin Engbersen</a></p>
</div></td>
</tr>
</tbody>
</table>

</div>
