---
title: "LUZ-106177 - Delete credit card expiry reminder"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47482765795/LUZ-106177+-+Delete+credit+card+expiry+reminder
space: "TS"
topic: programming
relevance: 0.716
depth: 2.89
updated: 2023-09-13
attachments: 48
tags:
  - confluence
  - programming
  - space/ts
---

# LUZ-106177 - Delete credit card expiry reminder

> [!info] Imported from Confluence
> Space **TS** · updated 2023-09-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47482765795/LUZ-106177+-+Delete+credit+card+expiry+reminder)
> Relevance 0.716 · topic `programming`

![[47482765795-Delete credit card info.png]]



<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47482765795_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-106177" macro-id="b70411f0-a450-4ec6-ba65-dad7f39eb4e5" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-106177" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-106177</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

[LUZ-78318 Inform customer about expiring credit card](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47151710609/LUZ-78318+Inform+customer+about+expiring+credit+card)

[Create and verify an address for individual tenant.](https://axonivy.atlassian.net/wiki/spaces/WOW/pages/47039676479/Create+and+verify+an+address+for+individual+tenant.)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d2d9a19f-adbd-433c-a972-f29ca04dd2d0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
nope, we only have a Scanning-as-a-Service_PRIVATE widget for ePost and Individual tenant.
```

</div>

</div>

Setup:

<div id="expander-1468870031" class="expand-container conf-macro output-block" hasbody="true" macro-id="7c74bbca-dd17-4058-9aa7-a603605c4bc3" macro-name="expand">

<div id="expander-control-1468870031" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">luz_online_payment</span>

</div>

<div id="expander-content-1468870031" class="expand-content expand-hidden">

Add endpoint to trigger expired card reminder cronjob

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-progress conf-macro output-inline" hasbody="false" macro-id="28e4313d-4cde-468e-abb7-9cf6278ff438" macro-name="status">POST</span>

`/luz_online_payment/api/test/trigger-expire-card-reminder-cronjob`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e9a02b69-9b04-413a-b57a-2114746591a9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package ch.klara.onlinepayment.rest;

import ch.klara.onlinepayment.scheduling.service.ExpireCardReminderCronjobRunner;

import javax.annotation.security.PermitAll;
import javax.inject.Inject;
import javax.ws.rs.POST;
import javax.ws.rs.Path;
import javax.ws.rs.core.Response;

@Path("/test")
@PermitAll
public class TestResource {

    @Inject
    ExpireCardReminderCronjobRunner expireCardReminderCronjobRunner;

    @POST
    @Path("/trigger-expire-card-reminder-cronjob")
    public Response triggerExpireCardReminderCronjob() {
        expireCardReminderCronjobRunner.run();
        return Response.ok().build();
    }

}
```

</div>

</div>

</div>

</div>

<div id="expander-1933272964" class="expand-container conf-macro output-block" hasbody="true" macro-id="046c8595-5828-42df-8eac-9ed6f8019e2a" macro-name="expand">

<div id="expander-control-1933272964" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Credit card</span>

</div>

<div id="expander-content-1933272964" class="expand-content expand-hidden">

4242 4242 4242 4242

</div>

</div>

Credit card info for testing: <a href="https://www.creditcardvalidator.org/country/ch-switzerland" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.creditcardvalidator.org/country/ch-switzerland</a>

<div>

<table style="width:100%;">
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
<th><p><strong>Test case</strong></p></th>
<th><p><strong>Test step</strong></p></th>
<th><p><strong>Expectation</strong></p></th>
<th><p><strong>Actual</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Delete COMPANY tenant</p></td>
<td><ol>
<li><p>Create a company (if not exist)</p></li>
<li><p>Buy a new widget =&gt; using credit card</p></li>
<li><p>Change the expire time in database</p></li>
<li><p>Trigger the job to send reminder</p></li>
<li><p>Update the reminder status to <code>NOT_REMIND</code></p></li>
<li><p>Delete the company</p></li>
<li><p>Check reminder status (<code>SKIP_REMIND</code>)</p></li>
<li><p>Trigger the job to send reminder</p></li>
<li><p>Trigger physical deletion</p></li>
</ol></td>
<td><ol>
<li><p>Receive reminder in #4</p></li>
<li><p>Reminder status is <code>SKIP_REMIND</code> in #7</p></li>
<li><p>Don’t receive reminder in #8</p></li>
<li><p>Credit card info is deleted in #9</p></li>
</ol></td>
<td><div id="expander-1710616907" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="e2503910-0775-4e9c-ada6-c062b1090a3c" data-macro-name="expand">
<div id="expander-control-1710616907" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-1710616907" class="expand-content expand-hidden">
<p>#4</p>

![[47482765795-image-20230911-090733.png]]

![[47482765795-image-20230911-090725.png]]


<p>#7</p>

![[47482765795-image-20230911-091106.png]]

![[47482765795-image-20230911-091100.png]]


<p>#8</p>

![[47482765795-image-20230911-091138.png]]


<p>#9</p>

![[47482765795-image-20230911-091252.png]]


</div>
</div></td>
<td><p>

![[47482765795-check.png]]

</p></td>
<td><div id="expander-1641594156" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="04db8801-49a4-4f1c-9289-1a7b66387fff" data-macro-name="expand">
<div id="expander-control-1641594156" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Note</span>
</div>
<div id="expander-content-1641594156" class="expand-content expand-hidden">
<p>fede9a0f-f647-4679-9518-941a6338b1a3</p>

![[47482765795-image-20230911-064038.png]]

![[47482765795-image-20230911-064345.png]]


</div>
</div></td>
</tr>
<tr>
<td><p>Delete INDIVIDUAL tenant</p></td>
<td><p>Same as COMPANY tenant</p>
<p>NOTE:</p>
<ul>
<li><p>Subcribe <code>Scanning-as-a-Service_PRIVATE widget</code> for INDIVIDUAL tenant</p></li>
</ul></td>
<td><p>Same as COMPANY tenant</p></td>
<td><div id="expander-588175188" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="d2822122-86f3-4126-85b2-3530d9bad968" data-macro-name="expand">
<div id="expander-control-588175188" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Details</span>
</div>
<div id="expander-content-588175188" class="expand-content expand-hidden">

![[47482765795-image-20230911-111231.png]]

![[47482765795-image-20230911-111908.png]]

![[47482765795-image-20230911-111854.png]]

![[47482765795-image-20230911-111920.png]]


<p>Logical deletion</p>

![[47482765795-image-20230911-112143.png]]


<p>No new email</p>

![[47482765795-image-20230911-112214.png]]

![[47482765795-image-20230911-112224.png]]


<p>Physical deletion</p>

![[47482765795-image-20230911-112319.png]]


</div>
</div></td>
<td><p>

![[47482765795-check.png]]

</p></td>
<td><div id="expander-1878299801" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="62ffb6bb-eed0-4603-868f-21d38292c371" data-macro-name="expand">
<div id="expander-control-1878299801" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Note</span>
</div>
<div id="expander-content-1878299801" class="expand-content expand-hidden">
<p>1cd9a305-ecad-4506-bdf4-4ff238eb0204</p>

![[47482765795-image-20230911-110510.png]]

![[47482765795-image-20230911-110557.png]]

![[47482765795-image-20230911-111205.png]]


</div>
</div></td>
</tr>
</tbody>
</table>

</div>

# Test charging money job

luz_store:miracle/LUZ-98265/precondition-company-deletion

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th colspan="2"><p><strong>Test case</strong></p></th>
<th><p><strong>Test step</strong></p></th>
<th><p><strong>Expectation</strong></p></th>
<th><p><strong>Actual</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td rowspan="2"><p>Company</p></td>
<td><p>Logical deletion</p></td>
<td><ol>
<li><p>Create a new company</p></li>
<li><p>Subcribe widget with credit card</p></li>
<li><p>Update start billing date (Admin GUI → Widget Store → Subscription) and delete testing duration</p></li>
</ol>

![[47482765795-image-20230912-073218.png]]


<ol start="4">
<li><p>Run billing job <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/47430861385/Billing">Billing</a></p></li>
<li><p>Logical delete for this company</p>

![[47482765795-image-20230912-083726.png]]

</li>
<li><p>Trigger invoice run <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/47431155866/Admin+GUI+after+delete+tenant+-+Billing+and+Invoice+run">Admin GUI after delete tenant - Billing and Invoice run</a></p>

![[47482765795-image-20230912-084624.png]]

</li>
</ol></td>
<td></td>
<td>

![[47482765795-image-20230912-085644.png]]

</td>
<td><p>

![[47482765795-check.png]]

</p></td>
<td><p>Company name: <code>Miracle - Loc - 23.09.12 - Logical deletion for credit card info</code></p>
<p>Company ID: <code>4de78c4c-69db-4363-9d9f-85ea1e79e178</code></p>
<p>Subscription</p>

![[47482765795-image-20230912-070451.png]]

![[47482765795-image-20230912-073302.png]]


<p>Billing</p>

![[47482765795-image-20230912-073541.png]]

</td>
</tr>
<tr>
<td><p>Physical deletion</p></td>
<td><ol>
<li><p>Rollback invoice run</p></li>
<li><p>Physical delete tenant</p>

![[47482765795-image-20230912-091746.png]]

</li>
<li><p>Trigger invoice run <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/47431155866/Admin+GUI+after+delete+tenant+-+Billing+and+Invoice+run">Admin GUI after delete tenant - Billing and Invoice run</a></p></li>
</ol></td>
<td></td>
<td>

![[47482765795-image-20230912-092026.png]]

</td>
<td><p>

![[47482765795-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td rowspan="2"><p>Individual</p></td>
<td><p>Logical deletion</p></td>
<td><ol>
<li><p>Create a new company</p></li>
<li><p>Subcribe widget with credit card</p></li>
<li><p>Update start billing date (Admin GUI → Widget Store → Subscription) and delete testing duration</p>

![[47482765795-image-20230912-080517.png]]

</li>
<li><p>Run billing job <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/47430861385/Billing">Billing</a></p></li>
<li><p>Logical delete for this company (set luz_store.subscription.subscription_until to NULL → unsubscribe)</p>

![[47482765795-image-20230912-083734.png]]

</li>
<li><p>Trigger invoice run <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/47431155866/Admin+GUI+after+delete+tenant+-+Billing+and+Invoice+run">Admin GUI after delete tenant - Billing and Invoice run</a> - <code>Individual Tenant Invoices</code></p>

![[47482765795-image-20230912-090444.png]]

</li>
</ol></td>
<td></td>
<td>

![[47482765795-image-20230912-090756.png]]

</td>
<td><p>

![[47482765795-check.png]]

</p>
<p>Note: Must change the database to unsubscribe widget and delete tenant</p></td>
<td><p>Company name: <code>Alfred Meier a9c611fa-9b18-4e25-87f2-3ff6dd3f933c</code></p>
<p>Tenant ID: <code>a9c611fa-9b18-4e25-87f2-3ff6dd3f933c</code></p>
<p>Subscription</p>

![[47482765795-image-20230912-080345.png]]

![[47482765795-image-20230912-080527.png]]


<p>Billing</p>

![[47482765795-image-20230912-081231.png]]

</td>
</tr>
<tr>
<td><p>Physical deletion</p></td>
<td><ol>
<li><p>Rollback invoice run</p></li>
<li><p>Physical delete tenant</p>

![[47482765795-image-20230912-091739.png]]

</li>
<li><p>Trigger invoice run <a href="https://axonivy.atlassian.net/wiki/spaces/TS/pages/47431155866/Admin+GUI+after+delete+tenant+-+Billing+and+Invoice+run">Admin GUI after delete tenant - Billing and Invoice run</a> - <code>Individual Tenant Invoices</code></p></li>
</ol></td>
<td></td>
<td>

![[47482765795-image-20230912-092302.png]]

</td>
<td><p>

![[47482765795-check.png]]

</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>
