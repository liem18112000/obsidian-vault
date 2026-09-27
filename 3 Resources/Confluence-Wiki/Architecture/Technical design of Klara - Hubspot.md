---
title: "Technical design of Klara - Hubspot"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20497190417/Technical+design+of+Klara+-+Hubspot
space: "LUZ"
topic: architecture
relevance: 0.777
depth: 2.72
updated: 2021-04-13
attachments: 16
tags:
  - confluence
  - architecture
  - space/luz
---

# Technical design of Klara - Hubspot

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-04-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20497190417/Technical+design+of+Klara+-+Hubspot)
> Relevance 0.777 · topic `architecture`

Basically, integrating data from Klara to Hubspot is a new flow where we need to detect changes (for company and user data), extract those changes, transform to Hubspot format and sync it to Hubspot via its API. This confluence will show you two proposed designs which using ETL (extract, transform and load) and event driven design. However, aligning with our current design as well as urgent plan of this requirement, we focus mainly on ETL- Cronjob solution.

## 1. ETL using tracking table to handle failed sync tasks

Base on the design of ETL (luz_analytics_etl) service, we will implement new service named "hubspot", on it we define ETL interface and implement this interface for Company and User for the first stage. We also using hubspot-client to store schedule job which using to trigger hubspot-service scanning failed job from the last trigger. Hubspot by itself owner an database to store tracking synced status to Hubspot.  If some data can't sync to Hubspot due to internet connection or service unavailable, we can re-process it later by the triggered from hubspot-client.

Link to the design: <a href="https://drive.google.com/file/d/1w4HvQYb5-95zlEnLMc3RuZelFnaX0cIr/view?usp=sharing" class="external-link" rel="nofollow">https://drive.google.com/file/d/1w4HvQYb5-95zlEnLMc3RuZelFnaX0cIr/view?usp=sharing</a>

### 1.1. High level of using ETL to sync User when it is sign up or its role is changed


![[20497190417-image2019-12-24_10-21-30.png]]



### 1.2. High level of using ETL to sync Company when it is register or update


![[20497190417-image2019-12-24_10-22-5.png]]



### 1.3. Flow charts relate to tracking jobs


![[20497190417-image2019-12-20_10-49-37.png]]

                           

![[20497190417-image2019-12-20_11-49-28.png]]



### 1.4. Schema - table to tracking synced status

<div>

<table style="width: 75.7207%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th><br />
</th>
<th>Field name</th>
<th>Field type</th>
<th>Description</th>
<th>Required</th>
<th>Note</th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>job_id </p></td>
<td>bigserial</td>
<td>Primary key</td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>2</td>
<td>status</td>
<td>varchar(100)</td>
<td><p>Status of sync job</p>
<ul>
<li>NEW</li>
<li>FAILED</li>
<li>SUCCESS</li>
</ul></td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>3</td>
<td>created_date</td>
<td>timestamp</td>
<td>The date sync job created</td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>4</td>
<td>tenant_id</td>
<td>varchar(100)</td>
<td>Unique id to recognize right user/company</td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>5</td>
<td>sync_action</td>
<td>varchar(100)</td>
<td><p>User action to change data in klara</p>
<ul>
<li>CREATE</li>
<li>UPDATE</li>
</ul></td>
<td>Yes</td>
<td><br />
</td>
</tr>
<tr>
<td>6</td>
<td>request_body</td>
<td>Json</td>
<td><div class="content-wrapper">
<p>There are 2 type of request body:</p>
<ul>
<li><p>For user:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="8c921247-494d-4953-9d30-9a986a0a1306" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: text; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;action&quot;: &quot;UPDATE&quot;  #CREATE/UPDATE
    &quot;username&quot;: &quot;vutran&quot;,
    &quot;firstName&quot;: &quot;Vu&quot;,
    &quot;lastName&quot;: &quot;Tran&quot;
}</code></pre>
</div>
</div></li>
<li><p>For company:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c49d16c3-3684-441a-b0c0-e8f8d375ffba" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: text; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;action&quot;: &quot;CREATE&quot;  #CREATE/UPDATE
    &quot;companyId&quot;: &quot;Xaxzbs2&quot;
}</code></pre>
</div>
</div></li>
</ul>
</div></td>
<td>No</td>
<td><p>View more company and user field in:</p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20497189168/Klara+-+Hubspot+fields+mapping+via+API">Klara - Hubspot fields mapping (via API)</a></p></td>
</tr>
<tr>
<td>7</td>
<td>reason</td>
<td>varchar(255)</td>
<td><p>Error message when exception occur or response code from hubspot</p></td>
<td>No</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

  

