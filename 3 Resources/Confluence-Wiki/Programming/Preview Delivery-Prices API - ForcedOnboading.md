---
title: "Preview Delivery-Prices API - ForcedOnboading"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48773038081/Preview+Delivery-Prices+API+-+ForcedOnboading
space: "HACKA"
topic: programming
relevance: 0.792
depth: 3
updated: 2025-10-21
attachments: 1
tags:
  - confluence
  - programming
  - space/hacka
---

# Preview Delivery-Prices API - ForcedOnboading

> [!info] Imported from Confluence
> Space **HACKA** · updated 2025-10-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/48773038081/Preview+Delivery-Prices+API+-+ForcedOnboading)
> Relevance 0.792 · topic `programming`

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48773038081_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-142173" macro-id="1ae702ff-9a5d-4ebe-9334-474499a61216" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-142173" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-142173</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48773038081_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-137253" macro-id="59c0aa2b-854d-4b07-bb82-ec8beaebb719" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-137253" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-137253</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

Preview delivery-prices API

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Case</strong></p></th>
<th><p><strong>Payload</strong></p></th>
<th><p><strong>Expected</strong></p></th>
<th><p><strong>Actual</strong></p></th>
<th><p><strong>Task</strong></p></th>
</tr>
&#10;<tr>
<td><ul>
<li><p>ForcedOnboarding</p></li>
<li><p>

![[48773038081-error.png]]

 No Match tenant</p>
<ul>
<li><p>Email</p></li>
</ul></li>
<li><p>ChannelPreferences</p>
<ul>
<li><p>DIGITAL</p></li>
<li><p>EBILL</p></li>
<li><p>EMAIL</p></li>
<li><p>PHYSICAL</p></li>
</ul></li>
</ul></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c2a94b69-c2e0-492f-8f00-439fe733daf1" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
   &quot;fileMetadata&quot;:[
      {
         &quot;fileName&quot;:&quot;Test.pdf&quot;,
         &quot;deliveryChannelPreferences&quot;:[
            &quot;DIGITAL&quot;,
            &quot;EBILL&quot;,
            &quot;EMAIL&quot;,
            &quot;PHYSICAL&quot;
         ],
         &quot;forcedOnboarding&quot;:{
            &quot;channel&quot;:&quot;EMAIL&quot;,
            &quot;language&quot;:&quot;en&quot;,
            &quot;emailToBeInformedWhenExpired&quot;:&quot;yan@hcsystems.de&quot;
         },
         &quot;documentReferenceDate&quot;:&quot;2025-10-13&quot;,
         &quot;ebillAdditionalInfo&quot;:{
            &quot;dueDate&quot;:&quot;2025-10-13&quot;,
            &quot;paymentAmount&quot;:485.15,
            &quot;qrReference&quot;:&quot;123456001690133632012308266&quot;
         },
         &quot;physicalAdditionalInfo&quot;:{
            &quot;duplex&quot;:false,
            &quot;shippingCountry&quot;:&quot;CH&quot;,
            &quot;envelopeSize&quot;:&quot;C5&quot;,
            &quot;postage&quot;:&quot;A_POST&quot;,
            &quot;windowLocation&quot;:&quot;LEFT&quot;,
            &quot;numberOfPage&quot;:2,
            &quot;qrInvoicePageNumber&quot;:2
         },
         &quot;documentTypes&quot;:[
            &quot;invoice&quot;
         ],
         &quot;origin&quot;:&quot;SmartSend&quot;,
         &quot;recipients&quot;:[
            {
               &quot;senderUserId&quot;:&quot;93ec5b2b-305b-459d-93e5-88a898549636&quot;,
               &quot;unhashedCredentials&quot;:{
                  &quot;email&quot;:&quot;sebastian.metzgeasdfr@klara.ch&quot;
               }
            }
         ]
      }
   ],
   &quot;featurePricePlan&quot;:&quot;smartsend&quot;
}</code></pre>
</div>
</div></td>
<td><p>Digital channel is appeared</p>
<p>Digital channel price is showed</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d6f3ba7a-db83-47de-83ba-527b41b668ba" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;status&quot;: &quot;FINISHED&quot;,
    &quot;pricePreview&quot;: [
        {
            &quot;senderUserId&quot;: &quot;93ec5b2b-305b-459d-93e5-88a898549636&quot;,
            &quot;fileName&quot;: &quot;document.pdf&quot;,
            &quot;potentialPrices&quot;: [
                {
                    &quot;channel&quot;: &quot;DIGITAL&quot;,
                    &quot;price&quot;: 0.37
                }
            ]
        }
    ]
}</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9a12a887-0ed3-429a-b50e-6405daf6d483" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;uuid&quot;: &quot;6df5e476-1229-4a56-a497-e277eea23e82&quot;,
  &quot;createdTime&quot;: &quot;2025-10-20T10:48:10Z&quot;,
  &quot;code&quot;: null,
  &quot;message&quot;: &quot;Cannot invoke \&quot;String.equals(Object)\&quot; because the return value of \&quot;ch.klara.luz.eletter.model.oneapi.preview.PreviewResultDetail.getParticipantId()\&quot; is null&quot;,
  &quot;detail&quot;: null
}</code></pre>
</div>
</div></td>
<td><p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48773038081_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-142523" data-macro-id="64591901-cef9-4497-a21b-5f280cbb4593" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-142523" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-142523</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>
