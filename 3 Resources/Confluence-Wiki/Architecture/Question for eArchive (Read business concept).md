---
ai_hash: 6188b2a57e67cb8a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 31
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47087422576/Question+for+eArchive+Read+business+concept
space: TP2020
status: reference
tags:
- confluence
- architecture
- space/tp2020
title: Question for eArchive (Read business concept)
topic: architecture
type: source
updated: 2022-04-14
---

# Question for eArchive (Read business concept)

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-04-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47087422576/Question+for+eArchive+Read+business+concept)
> Relevance 0.711 · topic `architecture`

After team Pioneer read the summary for eArchive( <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_47087422576_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-70814" macro-id="ce02fb1b-4923-454a-b327-d2d8e0be7d75" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-70814" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-70814</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> ) , we have some questions about eArchive :

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
<th></th>
<th><p><br />
<strong>Question</strong></p></th>
<th><p><strong>Discussion</strong></p></th>
<th><p><strong>For</strong></p></th>
<th><p><strong>answered</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>About the point in this image. We want to confirm that the current behavior support for all user (admin and non-admin) can subscribe the widget in Klara.</p>
<p>So this point doesn’t correct with current behavior. Please help us check it</p>

![[47087422576-image-20220406-030914.png]]

</td>
<td><p> Thank you, this was an old statement which I now removed. (I already removed in one chapter but it seems I forgot to remove here to)</p>
<p><strong><span> It is correct how we implemented now.</span></strong></p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>2</td>
<td>

![[47087422576-image-20220406-031318.png]]


<p>We think the application will generate the branded folder with the company name is better. What do you think?</p></td>
<td><p> I take this up and discuss within the business squad. I think Renato likes more to show HIS company brand there 

![[47087422576-wink.png]]

 Also the brand normally must be payed by companies-must clarify</p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>3</td>
<td>

![[47087422576-image-20220406-031837.png]]


<p>When the user subscribe Business Relax, the eArchive widget automatically subscribe. the status of eArchive widget is <code>running</code> or <code>grace period</code> in this case?</p></td>
<td><p> <code>running</code></p>
<p>same as letterbox is subscribed when user subscribes to scanning first.</p>
<p>Or maybe I do not get your question right?</p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>4</td>
<td>

![[47087422576-image-20220406-032055.png]]


<p>Is current plan we are doing?</p></td>
<td><p> yep. we must gi live with sellable solution as soon as possible to finally make some convertion rate.</p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>5</td>
<td><p>Variant 2 just for admin user. Is it correct?</p>
<p>Variant 1 for both non-admin and admin users?</p></td>
<td><p> Variant 1 for both</p>
<p>Variant 2 for both except adds (access management) are for admin only</p>

![[47087422576-image-20220407-154207.png]]

</td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>6</td>
<td>

![[47087422576-image-20220406-033918.png]]


<p>Why we need to remove signature stamps?</p></td>
<td><p>  yeah right, don’t remove. I updated the concept. Keep as per todays implementation.</p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>7</td>
<td>

![[47087422576-image-20220406-034129.png]]


<p>What is ePost custom content?</p></td>
<td><p> it is the cutom folders (and maybe also uploads in tab 1 (root) (tbd))</p>

![[47087422576-image-20220407-160002.png]]

</td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>8</td>
<td>

![[47087422576-image-20220406-034823.png]]


<p>What happens if users re-subscribe widget during not subscribe period?</p>
<p><em>What is the "trigger deletion of custom content"?</em></p></td>
<td><p> one day after grace period end (on day 31 after user clicked on unsubscribe) we must trigger the deletion of eArchive content. user still has view access on content 30 more days. on day 61 we delete the content. <strong></strong> <a href="https://axonivy.atlassian.net/wiki/people/600955fbdfb0c700693545cb?ref=confluence" class="confluence-userlink user-mention" data-account-id="600955fbdfb0c700693545cb" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Alessio Manzo (Unlicensed)</a> <strong>What happens if user resubscribes again between day 31 til 60? can you stop the deletion request?</strong></p>
<p><strong><u>Alessio wrote:</u></strong> the user <strong>should not be able</strong> to resubscribe <span class="inline-comment-marker" data-ref="3587eeb1-1374-405c-a922-19c328be065a">after the grace period. </span>all documents will be deleted and only be available technically. once there is a trash folder (like in mobile app) the documents <span class="inline-comment-marker" data-ref="60e54c99-4142-4810-bc33-32acba91e04a">will be shown in the trash folder.</span></p>
<p>Based on discussion on 13 Apr 2022 with <a href="https://axonivy.atlassian.net/wiki/people/600955fbdfb0c700693545cb?ref=confluence" class="confluence-userlink user-mention" data-account-id="600955fbdfb0c700693545cb" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Alessio Manzo (Unlicensed)</a>. (solution independently on if in WEB we have a trashfolder or not)</p>

