---
title: "How to implement a feature hint for eArchive (reuse new common component )"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47237169363/How+to+implement+a+feature+hint+for+eArchive+reuse+new+common+component
space: "TP2020"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2022-12-15
attachments: 2
tags:
  - confluence
  - programming
  - space/tp2020
---

# How to implement a feature hint for eArchive (reuse new common component )

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-12-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47237169363/How+to+implement+a+feature+hint+for+eArchive+reuse+new+common+component)
> Relevance 0.738 · topic `programming`

Reference story: <a href="https://axonivy.atlassian.net/browse/LUZ-89751" class="external-link" rel="nofollow">[LUZ-89751] show feature hint for eArchive (reuse new common component) - Jira (atlassian.net)</a>

## 1. Discussion

<div id="expander-528314194" class="expand-container conf-macro output-block" hasbody="true" macro-id="1fc9ec79-afe7-42b0-a9e5-c19989806a8a" macro-name="expand">

<div id="expander-control-528314194" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Discussion steps</span>

</div>

<div id="expander-content-528314194" class="expand-content expand-hidden">

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
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Need to do</strong></p></th>
<th><p><strong>Who does it?</strong></p></th>
<th><p><strong>Times</strong></p></th>
</tr>
&#10;<tr>
<td colspan="5"><p><strong>Day 1: 07/12/2022 - 3:00 PM</strong></p></td>
</tr>
<tr>
<td><p>Read the common component code</p></td>
<td><p>We get the source code of the common component to read the code to know:</p>
<ol>
<li><p>What is that component?</p></li>
<li><p>How can we show the feature hint as a pop-up</p></li>
<li><p>How many params/attributes need to binding the data?</p></li>
</ol></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>50 minute</p></td>
</tr>
<tr>
<td><p>Ask Muji team about the unclear points</p></td>
<td><p>Some params/attributes of the common component we are not sure about the behavior so that is the reason why we ask Muji to make sure we using correctly</p></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>10 minute</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

## 2. Investigate and define test cases

<div id="expander-110804013" class="expand-container conf-macro output-block" hasbody="true" macro-id="f5fe4b0f-7551-4d6d-a549-f08e376c8c43" macro-name="expand">

<div id="expander-control-110804013" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Investigate steps</span>

</div>

<div id="expander-content-110804013" class="expand-content expand-hidden">

<div>

<table>
<tbody>
<tr>
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Need to do</strong></p></th>
<th><p><strong>Who does it?</strong></p></th>
<th><p><strong>Times</strong></p></th>
</tr>
&#10;<tr>
<td colspan="5"><p><strong>Day 1: 07/12/2022 - 4:00 PM</strong></p></td>
</tr>
<tr>
<td><p>Read the existing PR use that common component</p></td>
<td><p>We read another PR use the common component to get understand more</p></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>20 minute</p></td>
</tr>
<tr>
<td><p>Define test cases</p></td>
<td><p>We need to define the business cases to test that behavior when we finish implement to make sure we implement correctly</p></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

## 3. Implement

<div id="expander-686055132" class="expand-container conf-macro output-block" hasbody="true" macro-id="0e32a2cd-5d1f-40b4-bed6-4ed1c9be0de6" macro-name="expand">

<div id="expander-control-686055132" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Implement steps</span>

</div>

<div id="expander-content-686055132" class="expand-content expand-hidden">

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
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Need to do</strong></p></th>
<th><p><strong>Who does it?</strong></p></th>
<th><p><strong>Times</strong></p></th>
</tr>
&#10;<tr>
<td colspan="5"><p><strong>Day 2: 08/12/2022 - 9:00 AM</strong></p></td>
</tr>
<tr>
<td><p>Remove the old logic for show the feature hint for eArchive</p></td>
<td><p>In the pase, we were used the logic of the feature advertisement to show the feature hint for eArchive. Now, we need to use the common component to do that, so the old logic is not correct anomore.</p></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>2 hour</p></td>
</tr>
<tr>
<td><p>Use the common component to show feature hint for eArchive</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>4 hour</p></td>
</tr>
<tr>
<td><p>Build style on the local</p></td>
<td><p>When we change the style code we need to build that style again to get the new implement to make sure correct implementation</p>
<p>Note: The new code of some module has the diferrent version so I need to get newest code of all neccessary modules. Then clean build and input into my workspace again to make my local work</p></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
<tr>
<td><p>Call with Muji to inform the case that the common component don’t support</p></td>
<td><p>The common component don’t have the params to input the value for the title and hyperlink also</p>