## 2. Event driven design (idea)

### 2.1. Idea

Whenever company or user data are changed, we expose an corresponding event to a channel of message broker (such as Kafka) and then in somewhere, the job which are listening to that channel can consume and sync data to Hubspot. Same with batch job solution, we also have another job to reprocess message if it was failed last time.

To be reduced dependency and avoid causing current flows to errors , we also suggest to use *Interceptor* (*AOP* approach) and place it to any methods which making changes to company and user data. There is a sample implementation which using Interceptor in *luz_analytics_etc* (*client* module). Refer to this below link for more information: <a href="https://docs.oracle.com/javaee/7/tutorial/cdi-adv006.htm" class="external-link" rel="nofollow">https://docs.oracle.com/javaee/7/tutorial/cdi-adv006.htm</a>.

### 2.2. Pros and cons when apply ETL or Event driven design

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
<th><br />
</th>
<th>Feature</th>
<th>ETL</th>
<th>Event driven design</th>
</tr>
&#10;<tr>
<td>1</td>
<td>Support sync data real-time</td>
<td>No, but we can adjust schedule down to near real-time. However, apply this change might impact to performance issue.</td>
<td>Yes, anytime the change is created, sync task will process.</td>
</tr>
<tr>
<td>2</td>
<td>Re-process unprocessed messages</td>
<td>Yes if we define a log table where store status of each changes.</td>
<td>Yes, if we enable and implement retry task. This also a one key highlight feature of some message brokers as Kafka.</td>
</tr>
<tr>
<td>3</td>
<td>Decouple dependency</td>
<td>In case we need to modify some services (in-charge service) to support our business, we make a new dependency between those services with Hubspot.</td>
<td>Idea is whenever data is changed, the in-charge service will publish an event and that all. It doesn't care which service is listening to so we can reduce dependency between in-charge service with Hubspot. It also valuable when there are more than Hubspot needed.</td>
</tr>
<tr>
<td>4</td>
<td>...</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

## 3. Concerns need to be clear

This section list out all concern which belongs to technical and need to thinking more or need to be solved before implementation.

<div>

<table style="width: 88.1489%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><br />
</th>
<th>Concern</th>
<th>Solution</th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>How to update to company in Hubspot if they change some fields that we are using as primary key?</p>
<p>For example: user change their email address.</p></td>
<td><br />
</td>
</tr>
<tr>
<td>2</td>
<td><p>How to reprocess unprocessed data due to Hubspot API not available sometime or other situations?</p></td>
<td>By using tracking table, we can keep old failed messages and reprocess it later by reprocess job</td>
</tr>
<tr>
<td>3</td>
<td><p>Can update a part of data instead of re-submit whole data when only a part from it has changed?</p>
<p>For example: user change company name only and we only sync this field to Hubspot.</p></td>
<td><br />
</td>
</tr>
<tr>
<td>4</td>
<td>By applying ETL, we need to define a schedule of sync task, how long is suitable?</td>
<td><br />
</td>
</tr>
<tr>
<td>5</td>
<td>Access permission when a service call service</td>
<td><br />
</td>
</tr>
<tr>
<td>6</td>
<td>Should store Hubspot contact_id and company_id in Klara side to use for update later?</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

## 4. Consider to support Deal in the second generation

Along with Contact and Company, Deal is also a kind of content supported by Hubspot. What is Deal: whenever an action made by a contact that could lead to revenue, it should be a Deal. As mentioned from overview of <a href="https://axonivy.atlassian.net/wiki/display/LUZ/Overview+Interaction+between+KLARA+and+HubSpot" rel="nofollow">interaction between Klara and Hubspot</a>, in the next generation the Deal contents will be synced to Hubspot. Therefore, it should be a point to consider when making the technical design. Here is the link of what is Deals overview in Hubspot: <a href="https://developers.hubspot.com/docs/methods/deals/deals_overview" class="external-link" rel="nofollow">https://developers.hubspot.com/docs/methods/deals/deals_overview</a>

## 4. Consider to support Deal in the second generation