![[47087422576-image-20220414-113546.png]]

</td>
<td><p>Tiziana</p>
<p>&amp;</p>
<p> <a href="https://axonivy.atlassian.net/wiki/people/600955fbdfb0c700693545cb?ref=confluence" class="confluence-userlink user-mention" data-account-id="600955fbdfb0c700693545cb" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Alessio Manzo (Unlicensed)</a></p></td>
<td><p>

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>9</td>
<td>

![[47087422576-image-20220406-035007.png]]


<p>In the grace period time we disable all affected actions. However, We re-enable all affected actions when the user unsubscrbe eArchive widget more than 60 days. Why do we need to do that?</p></td>
<td><p>Because we set back to status Not Subscribed. See table <a href="https://axonactivegroup-my.sharepoint.com/:x:/g/personal/tiziana_gullo_klara_ch/EVSjpjZVfJVCogEd9BLIdZ8Bcw7ZVrHhbHxBvAaTcSW3vQ?e=4EAYvp" class="external-link" rel="nofollow">Dependencies on subscriptions 1.0.xlsx</a></p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>10</td>
<td>

![[47087422576-image-20220406-035209.png]]


<p>What is the custom content we deletion on the M+1+30 days?</p>
<p>What is the eArchive custom content we remove on the M+1+60 days?</p></td>
<td><p> same same..just different wording.</p>
<p>I adjusted the wording in the diagram.</p></td>
<td><p> Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>11</td>
<td>

![[47087422576-image-20220406-035642.png]]


<p>We deactive the export button before trigger send notification mail to make the user know that can export the content. However, the export button already deactive so the user cannot do the export flow when get the email notification.</p>
<p>We think we don’t need to deactive export button after 30 days the user unsubscrbe eArchive widget.</p></td>
<td><p> yep you are right of course 

![[47087422576-smile.png]]

</p>
<p>NEVER disable this export button.</p>
<p><a href="https://axonivy.atlassian.net/wiki/people/600955050d83ff00766e2a56?ref=confluence" class="confluence-userlink user-mention" data-account-id="600955050d83ff00766e2a56" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Linda Rudolf von Rohr (Unlicensed)</a> in UI this export button should not be shown prominent. It should not easily be found..</p>
<p><a href="https://axonivy.atlassian.net/wiki/people/5aa6202a662ca62622f27472?ref=confluence" class="confluence-userlink user-mention" data-account-id="5aa6202a662ca62622f27472" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">[Gravity] Nhan Nguyen</a> we need to avoid that users can produce endless amount of exports… can we implement to allowe users to export 1 time per day only? And: the export file must be overwritten if user clicks on exports again. <strong>Please update the export PoC with here stated outcome</strong></p></td>
<td><p>Tiziana</p></td>
<td><p> 

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>12</td>
<td>

![[47087422576-image-20220406-035858.png]]


<p>Do we need to understand it?</p></td>
<td><p>Yes, don’t you? LOL I will translate 

![[47087422576-smile.png]]

</p></td>
<td><p>Tiziana</p></td>
<td><p>

![[47087422576-check.png]]

</p></td>
</tr>
<tr>
<td>13</td>
<td><p>which content exactly would you delete after user unsubscribed eArchive: custom folders and documents stored in root level (tab 1 in business web) only?</p>
<p>remember: when user did <strong>not</strong> subscribe to eArchive, user can see following content in eArchive:</p>
<ul>
<li><p>branded folders (if letterbox is been subscribed once and there are branded letters in int)</p></li>
<li><p>KLARA Business folder with internal generated documents</p></li>
</ul></td>
<td><p>My suggestion is to delete <span class="inline-comment-marker" data-ref="51519c05-236b-4aa5-a205-91864e302390">everything that is archived</span>, meaning if there is a folderID in the metadata of the document. plus I would delete all the custom folders</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/people/600955fbdfb0c700693545cb?ref=confluence" class="confluence-userlink user-mention" data-account-id="600955fbdfb0c700693545cb" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Alessio Manzo (Unlicensed)</a></p></td>
<td><p>

![[47087422576-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Business concept for frontend]]
- [[Analytics Analyze API call when accessing eArchive]]
- [[Proposal eArchived architecture direction for ePost web 2]]
- [[CROSS-TEST LUZ-159442 Implement real ZIP download for eArchive folders]]
- [[Research Protect ePost inbox with new Digital_Letterbox permission]]

%% ai-graph-end %%