![[47237169363-image-20221209-102847.png]]

</td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>30 minute</p></td>
</tr>
<tr>
<td><p>Muji adjust the logic to improve the common component</p></td>
<td><p>That common component need to be enhanced to support our eArchive feature hint.</p></td>
<td></td>
<td><p>Muji</p></td>
<td><p>1 hour</p></td>
</tr>
<tr>
<td colspan="5"><p><strong>Day 3: 09/12/2022 - 9:30 AM</strong></p></td>
</tr>
<tr>
<td><p>Muji continue adapt the common component</p></td>
<td>

![[47237169363-image-20221209-102847.png]]

</td>
<td></td>
<td><p>Muji</p></td>
<td><p>3 hour</p></td>
</tr>
<tr>
<td><p>Checkout branchs form Muji to use the enhancement common component</p></td>
<td><p>Because during the time Muji enhance the common component we still need to continue implement our logic. So we need to checkout that enhancement branchs to continue from our side</p></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>10 minute</p></td>
</tr>
<tr>
<td><p>Build style on the local</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Implement to create the first PR for use the common component for the eArchive feature hint</p></td>
<td><p>2 PRs:</p>
<ol>
<li><p><a href="https://bitbucket.org/axonivy-prod/luz_components/pull-requests/8473" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_components/pull-requests/8473</a></p></li>
<li><p><a href="https://bitbucket.org/axonivy-prod/luz_web/pull-requests/1790" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_web/pull-requests/1790</a></p></li>
</ol></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>3 hour 30 minute</p></td>
</tr>
<tr>
<td><p>Adapt comment the first PR</p></td>
<td><ol>
<li><p>Rename the logic to make that component name correctly</p></li>
<li><p>Do we need to care the 2 new eArchive widgets when check to show that feature hint and also create the task → ask PO</p></li>
</ol></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>30 minute</p></td>
</tr>
<tr>
<td><p>Implement the logic for</p>
<ol>
<li><p>Show eArchive feature hint</p></li>
<li><p>Redirect to eArchive widget detail page</p></li>
</ol></td>
<td><p>As you know we have 3 eArchive widgets for now: eArchive version 2 (current), eArchive BASIC and eArchive PLUS</p>
<p>So we need to adapt the behavior show eArchive feature hint base on the feature switch <em><strong>klara:widgetStore:newPricingModel</strong></em> that:</p>
<ol>
<li><p>if <em><strong>klara:widgetStore:newPricingModel</strong></em> is TURN ON</p>
<ol>
<li><p>SHOW the feature hint when has NO subscriptions for the eArchive BASIC and eArchive PLUS widgets yet</p></li>
<li><p>Click "Check later": Create the TODO task for eArchive PLUS widget</p></li>
<li><p>Click "Check it now": redirect to eArchive PLUS widget detail page</p></li>
</ol></li>
<li><p>if <em><strong>klara:widgetStore:newPricingModel</strong></em> is TURN OFF</p>
<ol>
<li><p>SHOW the feature hint when has NO subscriptions for the eArchive version 2 (current) widget yet</p></li>
<li><p>Click "Check later": Create the TODO task for eArchive version 2 (current) widget</p></li>
<li><p>Click "Check it now": redirect to eArchive version 2 (current) widget detail page</p></li>
</ol></li>
</ol>
<p>Biside that, we need to handle the case redirect to eArchive widget detail page</p></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
<tr>
<td colspan="5"><p><strong>Day 4: 12/12/2022 - 9:30 AM</strong></p></td>
</tr>
<tr>
<td><p>Implement business logic</p></td>
<td><ul>
<li><p>Handle logic when we show the eArchive feature hint</p></li>
<li><p>Handle logic which is the widget detail page when the user click “ check it now”</p></li>
<li><p>Handle logic update TODO list when the user click “check later”</p></li>
<li><p>Handle logic open eArchive feature hint when opening the dashboard</p></li>
</ul></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>2 hour 45 minute</p></td>
</tr>
<tr>
<td><p>Investigate why the common component cannot update TODO list after the user click “check later”</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Investigate why the common component cannot show the eArchive feature hint on dashboard</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
<tr>
<td><p>Deploy code into the DEV-VN env to test</p></td>
<td><p>3 times</p></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Discuss with MUJI team about the case update TODO list after the user click “check later”</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Discuss wiht MUJI team about the case open eArchive feature hint when opening the dashboard</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Investigate why still show the eArchive feature hint when that company already has TODO task</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td colspan="5"><p><strong>Day 5: 13/12/2022 - 9:30 AM</strong></p></td>
</tr>
<tr>
<td><p>Implement unit test</p></td>
<td><p>I need to write the unit test to cover our business</p></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
<tr>
<td><p>Discuss with Muji about the task processing</p></td>
<td><p>Check why the user click “check later” and go to dashboard again the eArchive feature hint still show</p></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Adjust the business condition logic</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
<tr>
<td><p>Deploy and test on the DEV-VN server</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Create PR and adapt the comment</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Deploy and test on the DEV-VN server</p></td>
<td></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Update test case</p></td>
<td><p>I need to update the test case base on the new information of the eArchive feature hint</p></td>
<td></td>
<td><p>Pioneer</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Discuss with WOW and NEXT teams to ask the help to support MUJI team</p></td>
<td><p>MUJI need the support for the behavior click on TODO task and open the pop-up again</p></td>
<td></td>
<td><p>Pioneer discuss</p></td>
<td><p>30 minute</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

