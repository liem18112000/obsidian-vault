---
ai_hash: cfaeba6dbb73a178
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.65
entities: []
relevance: 0.738
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47106262118/God+class+in+luz_components
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: God class in luz_components
topic: programming
type: source
updated: 2022-05-10
---

# God class in luz_components

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-05-10 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47106262118/God+class+in+luz_components)
> Relevance 0.738 · topic `programming`

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
<th><p><strong>Class</strong></p></th>
<th><p><strong>Type</strong></p></th>
<th><p><strong>Status</strong></p></th>
<th><p><strong>Reason</strong></p></th>
<th><p><strong>Idea/solution</strong></p></th>
</tr>
&#10;<tr>
<td><p>RedirectionOrder</p></td>
<td><p>model</p></td>
<td><p>Around 400 lines of code with only getter and setter → <span>is it really a god class???</span></p></td>
<td><p>Contains many attributes ̣(38 attributes)</p></td>
<td><p>Introduce sub type to hold info Eg. Person for attributes of person name, first name, language, etc.</p></td>
</tr>
<tr>
<td><p>CreditCardInformation</p></td>
<td><p>model</p></td>
<td><p>Around 270 lines of code → NOT a god class</p></td>
<td></td>
<td><p>One minor thing can be refactor is to move the isKlaraSwissBankerCard to Utils</p></td>
</tr>
<tr>
<td><p>OpenedPositionDisplay</p></td>
<td><p>model</p></td>
<td><p>Around 310 lines of code of itself and around 220 line of code of its parent</p></td>
<td></td>
<td><ul>
<li><p>It should only contains attributes and getters/setter. The logic should move to Utils class E.g. recalculateVatAmount</p>
<ul>
<li><p>Group attributes?</p></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td><p>Vat</p></td>
<td><p>model</p></td>
<td><ul>
<li><p>Only 13 attributes</p></li>
<li><p>305 lines of code</p></li>
</ul></td>
<td><ul>
<li><p>There is a builder inside</p></li>
<li><p>hashCode and equal is long → <span>cannot be changed</span></p></li>
</ul></td>
<td><p>Move out the builder</p></td>
</tr>
<tr>
<td><p>Address</p></td>
<td><p>model</p></td>
<td><p>There are several Address classes. The biggest one has around 240 lines of code</p></td>
<td><p>Long hashCode and equalMethod</p></td>
<td><p>Nothing to improve</p></td>
</tr>
<tr>
<td><p>EpostComponentViewHandler</p></td>
<td><p>Handler</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>SalaryStatementConfig</p></td>
<td><p><span>TODO</span></p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Contract</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>SalaryItemBasicInfo</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>EmployeeInformation</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>SellableArticle</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>InstitutionSalaryDeclaration</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>TaxAtSourceController2021</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>CompanyHouseHoldRegistrationInsuranceController</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>SalaryPaymentController</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>EpostComponentViewHandler</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>AuthenticationLetterService</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>CompanyWorkplaceController</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>BankConnectionHandler</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>EpostComponentViewHandler</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>TaxAtSourceBean</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>AddressInformationBean</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>BankConnetionSetUpManagedBean</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>TaxAtSourceChangeDetector</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>DateUtil</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Are the “technologies” (process files, java code, …) used correctly and efficiently]]
- [[Merging process]]
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[Impact of code changes on common components]]
- [[Research Design architecture concept for the service to generate the Generic Interface File]]

%% ai-graph-end %%