---
ai_hash: 89159ba77f1c2b89
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.837
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47134507982/HubSpot+Document+for+API+create+custom+behavior+event
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: '[HubSpot] Document for API create custom behavior event'
topic: programming
type: source
updated: 2022-07-04
---

# [HubSpot] Document for API create custom behavior event

> [!info] Imported from Confluence
> Space **Helios** · updated 2022-07-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/47134507982/HubSpot+Document+for+API+create+custom+behavior+event)
> Relevance 0.837 · topic `programming`

**Create Custom Behavior Event endpoint:**

This method to create custom behavioral events for contact in Hubspot (Hubspot contact represents a user on Klara)

Example:


![[47134507982-image-20220623-053238.png]]



- Method: POST

- Endpoint:<a href="http://luz-hubspot:8080" class="external-link" rel="nofollow"><span>http://luz-hubspot:8080</span></a><a href="#" rel="nofollow"><span>/luz_hubspot/api/custom-behavior</span>al-events/create</a>

- Consumes: MediaType.APPLICATION_JSON

- Produces: MediaType.APPLICATION_JSON

- Body (JSON)

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
<th><p><strong>No.</strong></p></th>
<th><p><strong>Key</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Example value</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>properties</p></td>
<td><p>It the properties of the events in Hubspot, you can view all the properties in the table “<strong>Custom behavior event 's properties</strong>“ below</p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="92b6c9c8-f6a4-4431-86f4-c1bda0f227fd" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;properties&quot;:{
      &quot;app_name&quot;:&quot;mylife&quot;,
      &quot;sender&quot;:&quot;KLARA&quot;,
      &quot;channel&quot;:&quot;Push Notification&quot;,
      &quot;app_language&quot;:&quot;DE&quot;,
      &quot;state&quot;:&quot;25 days&quot;,
      &quot;flow&quot;:&quot;inactive private customers&quot;
   }</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>email</p></td>
<td><p>Email of user</p></td>
<td><p>conghau@gmail.com</p></td>
</tr>
</tbody>
</table>

</div>

**Example:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1ea83358-567d-4698-9bec-abb8ae3458e2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
   "properties":{
      "app_name":"mylife",
      "sender":"KLARA",
      "channel":"Push Notification",
      "app_language":"DE",
      "state":"25 days",
      "flow":"inactive private customers"
   },
   "email":"phuoc.nguyenvinh@axonactive.com"
}
```

</div>

</div>

  
**Custom behavior event 's properties:**

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
<th><p><strong>No</strong></p></th>
<th><p><strong>Key</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>All posible value</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>channel</p></td>
<td><p><span>Only require in offbroading flow</span></p>
<p>Channel of offbroading flow</p></td>
<td><p>E-mail, SMS, Push Notification</p></td>
</tr>
<tr>
<td><p>2</p></td>
<td><p>sender</p></td>
<td><p>Email sender</p></td>
<td><p>KLARA, Hubspot</p></td>
</tr>
<tr>
<td><p>3</p></td>
<td><p>flow</p></td>
<td><p>Decide the flow for the email</p></td>
<td><p>inactive private customers, invitation share inbox</p></td>
</tr>
<tr>
<td><p>4</p></td>
<td><p>state</p></td>
<td><p><span>Only require in offbroading flow</span></p>
<p>State of offbroading flow</p></td>
<td><p>25 days, 30 days, 40 days, 50 days, 60 days, 70 days</p></td>
</tr>
<tr>
<td><p>5</p></td>
<td><p>app_language</p></td>
<td><p>Language to be use in the email</p></td>
<td><p>EN, DE, FR, IT</p></td>
</tr>
<tr>
<td><p>6</p></td>
<td><p>app_name</p></td>
<td><p>The name off the app it can be KLARA or EPOST</p></td>
<td><p>epost, mylife</p></td>
</tr>
</tbody>
</table>

</div>

- **Return: javax.ws.rs.core.Response**

Status 200 when success

Status 400 when create unsuccessful

Status 500 when exception occurs

- **cURL example:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="31e1ae2b-315e-426d-b62d-2e0f41aaebcf" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request POST 'http://localhost:8080/luz_hubspot/api/custom-behavioral-events/create' \
--header 'Content-Type: application/json' \
--data-raw '{
    "properties":{
        "channel": "Push Notification",
        "sender": "KLARA",
        "flow": "inactive private customers",
        "state": "70 days",
        "app_language": "DE",
        "app_name":"mylife"
    },
    "email": "conghau@gmail.com"
}'
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[API Document]]
- [[5. Analyze the current data status of company between Hubspot and Klara]]
- [[Technical design of Klara - Hubspot]]
- [[KLARA Booking - KLARA OBC API]]
- [[LUZ-110826 Public API - Widget subscription by activation code]]

%% ai-graph-end %%