## 4. Crosstest

<div id="expander-2137383041" class="expand-container conf-macro output-block" hasbody="true" macro-id="f7c91e7e-800f-44e1-afcd-4f5c045e76d6" macro-name="expand">

<div id="expander-control-2137383041" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-2137383041" class="expand-content expand-hidden">

<div>

<table>
<tbody>
<tr>
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Need to do</strong></p></th>
<th><p><strong>Who does it?</strong></p></th>
<th><p><strong>Times</strong></p></th>
</tr>
&#10;<tr>
<td colspan="5"><p><strong>Day 5: 13/12/2022 - 9:30 AM</strong></p></td>
</tr>
<tr>
<td><p>Crosstest</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>Pioneer</p></td>
<td><p>1 hour</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

## 5. Investigate should apply cached for business condition

<div id="expander-650155522" class="expand-container conf-macro output-block" hasbody="true" macro-id="c59ecafb-2d36-47c3-ba32-2d1b8c551c00" macro-name="expand">

<div id="expander-control-650155522" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-650155522" class="expand-content expand-hidden">

<div>

<table>
<tbody>
<tr>
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Standard steps</strong></p></th>
<th><p><strong>Who does it?</strong></p></th>
<th><p><strong>Times</strong></p></th>
</tr>
&#10;<tr>
<td colspan="5"><p><strong>Day 6: 14/12/2022 - 9:30 AM</strong></p></td>
</tr>
<tr>
<td><p>Get the API call from our business condition</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>DEV team</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Get the LOG for this API from the PROD</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>DEV team</p></td>
<td><p>15 minute</p></td>
</tr>
<tr>
<td><p>Calculate the average time-consuming on the PROD for that API call</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>DEV team</p></td>
<td><p>30 minute</p></td>
</tr>
<tr>
<td><p>Discuss with PO and Daniel to make the decision should apply the cached for our busineed condition of not</p></td>
<td></td>
<td><p>

![[47237169363-check.png]]

</p></td>
<td><p>DEV team</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>
