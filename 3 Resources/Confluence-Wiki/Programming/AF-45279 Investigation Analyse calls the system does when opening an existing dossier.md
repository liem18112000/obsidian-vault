---
ai_hash: 328f5565a790651b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.44
entities: []
relevance: 0.736
source: https://axonivy.atlassian.net/wiki/spaces/RT/pages/26423849052/AF-45279+Investigation+Analyse+calls+the+system+does+when+opening+an+existing+dossier
space: RT
status: reference
tags:
- confluence
- programming
- space/rt
title: AF-45279 [Investigation] Analyse calls the system does when opening an existing
  dossier
topic: programming
type: source
updated: 2020-04-10
---

# AF-45279 [Investigation] Analyse calls the system does when opening an existing dossier

> [!info] Imported from Confluence
> Space **RT** · updated 2020-04-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/RT/pages/26423849052/AF-45279+Investigation+Analyse+calls+the+system+does+when+opening+an+existing+dossier)
> Relevance 0.736 · topic `programming`

**<span style="font-size: 24.0px;letter-spacing: -0.01em;">Calls that being excuted in phase <span class="legacy-color-text-blue3">"in execution", "completed" or "refused"</span></span>**

<div>

<table style="width: 83.4783%;">
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
<th>No</th>
<th>URL</th>
<th>Operation</th>
<th>Reason</th>
<th>Remark</th>
<th>Effort to Refactor</th>
</tr>
&#10;<tr>
<td>1</td>
<td><span class="legacy-color-text-blue3">/v1/banks/{number}/clients/{key}/totalExposure</span></td>
<td>Existing Business</td>
<td>Initialize datas for ViewBean of ShortCheck component</td>
<td><p>Not good code handling from Samurai, this call should be only executed if the BusinessType == Commercial Financing</p>
<p>This call is only restore datas for view bean, therefore, it won't impact to the data Model</p></td>
<td><p>May not removable, the call could be reduce if check the dossier's Business Type. </p>
<p><br />
</p></td>
</tr>
<tr>
<td>2</td>
<td><span class="legacy-color-text-blue3">/v3/banks/{userbank}/orders/{orderKey}</span></td>
<td>Existing Business</td>
<td>Initialize datas for ViewBean of ShortCheck component</td>
<td><p>Not good code handling from Samurai, this call should be only executed if the BusinessType == Commercial Financing</p>
<p>This call is only restore datas for view bean, therefore, it won't impact to the data Model</p></td>
<td>May not removable, the call could be reduce if check the dossier's Business Type. </td>
</tr>
<tr>
<td>3</td>
<td>v3/banks/{userbank}/codes/PropertyTypes</td>
<td>New Busines</td>
<td>Get data for property type</td>
<td>This call is already imported to static data, could be query from ES instead of recall Finnova.</td>
<td>Small, could be replace easily</td>
</tr>
<tr>
<td>4</td>
<td>v3/banks/{userbank}/codes/PropertyTypesOfUse/</td>
<td>New Business</td>
<td>Get data for property type of use</td>
<td>This call is already imported to static data, could be query from ES instead of recall Finnova.</td>
<td>Small, could be replace easily</td>
</tr>
<tr>
<td>5</td>
<td>v2/banks/{userbank}/loanAdvisories/collateralTypes/{collateralType}/collateralSubtypes</td>
<td>New Business</td>
<td>Get collateralSubType for dossier</td>
<td><p>This call will be executed for the first load of everytime we opening an old dossier. </p>
<p>This dossier must has Property Type is one of these type (RESIDENTIAL_FLAT/SINGLE_FAMILY_HOME/AGRICULTURE)</p>
<p>and has done the base value calculation (base value has data)</p></td>
<td><p>Medium - Big effort.  </p></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Open API related to Finnova microservices - Create dossier]]
- [[Analytics Analyze API call when accessing eArchive]]
- [[Estimate for ivy and cob-unattended-business-dossier-service-api-spec]]
- [[List out places calling booking function]]
- [[Changes to SOB endpoints to align the response status code (FA-6800)]]

%% ai-graph-end %%