---
ai_hash: a2ccdcc68890c47c
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.835
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519729852/Analyze+performance+for+REST+API+calculate+payslips+for+overview+salary+processing
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Analyze performance for REST API calculate payslips for overview salary processing
topic: programming
type: source
updated: 2021-03-01
---

# Analyze performance for REST API calculate payslips for overview salary processing

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-03-01 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519729852/Analyze+performance+for+REST+API+calculate+payslips+for+overview+salary+processing)
> Relevance 0.835 · topic `programming`

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Ticket</th>
<th><div class="content-wrapper">
<p><span class="confluence-jim-macro jira-issue conf-macro output-block" data-client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20519729852_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" data-hasbody="false" data-jira-key="LUZ-48599" data-macro-id="08e38a71-4643-44b7-b2cf-2caf77b05949" data-macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-48599" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-48599</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span></p>
</div></th>
</tr>
&#10;<tr>
<td>Team</td>
<td><strong>WOW</strong></td>
</tr>
</tbody>
</table>

</div>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="a4cc0b5f-07d0-4f12-82bd-f062cd401001" macro-name="toc">

</div>

# **Overview of issue**

<span class="legacy-color-text-blue3">During calculate payslips for salary processing overview, there might have some performance issues.</span>

# **<span class="legacy-color-text-blue3">Service call</span>**

Refer to <a href="https://axonivy.atlassian.net/wiki/pages/viewpage.action?pageId=20519728473" rel="nofollow">https://axonivy.atlassian.net/wiki/pages/viewpage.action?pageId=20519728473</a>

# **Access logs**

Refer to <a href="https://axonivy.atlassian.net/wiki/pages/viewpage.action?pageId=20519728473" rel="nofollow">https://axonivy.atlassian.net/wiki/pages/viewpage.action?pageId=20519728473</a>

# **Process steps**

****<span class="legacy-color-text-blue4">The environment having 15 employees.</span>****

**Execution time from beginning until the client receive the data: ~7449**

Following steps are execute:

<div>

<table style="width: 80.268%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th>Step</th>
<th>Execution time(ms)</th>
<th>Behavior</th>
<th>Notes</th>
<th>Proposal</th>
</tr>
&#10;<tr>
<td rowspan="2">Find all contracts and related information</td>
<td rowspan="2">~152</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="bf6a17df-0d4a-4723-aabd-eed5d3f4dfb9" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Query get contracts</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Midnight; collapse: true" data-theme="Midnight"><code>     select
         distinct contracten0_.id as id1_35_0_,
         employeeen3_.id as id1_48_1_,
         contractte6_.id as id1_39_2_,
         contracten0_.create_by as create_b2_35_0_,
         contracten0_.create_date as create_d3_35_0_,
         contracten0_.update_by as update_b4_35_0_,
         contracten0_.update_date as update_d5_35_0_,
         contracten0_.contract_type as contract6_35_0_,
         contracten0_.education as educatio7_35_0_,
         contracten0_.employee_id as employe23_35_0_,
         contracten0_.employment_contract as employme8_35_0_,
         contracten0_.entry_date as entry_da9_35_0_,
         contracten0_.exit_date as exit_da10_35_0_,
         contracten0_.first_payment_date as first_p11_35_0_,
         contracten0_.first_working_day as first_w12_35_0_,
         contracten0_.holidays as holiday13_35_0_,
         contracten0_.holidays_weeks as holiday14_35_0_,
         contracten0_.informal_entry_date as informa15_35_0_,
         contracten0_.job_title as job_tit16_35_0_,
         contracten0_.last_working_day as last_wo17_35_0_,
         contracten0_.planned_work_hours as planned18_35_0_,
         contracten0_.planned_work_weeks as planned19_35_0_,
         contracten0_.position as positio20_35_0_,
         contracten0_.weekly_hours as weekly_21_35_0_,
         contracten0_.weekly_lessons as weekly_22_35_0_,
         employeeen3_.create_by as create_b2_48_1_,
         employeeen3_.create_date as create_d3_48_1_,
         employeeen3_.update_by as update_b4_48_1_,
         employeeen3_.update_date as update_d5_48_1_,
         employeeen3_.avatar_uri as avatar_u6_48_1_,
         employeeen3_.person_uri as person_u7_48_1_,
         employeeen3_.abbreviation as abbrevia8_48_1_,
         employeeen3_.company as company13_48_1_,
         employeeen3_.email_for_payslip as email_fo9_48_1_,
         employeeen3_.emergency_address as emergen10_48_1_,
         employeeen3_.employee_number as employe11_48_1_,
         employeeen3_.payment as payment14_48_1_,
         employeeen3_.is_payslip_sent_by_email as is_pays12_48_1_,
         contractte6_.create_by as create_b2_39_2_,
         contractte6_.create_date as create_d3_39_2_,
         contractte6_.update_by as update_b4_39_2_,
         contractte6_.update_date as update_d5_39_2_,
         contractte6_.valid_from as valid_fr6_39_2_,
         contractte6_.valid_to as valid_to7_39_2_,
         contractte6_.bvg_deduction as bvg_dedu8_39_2_,
         contractte6_.bvg_deduction_of_employer as bvg_dedu9_39_2_,
         contractte6_.contract_id as contrac21_39_2_,
         contractte6_.cost_center as cost_ce10_39_2_,
         contractte6_.employment_type as employm11_39_2_,
         contractte6_.engagement_level as engagem12_39_2_,
         contractte6_.function_title_id as functio22_39_2_,
         contractte6_.granted_rate as granted13_39_2_,
         contractte6_.hourly_salary_rate as hourly_14_39_2_,
         contractte6_.job_activity as job_act15_39_2_,
         contractte6_.manual_tax_rate as manual_16_39_2_,
         contractte6_.monthly_salary as monthly17_39_2_,
         contractte6_.pension_fund_id as pension23_39_2_,
         contractte6_.single_parent as single_18_39_2_,
         contractte6_.special_tax_city_uri as special19_39_2_,
         contractte6_.tariff_code as tariff_20_39_2_,
         contractte6_.tax_at_source_id as tax_at_24_39_2_,
         contractte6_.workplace_id as workpla25_39_2_,
         contractte6_.contract_id as contrac21_39_0__,
         contractte6_.id as id1_39_0__ 
     from
         contract contracten0_ 
     inner join
         employee employeeen1_ 
             on contracten0_.employee_id=employeeen1_.id 
     inner join
         company companyent2_ 
             on employeeen1_.company=companyent2_.id 
     inner join
         employee employeeen3_ 
             on contracten0_.employee_id=employeeen3_.id 
     left outer join
         contract_temporal_info contractte6_ 
             on contracten0_.id=contractte6_.contract_id 
     where
         contracten0_.entry_date=(
             select
                 max(contracten4_.entry_date) 
             from
                 contract contracten4_ 
             inner join
                 employee employeeen5_ 
                     on contracten4_.employee_id=employeeen5_.id 
             where
                 contracten0_.employee_id=contracten4_.employee_id
         ) 
         and companyent2_.id=1 
     order by
         contracten0_.id asc</code></pre>
</div>
</div>
</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>For each contract N+1 queries are triggered:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="06a88739-4edd-4845-8a7e-5516d845bb38" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get company</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         companyent0_.id as id1_32_0_,
         companyent0_.create_by as create_b2_32_0_,
         companyent0_.create_date as create_d3_32_0_,
         companyent0_.update_by as update_b4_32_0_,
         companyent0_.update_date as update_d5_32_0_,
         companyent0_.company_uri as company_6_32_0_,
         companyent0_.description as descript7_32_0_,
         companyent0_.has_vat_id as has_vat_8_32_0_,
         companyent0_.image_file_id as image_fi9_32_0_,
         companyent0_.language as languag10_32_0_,
         companyent0_.logo_fileid as logo_fi11_32_0_,
         companyent0_.logo_position as logo_po12_32_0_,
         companyent0_.representative_id as represe16_32_0_,
         companyent0_.sector_name as sector_13_32_0_,
         companyent0_.simplified_tax_at_source as simplif14_32_0_
         companyent0_.wage_agreement as wage_ag15_32_0_,
         insurancec1_.company_id as company15_56_1_,
         insurancec1_.id as id1_56_1_,
         insurancec1_.id as id1_56_2_,
         insurancec1_.create_by as create_b2_56_2_,
         insurancec1_.create_date as create_d3_56_2_,
         insurancec1_.update_by as update_b4_56_2_,
         insurancec1_.update_date as update_d5_56_2_,
         insurancec1_.valid_from as valid_fr6_56_2_,
         insurancec1_.valid_to as valid_to7_56_2_,
         insurancec1_.additional_info as addition8_56_2_,
         insurancec1_.business_case as business9_56_2_,
         insurancec1_.company_id as company15_56_2_,
         insurancec1_.customer_number as custome10_56_2_,
         insurancec1_.global_insurer_id as global_11_56_2_,
         insurancec1_.insurance_policy_number as insuran12_56_2_,
         insurancec1_.insurance_policy_rate as insuran13_56_2_,
         insurancec1_.insurance_short_code as insuran14_56_2_,
         insurancec1_.insurer_id as insurer16_56_2_,
         insurerent2_.id as id1_57_3_,
         insurerent2_.create_by as create_b2_57_3_,
         insurerent2_.create_date as create_d3_57_3_,
         insurerent2_.update_by as update_b4_57_3_,
         insurerent2_.update_date as update_d5_57_3_,
         insurerent2_.canton_uri as canton_u6_57_3_,
         insurerent2_.company_id as company11_57_3_,
         insurerent2_.company_uri as company_7_57_3_,
         insurerent2_.insurer_type as insurer_8_57_3_,
         insurerent2_.insurer_name as insurer_9_57_3_,
         insurerent2_.insurer_number as insurer10_57_3_,
         companyent3_.id as id1_32_4_,
         companyent3_.create_by as create_b2_32_4_,
         companyent3_.create_date as create_d3_32_4_,
         companyent3_.update_by as update_b4_32_4_,
         companyent3_.update_date as update_d5_32_4_,
         companyent3_.company_uri as company_6_32_4_,
         companyent3_.description as descript7_32_4_,
         companyent3_.has_vat_id as has_vat_8_32_4_,
         companyent3_.image_file_id as image_fi9_32_4_,
         companyent3_.language as languag10_32_4_,
         companyent3_.logo_fileid as logo_fi11_32_4_,
         companyent3_.logo_position as logo_po12_32_4_,
         companyent3_.representative_id as represe16_32_4_,
         companyent3_.sector_name as sector_13_32_4_,
         companyent3_.simplified_tax_at_source as simplif14_32_4_
         companyent3_.wage_agreement as wage_ag15_32_4_,
         representa4_.id as id1_74_5_,
         representa4_.create_by as create_b2_74_5_,
         representa4_.create_date as create_d3_74_5_,
         representa4_.update_by as update_b4_74_5_,
         representa4_.update_date as update_d5_74_5_,
         representa4_.address as address6_74_5_,
         representa4_.company as company7_74_5_,
         representa4_.country as country8_74_5_,
         representa4_.email as email9_74_5_,
         representa4_.name as name10_74_5_,
         representa4_.phone as phone11_74_5_,
         representa4_.website as website12_74_5_,
         representa4_.zip_code as zip_cod13_74_5_,
         workplaces5_.company_id as company11_120_6_,
         workplaces5_.id as id1_120_6_,
         workplaces5_.id as id1_120_7_,
         workplaces5_.create_by as create_b2_120_7_,
         workplaces5_.create_date as create_d3_120_7_,
         workplaces5_.update_by as update_b4_120_7_,
         workplaces5_.update_date as update_d5_120_7_,
         workplaces5_.code as code6_120_7_,
         workplaces5_.companyUri as companyU7_120_7_,
         workplaces5_.description as descript8_120_7_,
         workplaces5_.company_id as company11_120_7_,
         workplaces5_.weekly_hours as weekly_h9_120_7_,
         workplaces5_.weekly_lessons as weekly_10_120_7_,
         workplaceq6_.workplace_id as workplac9_121_8_,
         workplaceq6_.id as id1_121_8_,
         workplaceq6_.id as id1_121_9_,
         workplaceq6_.create_by as create_b2_121_9_,
         workplaceq6_.create_date as create_d3_121_9_,
         workplaceq6_.update_by as update_b4_121_9_,
         workplaceq6_.update_date as update_d5_121_9_,
         workplaceq6_.commission_rate as commissi6_121_9_,
         workplaceq6_.qst_id as qst_id7_121_9_,
         workplaceq6_.state_uri as state_ur8_121_9_,
         workplaceq6_.workplace_id as workplac9_121_9_ 
     from
         company companyent0_ 
     left outer join
         insurance_contract insurancec1_ 
             on companyent0_.id=insurancec1_.company_id 
     left outer join
         insurer insurerent2_ 
             on insurancec1_.insurer_id=insurerent2_.id 
     left outer join
         company companyent3_ 
             on insurerent2_.company_id=companyent3_.id 
     left outer join
         representative representa4_ 
             on companyent3_.representative_id=representa4_.id 
     left outer join
         workplace workplaces5_ 
             on companyent3_.id=workplaces5_.company_id 
     left outer join
         workplace_qst workplaceq6_ 
             on workplaces5_.id=workplaceq6_.workplace_id 
     where
         companyent0_.id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="3473415a-f861-49e2-ba9c-a6bb0dc99b2f" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get employee</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         employeepa0_.id as id1_49_0_,
         employeepa0_.create_by as create_b2_49_0_,
         employeepa0_.create_date as create_d3_49_0_,
         employeepa0_.update_by as update_b4_49_0_,
         employeepa0_.update_date as update_d5_49_0_,
         employeepa0_.additional_address as addition6_49_0_,
         employeepa0_.address as address7_49_0_,
         employeepa0_.bank_account_scope as bank_acc8_49_0_,
         employeepa0_.bank_name as bank_nam9_49_0_,
         employeepa0_.bic_code as bic_cod10_49_0_,
         employeepa0_.cash_payment as cash_pa11_49_0_,
         employeepa0_.iban_number as iban_nu12_49_0_,
         employeepa0_.intended_use as intende13_49_0_,
         employeepa0_.owner_name as owner_n14_49_0_,
         employeepa0_.zip_city as zip_cit15_49_0_ 
     from
         employee_payment employeepa0_ 
     where
         employeepa0_.id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="664d224e-efce-497b-820f-129c3f5b3e92" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get workplace</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         workplacee0_.id as id1_120_0_,
         workplacee0_.create_by as create_b2_120_0_,
         workplacee0_.create_date as create_d3_120_0_,
         workplacee0_.update_by as update_b4_120_0_,
         workplacee0_.update_date as update_d5_120_0_,
         workplacee0_.code as code6_120_0_,
         workplacee0_.companyUri as companyU7_120_0_,
         workplacee0_.description as descript8_120_0_,
         workplacee0_.company_id as company11_120_0_,
         workplacee0_.weekly_hours as weekly_h9_120_0_,
         workplacee0_.weekly_lessons as weekly_10_120_0_,
         companyent1_.id as id1_32_1_,
         companyent1_.create_by as create_b2_32_1_,
         companyent1_.create_date as create_d3_32_1_,
         companyent1_.update_by as update_b4_32_1_,
         companyent1_.update_date as update_d5_32_1_,
         companyent1_.company_uri as company_6_32_1_,
         companyent1_.description as descript7_32_1_,
         companyent1_.has_vat_id as has_vat_8_32_1_,
         companyent1_.image_file_id as image_fi9_32_1_,
         companyent1_.language as languag10_32_1_,
         companyent1_.logo_fileid as logo_fi11_32_1_,
         companyent1_.logo_position as logo_po12_32_1_,
         companyent1_.representative_id as represe16_32_1_,
         companyent1_.sector_name as sector_13_32_1_,
         companyent1_.simplified_tax_at_source as simplif14_32_1_,
         companyent1_.wage_agreement as wage_ag15_32_1_,
         insurancec2_.company_id as company15_56_2_,
         insurancec2_.id as id1_56_2_,
         insurancec2_.id as id1_56_3_,
         insurancec2_.create_by as create_b2_56_3_,
         insurancec2_.create_date as create_d3_56_3_,
         insurancec2_.update_by as update_b4_56_3_,
         insurancec2_.update_date as update_d5_56_3_,
         insurancec2_.valid_from as valid_fr6_56_3_,
         insurancec2_.valid_to as valid_to7_56_3_,
         insurancec2_.additional_info as addition8_56_3_,
         insurancec2_.business_case as business9_56_3_,
         insurancec2_.company_id as company15_56_3_,
         insurancec2_.customer_number as custome10_56_3_,
         insurancec2_.global_insurer_id as global_11_56_3_,
         insurancec2_.insurance_policy_number as insuran12_56_3_,
         insurancec2_.insurance_policy_rate as insuran13_56_3_,
         insurancec2_.insurance_short_code as insuran14_56_3_,
         insurancec2_.insurer_id as insurer16_56_3_,
         insurerent3_.id as id1_57_4_,
         insurerent3_.create_by as create_b2_57_4_,
         insurerent3_.create_date as create_d3_57_4_,
         insurerent3_.update_by as update_b4_57_4_,
         insurerent3_.update_date as update_d5_57_4_,
         insurerent3_.canton_uri as canton_u6_57_4_,
         insurerent3_.company_id as company11_57_4_,
         insurerent3_.company_uri as company_7_57_4_,
         insurerent3_.insurer_type as insurer_8_57_4_,
         insurerent3_.insurer_name as insurer_9_57_4_,
         insurerent3_.insurer_number as insurer10_57_4_,
         companyent4_.id as id1_32_5_,
         companyent4_.create_by as create_b2_32_5_,
         companyent4_.create_date as create_d3_32_5_,
         companyent4_.update_by as update_b4_32_5_,
         companyent4_.update_date as update_d5_32_5_,
         companyent4_.company_uri as company_6_32_5_,
         companyent4_.description as descript7_32_5_,
         companyent4_.has_vat_id as has_vat_8_32_5_,
         companyent4_.image_file_id as image_fi9_32_5_,
         companyent4_.language as languag10_32_5_,
         companyent4_.logo_fileid as logo_fi11_32_5_,
         companyent4_.logo_position as logo_po12_32_5_,
         companyent4_.representative_id as represe16_32_5_,
         companyent4_.sector_name as sector_13_32_5_,
         companyent4_.simplified_tax_at_source as simplif14_32_5_,
         companyent4_.wage_agreement as wage_ag15_32_5_,
         representa5_.id as id1_74_6_,
         representa5_.create_by as create_b2_74_6_,
         representa5_.create_date as create_d3_74_6_,
         representa5_.update_by as update_b4_74_6_,
         representa5_.update_date as update_d5_74_6_,
         representa5_.address as address6_74_6_,
         representa5_.company as company7_74_6_,
         representa5_.country as country8_74_6_,
         representa5_.email as email9_74_6_,
         representa5_.name as name10_74_6_,
         representa5_.phone as phone11_74_6_,
         representa5_.website as website12_74_6_,
         representa5_.zip_code as zip_cod13_74_6_,
         workplaceq6_.workplace_id as workplac9_121_7_,
         workplaceq6_.id as id1_121_7_,
         workplaceq6_.id as id1_121_8_,
         workplaceq6_.create_by as create_b2_121_8_,
         workplaceq6_.create_date as create_d3_121_8_,
         workplaceq6_.update_by as update_b4_121_8_,
         workplaceq6_.update_date as update_d5_121_8_,
         workplaceq6_.commission_rate as commissi6_121_8_,
         workplaceq6_.qst_id as qst_id7_121_8_,
         workplaceq6_.state_uri as state_ur8_121_8_,
         workplaceq6_.workplace_id as workplac9_121_8_ 
     from
         workplace workplacee0_ 
     left outer join
         company companyent1_ 
             on workplacee0_.company_id=companyent1_.id 
     left outer join
         insurance_contract insurancec2_ 
             on companyent1_.id=insurancec2_.company_id 
     left outer join
         insurer insurerent3_ 
             on insurancec2_.insurer_id=insurerent3_.id 
     left outer join
         company companyent4_ 
             on insurerent3_.company_id=companyent4_.id 
     left outer join
         representative representa5_ 
             on companyent4_.representative_id=representa5_.id 
     left outer join
         workplace_qst workplaceq6_ 
             on workplacee0_.id=workplaceq6_.workplace_id 
     where
         workplacee0_.id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="82bd194b-97b7-48b9-ada7-3f3438b42362" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get work permit (if exist)</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         workpermit0_.employee_id as employe10_117_0_,
         workpermit0_.id as id1_117_0_,
         workpermit0_.id as id1_117_1_,
         workpermit0_.create_by as create_b2_117_1_,
         workpermit0_.create_date as create_d3_117_1_,
         workpermit0_.update_by as update_b4_117_1_,
         workpermit0_.update_date as update_d5_117_1_,
         workpermit0_.valid_from as valid_fr6_117_1_,
         workpermit0_.valid_to as valid_to7_117_1_,
         workpermit0_.employee_id as employe10_117_1_,
         workpermit0_.withholding_tax_duty_despite_settledC as withhold8_117_1_,
         workpermit0_.work_permit as work_per9_117_1_ 
     from
         work_permit_info workpermit0_ 
     where
         workpermit0_.employee_id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f05ab6cf-f84c-41c4-81a7-7b4ef61e07d9" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get civil information (if exist)</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         civilinfos0_.employee_id as employe14_31_0_,
         civilinfos0_.id as id1_31_0_,
         civilinfos0_.id as id1_31_1_,
         civilinfos0_.create_by as create_b2_31_1_,
         civilinfos0_.create_date as create_d3_31_1_,
         civilinfos0_.update_by as update_b4_31_1_,
         civilinfos0_.update_date as update_d5_31_1_,
         civilinfos0_.valid_from as valid_fr6_31_1_,
         civilinfos0_.valid_to as valid_to7_31_1_,
         civilinfos0_.concubinage_type as concubin8_31_1_,
         civilinfos0_.employee_id as employe14_31_1_,
         civilinfos0_.spouse_first_name as spouse_f9_31_1_,
         civilinfos0_.spouse_last_name as spouse_10_31_1_,
         civilinfos0_.spouse_uri as spouse_11_31_1_,
         civilinfos0_.state as state12_31_1_,
         civilinfos0_.valid_date as valid_d13_31_1_ 
     from
         civil_info civilinfos0_ 
     where
         civilinfos0_.employee_id=?
</code></pre>
</div>
</div>
</div></td>
<td><span class="legacy-color-text-blue3">It's triggered by the eager fetch type annotated in Contract Entity and related entities</span></td>
<td><ul>
<li>Remove Eager fetch for data not prepare for payslip calculation</li>
<li>Loading once along with contract for data need for payslip calculation</li>
</ul></td>
</tr>
<tr>
<td>Get all latest payslips by contract ids</td>
<td>~134</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f18cbb15-ecf1-4e41-abfc-1f534599395d" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get latest payslip ids by contract ids</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     SELECT
         max(payslipent1_.id) 
     FROM
         (SELECT
             p.contract_id as c_id,
             max(p.period_from) as p_month 
         FROM
             payslip p 
         GROUP BY
             p.contract_id,
             p.state) latest 
     LEFT OUTER JOIN
         payslip payslipent1_ 
             ON payslipent1_.contract_id=latest.c_id 
             AND payslipent1_.period_from=latest.p_month 
     WHERE
         payslipent1_.contract_id IN (
             ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?
         ) 
     GROUP BY
         payslipent1_.contract_id,
         payslipent1_.state
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="11a21ef1-bc60-41aa-bab7-0487d53bb03d" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get payslip include SITs from list payslips' id</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         distinct payslipent0_.id as id1_69_0_,
         salaryrune4_.id as id1_80_1_,
         salaryitem5_.id as id1_77_2_,
         payslipent0_.create_by as create_b2_69_0_,
         payslipent0_.create_date as create_d3_69_0_,
         payslipent0_.update_by as update_b4_69_0_,
         payslipent0_.update_date as update_d5_69_0_,
         payslipent0_.contract_id as contrac12_69_0_,
         payslipent0_.period_from as period_f6_69_0_,
         payslipent0_.period_to_exclusive as period_t7_69_0_,
         payslipent0_.salary_run_id as salary_13_69_0_,
         payslipent0_.sealed_time as sealed_t8_69_0_,
         payslipent0_.state as state9_69_0_,
         payslipent0_.version as version10_69_0_,
         payslipent0_.warning_level as warning11_69_0_,
         salaryrune4_.create_by as create_b2_80_1_,
         salaryrune4_.create_date as create_d3_80_1_,
         salaryrune4_.update_by as update_b4_80_1_,
         salaryrune4_.update_date as update_d5_80_1_,
         salaryrune4_.booking_link as booking_6_80_1_,
         salaryrune4_.company_id as company_8_80_1_,
         salaryrune4_.payment_date as payment_7_80_1_,
         salaryitem5_.create_by as create_b2_77_2_,
         salaryitem5_.create_date as create_d3_77_2_,
         salaryitem5_.update_by as update_b4_77_2_,
         salaryitem5_.update_date as update_d5_77_2_,
         salaryitem5_.final_base_value as final_ba6_77_2_,
         salaryitem5_.final_quantity as final_qu7_77_2_,
         salaryitem5_.final_rate as final_ra8_77_2_,
         salaryitem5_.final_value as final_va9_77_2_,
         salaryitem5_.from_formula_base_value as from_fo10_77_2_,
         salaryitem5_.from_formula_quantity as from_fo11_77_2_,
         salaryitem5_.from_formula_rate as from_fo12_77_2_,
         salaryitem5_.from_formula_value as from_fo13_77_2_,
         salaryitem5_.full_month_base_value as full_mo14_77_2_,
         salaryitem5_.full_month_quantity as full_mo15_77_2_,
         salaryitem5_.full_month_rate as full_mo16_77_2_,
         salaryitem5_.full_month_value as full_mo17_77_2_,
         salaryitem5_.name_modifier as name_mo18_77_2_,
         salaryitem5_.overwrite_base_value as overwri19_77_2_,
         salaryitem5_.overwrite_quantity as overwri20_77_2_,
         salaryitem5_.overwrite_rate as overwri21_77_2_,
         salaryitem5_.overwrite_value as overwri22_77_2_,
         salaryitem5_.payslip_id as payslip28_77_2_,
         salaryitem5_.reimbursement_id as reimbur29_77_2_,
         salaryitem5_.reimbursement_request_uri as reimbur23_77_2_,
         salaryitem5_.reimbursement_resource_uri as reimbur24_77_2_,
         salaryitem5_.remark as remark25_77_2_,
         salaryitem5_.salary_item_code_string as salary_26_77_2_,
         salaryitem5_.salary_item_type_id as salary_27_77_2_,
         salaryitem5_.payslip_id as payslip28_77_0__,
         salaryitem5_.id as id1_77_0__ 
     from
         payslip payslipent0_ 
     left outer join
         contract contracten1_ 
             on payslipent0_.contract_id=contracten1_.id 
     left outer join
         employee employeeen2_ 
             on contracten1_.employee_id=employeeen2_.id 
     left outer join
         company companyent3_ 
             on employeeen2_.company=companyent3_.id 
     left outer join
         salary_run salaryrune4_ 
             on payslipent0_.salary_run_id=salaryrune4_.id 
     left outer join
         salary_item salaryitem5_ 
             on payslipent0_.id=salaryitem5_.payslip_id 
     where
         companyent3_.id=1 
         and (
             payslipent0_.id in (
                 139 , 106 , 88 , 145 , 87 , 124 , 125 , 107 , 101 , 115 , 99 , 140 , 135 , 114 , 98 , 134 , 35 , 136 , 138 , 36 , 100 , 137 , 72 , 42 , 73
             ) 
             or (
                 contracten1_.exit_date is not null
             ) 
             and payslipent0_.period_from&gt;contracten1_.exit_date 
             and payslipent0_.state=? 
             and (
                 contracten1_.id in (
                     8 , 10 , 15 , 16 , 18 , 19 , 20 , 22 , 23 , 25 , 26 , 27 , 28 , 29 , 30 , 33
                 )
             )
         )
</code></pre>
</div>
</div>
</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td rowspan="2">Load company information</td>
<td rowspan="2">~160</td>
<td><div class="content-wrapper">
<p>Find company information</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="dd90d08a-d74b-4671-b8cb-d044e955254a" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Find company</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         distinct companyent0_.id as id1_32_0_,
         workplaces1_.id as id1_120_1_,
         workplaceq2_.id as id1_121_2_,
         insurancec3_.id as id1_56_3_,
         insurerent4_.id as id1_57_4_,
         insurancec5_.id as id1_55_5_,
         companyent0_.create_by as create_b2_32_0_,
         companyent0_.create_date as create_d3_32_0_,
         companyent0_.update_by as update_b4_32_0_,
         companyent0_.update_date as update_d5_32_0_,
         companyent0_.company_uri as company_6_32_0_,
         companyent0_.description as descript7_32_0_,
         companyent0_.has_vat_id as has_vat_8_32_0_,
         companyent0_.image_file_id as image_fi9_32_0_,
         companyent0_.language as languag10_32_0_,
         companyent0_.logo_fileid as logo_fi11_32_0_,
         companyent0_.logo_position as logo_po12_32_0_,
         companyent0_.representative_id as represe16_32_0_,
         companyent0_.sector_name as sector_13_32_0_,
         companyent0_.simplified_tax_at_source as simplif14_32_0_,
         companyent0_.wage_agreement as wage_ag15_32_0_,
         workplaces1_.create_by as create_b2_120_1_,
         workplaces1_.create_date as create_d3_120_1_,
         workplaces1_.update_by as update_b4_120_1_,
         workplaces1_.update_date as update_d5_120_1_,
         workplaces1_.code as code6_120_1_,
         workplaces1_.companyUri as companyU7_120_1_,
         workplaces1_.description as descript8_120_1_,
         workplaces1_.company_id as company11_120_1_,
         workplaces1_.weekly_hours as weekly_h9_120_1_,
         workplaces1_.weekly_lessons as weekly_10_120_1_,
         workplaces1_.company_id as company11_120_0__,
         workplaces1_.id as id1_120_0__,
         workplaceq2_.create_by as create_b2_121_2_,
         workplaceq2_.create_date as create_d3_121_2_,
         workplaceq2_.update_by as update_b4_121_2_,
         workplaceq2_.update_date as update_d5_121_2_,
         workplaceq2_.commission_rate as commissi6_121_2_,
         workplaceq2_.qst_id as qst_id7_121_2_,
         workplaceq2_.state_uri as state_ur8_121_2_,
         workplaceq2_.workplace_id as workplac9_121_2_,
         workplaceq2_.workplace_id as workplac9_121_1__,
         workplaceq2_.id as id1_121_1__,
         insurancec3_.create_by as create_b2_56_3_,
         insurancec3_.create_date as create_d3_56_3_,
         insurancec3_.update_by as update_b4_56_3_,
         insurancec3_.update_date as update_d5_56_3_,
         insurancec3_.valid_from as valid_fr6_56_3_,
         insurancec3_.valid_to as valid_to7_56_3_,
         insurancec3_.additional_info as addition8_56_3_,
         insurancec3_.business_case as business9_56_3_,
         insurancec3_.company_id as company15_56_3_,
         insurancec3_.customer_number as custome10_56_3_,
         insurancec3_.global_insurer_id as global_11_56_3_,
         insurancec3_.insurance_policy_number as insuran12_56_3_,
         insurancec3_.insurance_policy_rate as insuran13_56_3_,
         insurancec3_.insurance_short_code as insuran14_56_3_,
         insurancec3_.insurer_id as insurer16_56_3_,
         insurancec3_.company_id as company15_56_2__,
         insurancec3_.id as id1_56_2__,
         insurerent4_.create_by as create_b2_57_4_,
         insurerent4_.create_date as create_d3_57_4_,
         insurerent4_.update_by as update_b4_57_4_,
         insurerent4_.update_date as update_d5_57_4_,
         insurerent4_.canton_uri as canton_u6_57_4_,
         insurerent4_.company_id as company11_57_4_,
         insurerent4_.company_uri as company_7_57_4_,
         insurerent4_.insurer_type as insurer_8_57_4_,
         insurerent4_.insurer_name as insurer_9_57_4_,
         insurerent4_.insurer_number as insurer10_57_4_,
         insurancec5_.create_by as create_b2_55_5_,
         insurancec5_.create_date as create_d3_55_5_,
         insurancec5_.update_by as update_b4_55_5_,
         insurancec5_.update_date as update_d5_55_5_,
         insurancec5_.valid_from as valid_fr6_55_5_,
         insurancec5_.valid_to as valid_to7_55_5_,
         insurancec5_.emp_rate_for_men as emp_rate8_55_5_,
         insurancec5_.emp_rate_for_women as emp_rate9_55_5_,
         insurancec5_.employer_secondary_rate as employe10_55_5_,
         insurancec5_.insurance_code_description as insuran11_55_5_,
         insurancec5_.insurance_code_string as insuran12_55_5_,
         insurancec5_.insurance_contract_id as insuran18_55_5_,
         insurancec5_.rate_for_men as rate_fo13_55_5_,
         insurancec5_.rate_for_women as rate_fo14_55_5_,
         insurancec5_.salary_item_type_code as salary_15_55_5_,
         insurancec5_.salary_range_maximum as salary_16_55_5_,
         insurancec5_.salary_range_minimum as salary_17_55_5_,
         insurancec5_.insurance_contract_id as insuran18_55_3__,
         insurancec5_.id as id1_55_3__ 
     from
         company companyent0_ 
     left outer join
         workplace workplaces1_ 
             on companyent0_.id=workplaces1_.company_id 
     left outer join
         workplace_qst workplaceq2_ 
             on workplaces1_.id=workplaceq2_.workplace_id 
     left outer join
         insurance_contract insurancec3_ 
             on companyent0_.id=insurancec3_.company_id 
     left outer join
         insurer insurerent4_ 
             on insurancec3_.insurer_id=insurerent4_.id 
     left outer join
         insurance_code insurancec5_ 
             on insurancec3_.id=insurancec5_.insurance_contract_id 
     where
         companyent0_.id in (
             1
         )
</code></pre>
</div>
</div>
</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Trigger service call to luz_person to fetch other infomation regarding workplace, global insurance</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d86e00a8-53e0-42b0-b759-41c996228dec" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get detail workplace info</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         workingday0_.workplace_id as workplac9_118_1_,
         workingday0_.id as id1_118_1_,
         workingday0_.id as id1_118_0_,
         workingday0_.create_by as create_b2_118_0_,
         workingday0_.create_date as create_d3_118_0_,
         workingday0_.update_by as update_b4_118_0_,
         workingday0_.update_date as update_d5_118_0_,
         workingday0_.date_status as date_sta6_118_0_,
         workingday0_.opened as opened7_118_0_,
         workingday0_.workplace_id as workplac9_118_0_,
         workingday0_.day_of_week as day_of_w8_118_0_ 
     from
         working_day workingday0_ 
     where
         workingday0_.workplace_id in (
             select
                 workplaces1_.id 
             from
                 company companyent0_ 
             left outer join
                 workplace workplaces1_ 
                     on companyent0_.id=workplaces1_.company_id 
             left outer join
                 workplace_qst workplaceq2_ 
                     on workplaces1_.id=workplaceq2_.workplace_id 
             left outer join
                 insurance_contract insurancec3_ 
                     on companyent0_.id=insurancec3_.company_id 
             left outer join
                 insurer insurerent4_ 
                     on insurancec3_.insurer_id=insurerent4_.id 
             left outer join
                 insurance_code insurancec5_ 
                     on insurancec3_.id=insurancec5_.insurance_contract_id 
             where
                 companyent0_.id in (
                     1
                 )
         )
&#10; Hibernate: 
     select
         specialday0_.workplace_id as workpla11_84_1_,
         specialday0_.id as id1_84_1_,
         specialday0_.id as id1_84_0_,
         specialday0_.create_by as create_b2_84_0_,
         specialday0_.create_date as create_d3_84_0_,
         specialday0_.update_by as update_b4_84_0_,
         specialday0_.update_date as update_d5_84_0_,
         specialday0_.date_status as date_sta6_84_0_,
         specialday0_.opened as opened7_84_0_,
         specialday0_.workplace_id as workpla11_84_0_,
         specialday0_.from_date as from_dat8_84_0_,
         specialday0_.name as name9_84_0_,
         specialday0_.to_date as to_date10_84_0_ 
     from
         special_day specialday0_ 
     where
         specialday0_.workplace_id in (
             select
                 workplaces1_.id 
             from
                 company companyent0_ 
             left outer join
                 workplace workplaces1_ 
                     on companyent0_.id=workplaces1_.company_id 
             left outer join
                 workplace_qst workplaceq2_ 
                     on workplaces1_.id=workplaceq2_.workplace_id 
             left outer join
                 insurance_contract insurancec3_ 
                     on companyent0_.id=insurancec3_.company_id 
             left outer join
                 insurer insurerent4_ 
                     on insurancec3_.insurer_id=insurerent4_.id 
             left outer join
                 insurance_code insurancec5_ 
                     on insurancec3_.id=insurancec5_.insurance_contract_id 
             where
                 companyent0_.id in (
                     1
                 )
         )
&#10; Hibernate: 
     select
         specialhou0_.special_day_id as special10_85_1_,
         specialhou0_.id as id1_85_1_,
         specialhou0_.id as id1_85_0_,
         specialhou0_.create_by as create_b2_85_0_,
         specialhou0_.create_date as create_d3_85_0_,
         specialhou0_.update_by as update_b4_85_0_,
         specialhou0_.update_date as update_d5_85_0_,
         specialhou0_.from_hour as from_hou6_85_0_,
         specialhou0_.from_minute as from_min7_85_0_,
         specialhou0_.to_hour as to_hour8_85_0_,
         specialhou0_.to_minute as to_minut9_85_0_,
         specialhou0_.special_day_id as special10_85_0_ 
     from
         special_hour specialhou0_ 
     where
         specialhou0_.special_day_id in (
             select
                 specialday0_.id 
             from
                 special_day specialday0_ 
             where
                 specialday0_.workplace_id in (
                     select
                         workplaces1_.id 
                     from
                         company companyent0_ 
                     left outer join
                         workplace workplaces1_ 
                             on companyent0_.id=workplaces1_.company_id 
                     left outer join
                         workplace_qst workplaceq2_ 
                             on workplaces1_.id=workplaceq2_.workplace_id 
                     left outer join
                         insurance_contract insurancec3_ 
                             on companyent0_.id=insurancec3_.company_id 
                     left outer join
                         insurer insurerent4_ 
                             on insurancec3_.insurer_id=insurerent4_.id 
                     left outer join
                         insurance_code insurancec5_ 
                             on insurancec3_.id=insurancec5_.insurance_contract_id 
                     where
                         companyent0_.id in (
                             1
                         )
                 )
             ) 
         order by
             specialhou0_.from_hour,
             specialhou0_.from_minute,
             specialhou0_.to_hour,
             specialhou0_.to_minute asc
&#10; Hibernate: 
     select
         globalinsu0_.id as id1_1_,
         globalinsu0_.create_by as create_b2_1_,
         globalinsu0_.create_date as create_d3_1_,
         globalinsu0_.update_by as update_b4_1_,
         globalinsu0_.update_date as update_d5_1_,
         globalinsu0_.additional_name as addition6_1_,
         globalinsu0_.address_lines as address_7_1_,
         globalinsu0_.canton_uri as canton_u8_1_,
         globalinsu0_.city_uri as city_uri9_1_,
         globalinsu0_.email as email10_1_,
         globalinsu0_.employer_contribution_rate as employe11_1_,
         globalinsu0_.insurer_type as insurer12_1_,
         globalinsu0_.insurer_name as insurer13_1_,
         globalinsu0_.insurer_number as insurer14_1_,
         globalinsu0_.local_branch as local_b15_1_,
         globalinsu0_.official_for_canton as officia16_1_,
         globalinsu0_.phone as phone17_1_ 
     from
         public.global_insurer globalinsu0_
</code></pre>
</div>
</div>
</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td rowspan="2">Load employees' information</td>
<td>~130</td>
<td><div class="content-wrapper">
<p>Get information of employee from luz_compensation</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="56c3c895-31fb-4cd4-ac14-8b64ff336d9c" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get employees</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         distinct employeeen0_.id as id1_48_0_,
         children1_.id as id1_30_1_,
         civilinfos2_.id as id1_31_2_,
         employeepa3_.id as id1_49_3_,
         workpermit4_.id as id1_117_4_,
         employeeen0_.create_by as create_b2_48_0_,
         employeeen0_.create_date as create_d3_48_0_,
         employeeen0_.update_by as update_b4_48_0_,
         employeeen0_.update_date as update_d5_48_0_,
         employeeen0_.avatar_uri as avatar_u6_48_0_,
         employeeen0_.person_uri as person_u7_48_0_,
         employeeen0_.abbreviation as abbrevia8_48_0_,
         employeeen0_.company as company13_48_0_,
         employeeen0_.email_for_payslip as email_fo9_48_0_,
         employeeen0_.emergency_address as emergen10_48_0_,
         employeeen0_.employee_number as employe11_48_0_,
         employeeen0_.payment as payment14_48_0_,
         employeeen0_.is_payslip_sent_by_email as is_pays12_48_0_,
         children1_.create_by as create_b2_30_1_,
         children1_.create_date as create_d3_30_1_,
         children1_.update_by as update_b4_30_1_,
         children1_.update_date as update_d5_30_1_,
         children1_.person_uri as person_u6_30_1_,
         children1_.employee as employee9_30_1_,
         children1_.canton_code as canton_c7_30_1_,
         children1_.country_code as country_8_30_1_,
         children1_.employee as employee9_30_0__,
         children1_.id as id1_30_0__,
         civilinfos2_.create_by as create_b2_31_2_,
         civilinfos2_.create_date as create_d3_31_2_,
         civilinfos2_.update_by as update_b4_31_2_,
         civilinfos2_.update_date as update_d5_31_2_,
         civilinfos2_.valid_from as valid_fr6_31_2_,
         civilinfos2_.valid_to as valid_to7_31_2_,
         civilinfos2_.concubinage_type as concubin8_31_2_,
         civilinfos2_.employee_id as employe14_31_2_,
         civilinfos2_.spouse_first_name as spouse_f9_31_2_,
         civilinfos2_.spouse_last_name as spouse_10_31_2_,
         civilinfos2_.spouse_uri as spouse_11_31_2_,
         civilinfos2_.state as state12_31_2_,
         civilinfos2_.valid_date as valid_d13_31_2_,
         civilinfos2_.employee_id as employe14_31_1__,
         civilinfos2_.id as id1_31_1__,
         employeepa3_.create_by as create_b2_49_3_,
         employeepa3_.create_date as create_d3_49_3_,
         employeepa3_.update_by as update_b4_49_3_,
         employeepa3_.update_date as update_d5_49_3_,
         employeepa3_.additional_address as addition6_49_3_,
         employeepa3_.address as address7_49_3_,
         employeepa3_.bank_account_scope as bank_acc8_49_3_,
         employeepa3_.bank_name as bank_nam9_49_3_,
         employeepa3_.bic_code as bic_cod10_49_3_,
         employeepa3_.cash_payment as cash_pa11_49_3_,
         employeepa3_.iban_number as iban_nu12_49_3_,
         employeepa3_.intended_use as intende13_49_3_,
         employeepa3_.owner_name as owner_n14_49_3_,
         employeepa3_.zip_city as zip_cit15_49_3_,
         workpermit4_.create_by as create_b2_117_4_,
         workpermit4_.create_date as create_d3_117_4_,
         workpermit4_.update_by as update_b4_117_4_,
         workpermit4_.update_date as update_d5_117_4_,
         workpermit4_.valid_from as valid_fr6_117_4_,
         workpermit4_.valid_to as valid_to7_117_4_,
         workpermit4_.employee_id as employe10_117_4_,
         workpermit4_.withholding_tax_duty_despite_settledC as withhold8_117_4_,
         workpermit4_.work_permit as work_per9_117_4_,
         workpermit4_.employee_id as employe10_117_2__,
         workpermit4_.id as id1_117_2__ 
     from
         employee employeeen0_ 
     left outer join
         child children1_ 
             on employeeen0_.id=children1_.employee 
     left outer join
         civil_info civilinfos2_ 
             on employeeen0_.id=civilinfos2_.employee_id 
     left outer join
         employee_payment employeepa3_ 
             on employeeen0_.payment=employeepa3_.id 
     left outer join
         work_permit_info workpermit4_ 
             on employeeen0_.id=workpermit4_.employee_id 
     where
         employeeen0_.company=1
</code></pre>
</div>
</div>
</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="98d99b10-c94f-4db0-bd81-7c9ab65556c7" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Loading person info by employee ids</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         distinct personenti0_.id as id1_18_0_,
         emailentit3_.id as id1_17_1_,
         customfiel4_.id as id1_22_2_,
         addressent6_.id as id1_4_3_,
         cityentity7_.id as id1_0_4_,
         countryent8_.id as id1_2_5_,
         stateentit9_.id as id1_3_6_,
         communitye10_.id as id1_1_7_,
         phoneentit12_.id as id1_27_8_,
         categories13_.id as id1_21_9_,
         onlineplat14_.id as id1_24_10_,
         personenti0_.create_by as create_b2_18_0_,
         personenti0_.create_date as create_d3_18_0_,
         personenti0_.update_by as update_b4_18_0_,
         personenti0_.update_date as update_d5_18_0_,
         personenti0_.additional_name as addition6_18_0_,
         personenti0_.birthday as birthday7_18_0_,
         personenti0_.correspondence as correspo8_18_0_,
         personenti0_.first_name as first_na9_18_0_,
         personenti0_.image_id as image_i10_18_0_,
         personenti0_.language as languag11_18_0_,
         personenti0_.last_name as last_na12_18_0_,
         personenti0_.nationality as nationa13_18_0_,
         personenti0_.person_number as person_14_18_0_,
         personenti0_.origin_reference as origin_15_18_0_,
         personenti0_.place_of_origin as place_o16_18_0_,
         personenti0_.responsible_counterpart as respons17_18_0_,
         personenti0_.salutation as salutat18_18_0_,
         personenti0_.sex as sex19_18_0_,
         personenti0_.social_security_number as social_20_18_0_,
         personenti0_.website as website21_18_0_,
         externalre1_.person_id as person_i1_26_0__,
         externalre1_.referencing_module as referenc2_26_0__,
         externalre1_.referencing_type as referenc3_26_0__,
         externalre1_.referencing_uri as referenc4_26_0__,
         emailentit3_.create_by as create_b2_17_1_,
         emailentit3_.create_date as create_d3_17_1_,
         emailentit3_.update_by as update_b4_17_1_,
         emailentit3_.update_date as update_d5_17_1_,
         emailentit3_.email_address as email_ad6_17_1_,
         emailentit3_.type as type7_17_1_,
         emailentit3_1_.person_id as person_i1_23_1_,
         emailentit3_2_.company_id as company_1_11_1_,
         emaillist2_.person_id as person_i1_23_1__,
         emaillist2_.email_id as email_id2_23_1__,
         customfiel4_.create_by as create_b2_22_2_,
         customfiel4_.create_date as create_d3_22_2_,
         customfiel4_.update_by as update_b4_22_2_,
         customfiel4_.update_date as update_d5_22_2_,
         customfiel4_.custom_name as custom_n6_22_2_,
         customfiel4_.custom_value as custom_v7_22_2_,
         customfiel4_.person_id as person_i8_22_2_,
         customfiel4_.person_id as person_i8_22_2__,
         customfiel4_.id as id1_22_2__,
         addressent6_.create_by as create_b2_4_3_,
         addressent6_.create_date as create_d3_4_3_,
         addressent6_.update_by as update_b4_4_3_,
         addressent6_.update_date as update_d5_4_3_,
         addressent6_.valid_from as valid_fr6_4_3_,
         addressent6_.valid_to as valid_to7_4_3_,
         addressent6_.additional_address as addition8_4_3_,
         addressent6_.address_lines as address_9_4_3_,
         addressent6_.address_type as address10_4_3_,
         addressent6_.city_id as city_id20_4_3_,
         addressent6_.company_name as company11_4_3_,
         addressent6_.definition_name as definit12_4_3_,
         addressent6_.first_name as first_n13_4_3_,
         addressent6_.geo_hash as geo_has14_4_3_,
         addressent6_.last_name as last_na15_4_3_,
         addressent6_.latitude as latitud16_4_3_,
         addressent6_.longitude as longitu17_4_3_,
         addressent6_.salutation as salutat18_4_3_,
         addressent6_.type as type19_4_3_,
         addressent6_1_.person_id as person_i1_20_3_,
         addressent6_2_.company_id as company_1_8_3_,
         addresslis5_.person_id as person_i1_20_3__,
         addresslis5_.address_id as address_2_20_3__,
         cityentity7_.create_by as create_b2_0_4_,
         cityentity7_.create_date as create_d3_0_4_,
         cityentity7_.update_by as update_b4_0_4_,
         cityentity7_.update_date as update_d5_0_4_,
         cityentity7_.additional_postcode as addition6_0_4_,
         cityentity7_.address_postcode as address_7_0_4_,
         cityentity7_.alternative_language as alternat8_0_4_,
         cityentity7_.basic_postcode as basic_po9_0_4_,
         cityentity7_.city_name_18 as city_na10_0_4_,
         cityentity7_.city_name_27 as city_na11_0_4_,
         cityentity7_.community_id as communi19_0_4_,
         cityentity7_.country_id as country20_0_4_,
         cityentity7_.delivery_office as deliver12_0_4_,
         cityentity7_.delivery_office_postcode as deliver13_0_4_,
         cityentity7_.language_code as languag14_0_4_,
         cityentity7_.onrp as onrp15_0_4_,
         cityentity7_.plz_coff as plz_cof16_0_4_,
         cityentity7_.postcode_type as postcod17_0_4_,
         cityentity7_.state_id as state_i21_0_4_,
         cityentity7_.validity as validit18_0_4_,
         countryent8_.create_by as create_b2_2_5_,
         countryent8_.create_date as create_d3_2_5_,
         countryent8_.update_by as update_b4_2_5_,
         countryent8_.update_date as update_d5_2_5_,
         countryent8_.country_name as country_6_2_5_,
         countryent8_.iso_2code as iso_7_2_5_,
         countryent8_.iso_3code as iso_8_2_5_,
         countryent8_.numeric_code as numeric_9_2_5_,
         countryent8_.phone_code as phone_c10_2_5_,
         stateentit9_.create_by as create_b2_3_6_,
         stateentit9_.create_date as create_d3_3_6_,
         stateentit9_.update_by as update_b4_3_6_,
         stateentit9_.update_date as update_d5_3_6_,
         stateentit9_.code as code6_3_6_,
         stateentit9_.description as descript7_3_6_,
         communitye10_.create_by as create_b2_1_7_,
         communitye10_.create_date as create_d3_1_7_,
         communitye10_.update_by as update_b4_1_7_,
         communitye10_.update_date as update_d5_1_7_,
         communitye10_.bfsnr as bfsnr6_1_7_,
         communitye10_.community_name as communit7_1_7_,
         communitye10_.conurbation_number as conurbat8_1_7_,
         communitye10_.state_id as state_id9_1_7_,
         phoneentit12_.create_by as create_b2_27_8_,
         phoneentit12_.create_date as create_d3_27_8_,
         phoneentit12_.update_by as update_b4_27_8_,
         phoneentit12_.update_date as update_d5_27_8_,
         phoneentit12_.phone_number as phone_nu6_27_8_,
         phoneentit12_.type as type7_27_8_,
         phoneentit12_1_.company_id as company_1_13_8_,
         phoneentit12_2_.person_id as person_i1_25_8_,
         phonelist11_.person_id as person_i1_25_4__,
         phonelist11_.phone_id as phone_id2_25_4__,
         categories13_.create_by as create_b2_21_9_,
         categories13_.create_date as create_d3_21_9_,
         categories13_.update_by as update_b4_21_9_,
         categories13_.update_date as update_d5_21_9_,
         categories13_.category as category6_21_9_,
         categories13_.person_id as person_i7_21_9_,
         categories13_.person_id as person_i7_21_5__,
         categories13_.id as id1_21_5__,
         onlineplat14_.create_by as create_b2_24_10_,
         onlineplat14_.create_date as create_d3_24_10_,
         onlineplat14_.update_by as update_b4_24_10_,
         onlineplat14_.update_date as update_d5_24_10_,
         onlineplat14_.platform_name as platform6_24_10_,
         onlineplat14_.platform_value as platform7_24_10_,
         onlineplat14_.person_id as person_i8_24_10_,
         onlineplat14_.person_id as person_i8_24_6__,
         onlineplat14_.id as id1_24_6__ 
     from
         person personenti0_ 
     left outer join
         person_reference externalre1_ 
             on personenti0_.id=externalre1_.person_id 
     left outer join
         person_email emaillist2_ 
             on personenti0_.id=emaillist2_.person_id 
     left outer join
         email emailentit3_ 
             on emaillist2_.email_id=emailentit3_.id 
     left outer join
         person_email emailentit3_1_ 
             on emailentit3_.id=emailentit3_1_.email_id 
     left outer join
         company_email emailentit3_2_ 
             on emailentit3_.id=emailentit3_2_.email_id 
     left outer join
         person_custom_field customfiel4_ 
             on personenti0_.id=customfiel4_.person_id 
     left outer join
         person_address addresslis5_ 
             on personenti0_.id=addresslis5_.person_id 
     left outer join
         address addressent6_ 
             on addresslis5_.address_id=addressent6_.id 
     left outer join
         person_address addressent6_1_ 
             on addressent6_.id=addressent6_1_.address_id 
     left outer join
         company_address addressent6_2_ 
             on addressent6_.id=addressent6_2_.address_id 
     left outer join
         public.city cityentity7_ 
             on addressent6_.city_id=cityentity7_.id 
     left outer join
         public.country countryent8_ 
             on cityentity7_.country_id=countryent8_.id 
     left outer join
         public.state stateentit9_ 
             on cityentity7_.state_id=stateentit9_.id 
     left outer join
         public.community communitye10_ 
             on cityentity7_.community_id=communitye10_.id 
     left outer join
         person_phone phonelist11_ 
             on personenti0_.id=phonelist11_.person_id 
     left outer join
         phone phoneentit12_ 
             on phonelist11_.phone_id=phoneentit12_.id 
     left outer join
         company_phone phoneentit12_1_ 
             on phoneentit12_.id=phoneentit12_1_.phone_id 
     left outer join
         person_phone phoneentit12_2_ 
             on phoneentit12_.id=phoneentit12_2_.phone_id 
     left outer join
         person_category categories13_ 
             on personenti0_.id=categories13_.person_id 
     left outer join
         person_online_platform onlineplat14_ 
             on personenti0_.id=onlineplat14_.person_id 
     where
         personenti0_.id in (
             32 , 20 , 26 , 30 , 33 , 21 , 36 , 28 , 27 , 37 , 29 , 40 , 18 , 35 , 22 , 34 , 12 , 10 , 17 , 38
         ) 
     order by
         emailentit3_.id asc,
         addressent6_.id asc,
         phoneentit12_.id asc,
         categories13_.category asc,
         onlineplat14_.platform_name asc
</code></pre>
</div>
</div>
</div></td>
<td>The query is a little bit long.</td>
<td><p>We can split them by:</p>
<ul>
<li>Loading person first</li>
<li>Loading dependent data(email, phone, address...)</li>
<li>Join dependent data with person</li>
</ul></td>
</tr>
<tr>
<td rowspan="2"><p>Loading tax records</p></td>
<td rowspan="2">~420<br />
(Test data just 4 cantons and 4 tariff codes)</td>
<td><div class="content-wrapper">
<p>Before loading tax record, we need canton and tariff codes from contracts regarding to tax at source.</p>
<p>If any contract having tax at source, N+1 queries are triggered:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="bde053e9-8a1b-475b-9bd3-46aa2569a22a" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get tax at source residence</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         taxatsourc0_.id as id1_86_0_,
         taxatsourc0_.create_by as create_b2_86_0_,
         taxatsourc0_.create_date as create_d3_86_0_,
         taxatsourc0_.update_by as update_b4_86_0_,
         taxatsourc0_.update_date as update_d5_86_0_,
         taxatsourc0_.annuity as annuity6_86_0_,
         taxatsourc0_.category_predefined as category7_86_0_,
         taxatsourc0_.crossboarder_living_in_italy_id as crossbo17_86_0_,
         taxatsourc0_.denomination as denomina8_86_0_,
         taxatsourc0_.employment_category as employme9_86_0_,
         taxatsourc0_.exclude_from_elm as exclude10_86_0_,
         taxatsourc0_.has_other_employment_or_pension as has_oth11_86_0_,
         taxatsourc0_.other_activities as other_a12_86_0_,
         taxatsourc0_.other_employment_or_pension_amount as other_e13_86_0_,
         taxatsourc0_.other_employment_or_pension_rate as other_e14_86_0_,
         taxatsourc0_.residence_id as residen18_86_0_,
         taxatsourc0_.simplified_procedure as simplif15_86_0_,
         taxatsourc0_.single_parent_info_id as single_19_86_0_,
         taxatsourc0_.spouse_income_id as spouse_20_86_0_,
         taxatsourc0_.valid_date as valid_d16_86_0_ 
     from
         tax_at_source taxatsourc0_ 
     where
         taxatsourc0_.id=?
&#10; Hibernate: 
     select
         residencee0_.id as id1_88_0_,
         residencee0_.create_by as create_b2_88_0_,
         residencee0_.create_date as create_d3_88_0_,
         residencee0_.update_by as update_b4_88_0_,
         residencee0_.update_date as update_d5_88_0_,
         residencee0_.abroad_country_code as abroad_c6_88_0_,
         residencee0_.additional_address as addition7_88_0_,
         residencee0_.canton_CH as canton_C8_88_0_,
         residencee0_.city_uri as city_uri9_88_0_,
         residencee0_.number_illness_days as number_10_88_0_,
         residencee0_.number_working_days as number_11_88_0_,
         residencee0_.residence_type as residen12_88_0_,
         residencee0_.weekly_address as weekly_13_88_0_ 
     from
         tax_at_source_residence residencee0_ 
     where
         residencee0_.id=?
</code></pre>
</div>
</div>
</div></td>
<td>It is trigger by <strong>getter</strong> method during access to TaxAtSource entity</td>
<td>Should load this info along with tax at source info at once</td>
</tr>
<tr>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1eada2b9-6df4-4095-a29f-777d99ef5e17" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Find tax records</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         taxrecorde0_.id as id1_23_,
         taxrecorde0_.create_by as create_b2_23_,
         taxrecorde0_.create_date as create_d3_23_,
         taxrecorde0_.update_by as update_b4_23_,
         taxrecorde0_.update_date as update_d5_23_,
         taxrecorde0_.valid_from as valid_fr6_23_,
         taxrecorde0_.valid_to as valid_to7_23_,
         taxrecorde0_.canton as canton8_23_,
         taxrecorde0_.code_gender as code_gen9_23_,
         taxrecorde0_.code_status as code_st10_23_,
         taxrecorde0_.number_of_children as number_11_23_,
         taxrecorde0_.record_type as record_12_23_,
         taxrecorde0_.tariff as tariff13_23_,
         taxrecorde0_.tariff_step as tariff_14_23_,
         taxrecorde0_.tax as tax15_23_,
         taxrecorde0_.tax_rate as tax_rat16_23_,
         taxrecorde0_.taxable_income as taxable17_23_,
         taxrecorde0_.transaction_type as transac18_23_ 
     from
         public.tax_record taxrecorde0_ 
     where
         (
             taxrecorde0_.canton in (
                 ? , ? , ? , ?
             )
         ) 
         and (
             taxrecorde0_.tariff in (
                 ? , ? , ? , ?
             )
         ) 
     order by
         taxrecorde0_.taxable_income desc
</code></pre>
</div>
</div>
</div></td>
<td>Currently we load too much records than the need.</td>
<td>Load enough tax records that need for all contracts(<strong>HARD</strong>)</td>
</tr>
<tr>
<td>Load all canton configurations</td>
<td>~50</td>
<td><div class="content-wrapper">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="2dbf1534-cfcd-4f84-849c-bbd25f32bd55" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Load canton configurations</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         cantonconf0_.id as id1_0_,
         cantonconf0_.create_by as create_b2_0_,
         cantonconf0_.create_date as create_d3_0_,
         cantonconf0_.update_by as update_b4_0_,
         cantonconf0_.update_date as update_d5_0_,
         cantonconf0_.valid_from as valid_fr6_0_,
         cantonconf0_.valid_to as valid_to7_0_,
         cantonconf0_.a2b_exeption as a8_0_,
         cantonconf0_.canton_code as canton_c9_0_,
         cantonconf0_.consider_all_months as conside10_0_,
         cantonconf0_.extrapolate_hourly_salary as extrapo11_0_,
         cantonconf0_.extrapolate_month as extrapo12_0_,
         cantonconf0_.minimal_deduction as minimal13_0_,
         cantonconf0_.monthly_adjustement as monthly14_0_,
         cantonconf0_.rate_group_adjustement as rate_gr15_0_,
         cantonconf0_.yearly_compensation as yearly_16_0_ 
     from
         public.canton_configuration cantonconf0_ 
     where
         1=1
</code></pre>
</div>
</div>
</div></td>
<td>Always load all canton in public.canton_configuration</td>
<td><br />
</td>
</tr>
<tr>
<td rowspan="2">Calculate salary for each SIT in payslip</td>
<td rowspan="2">~4210</td>
<td><div class="content-wrapper">
<p>Before calculate, public salary configuration need to load first</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="2acb17f9-679c-4d0c-804f-93ca0b5c2f65" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get public salary configuration</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         distinct salaryconf0_.id as id1_4_0_,
         variables1_.id as id1_5_1_,
         salaryitem3_.id as id1_15_2_,
         salaryitem5_.id as id1_16_3_,
         salaryconf0_.create_by as create_b2_4_0_,
         salaryconf0_.create_date as create_d3_4_0_,
         salaryconf0_.update_by as update_b4_4_0_,
         salaryconf0_.update_date as update_d5_4_0_,
         salaryconf0_.code as code6_4_0_,
         salaryconf0_.description as descript7_4_0_,
         salaryconf0_.name as name8_4_0_,
         variables1_.create_by as create_b2_5_1_,
         variables1_.create_date as create_d3_5_1_,
         variables1_.update_by as update_b4_5_1_,
         variables1_.update_date as update_d5_5_1_,
         variables1_.value as value6_5_1_,
         variables1_.data_type as data_typ7_5_1_,
         variables1_.is_final as is_final8_5_1_,
         variables1_.name as name9_5_1_,
         variables1_.valid_from as valid_f10_5_1_,
         variables1_.valid_to as valid_t11_5_1_,
         variables1_.holder_id as holder_12_5_0__,
         variables1_.id as id1_5_0__,
         salaryitem3_.create_by as create_b2_15_2_,
         salaryitem3_.create_date as create_d3_15_2_,
         salaryitem3_.update_by as update_b4_15_2_,
         salaryitem3_.update_date as update_d5_15_2_,
         salaryitem3_.code as code6_15_2_,
         salaryitem3_.formula as formula7_15_2_,
         salaryitem3_.name as name8_15_2_,
         salaryitem3_.print_sequence as print_se9_15_2_,
         salaryitem3_.reverse_sign as reverse10_15_2_,
         salaryitem2_.salary_configuration_id as salary_c1_20_1__,
         salaryitem2_.salary_item_type_id as salary_i2_20_1__,
         salaryitem5_.create_by as create_b2_16_3_,
         salaryitem5_.create_date as create_d3_16_3_,
         salaryitem5_.update_by as update_b4_16_3_,
         salaryitem5_.update_date as update_d5_16_3_,
         salaryitem5_.code as code6_16_3_,
         salaryitem5_.description as descript7_16_3_,
         salaryitem5_.name as name8_16_3_,
         salaryitem4_.salary_configuration_id as salary_c1_17_2__,
         salaryitem4_.salary_item_type_group_id as salary_i2_17_2__ 
     from
         public.salary_configuration salaryconf0_ 
     left outer join
         public.salary_configuration_variable variables1_ 
             on salaryconf0_.id=variables1_.holder_id 
     left outer join
         public.salary_item_type_salary_configuration salaryitem2_ 
             on salaryconf0_.id=salaryitem2_.salary_configuration_id 
     left outer join
         public.salary_item_type salaryitem3_ 
             on salaryitem2_.salary_item_type_id=salaryitem3_.id 
     left outer join
         public.salary_item_type_group_salary_configuration salaryitem4_ 
             on salaryconf0_.id=salaryitem4_.salary_configuration_id 
     left outer join
         public.salary_item_type_group salaryitem5_ 
             on salaryitem4_.salary_item_type_group_id=salaryitem5_.id
</code></pre>
</div>
</div>
</div></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>N+ 1 queries for each salary item type in public salary configuration are triggered:</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="d294ec71-5211-4238-9776-c21c76859ae3" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get salary item type</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         i18n0_.salary_item_type_id as salary_i9_19_0_,
         i18n0_.id as id1_19_0_,
         i18n0_.id as id1_19_1_,
         i18n0_.create_by as create_b2_19_1_,
         i18n0_.create_date as create_d3_19_1_,
         i18n0_.update_by as update_b4_19_1_,
         i18n0_.update_date as update_d5_19_1_,
         i18n0_.description as descript6_19_1_,
         i18n0_.locale as locale7_19_1_,
         i18n0_.salary_item_type_id as salary_i9_19_1_,
         i18n0_.tag as tag8_19_1_ 
     from
         public.salary_item_type_i18n i18n0_ 
     where
         i18n0_.salary_item_type_id=?
</code></pre>
</div>
</div>
</div></td>
<td><span class="legacy-color-text-blue3">It it triggered by equals and hashCode method</span></td>
<td>Should load salary configuration including its salary item type at once</td>
</tr>
<tr>
<td>Convert data to model before returning to client</td>
<td>~1105</td>
<td><div class="content-wrapper">
<p>When convert contract temporal of contract, some N + 1 queries are triggered</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="84581f89-eb43-43e1-a0d6-01e6a65100ed" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get TAS spouse income (if exist)</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         spouseinco0_.id as id1_90_0_,
         spouseinco0_.create_by as create_b2_90_0_,
         spouseinco0_.create_date as create_d3_90_0_,
         spouseinco0_.update_by as update_b4_90_0_,
         spouseinco0_.update_date as update_d5_90_0_,
         spouseinco0_.city_name as city_nam6_90_0_,
         spouseinco0_.country_name as country_7_90_0_,
         spouseinco0_.end_of_employment as end_of_e8_90_0_,
         spouseinco0_.income_employment as income_e9_90_0_,
         spouseinco0_.income_type as income_10_90_0_,
         spouseinco0_.partner_work_in_italy as partner11_90_0_,
         spouseinco0_.spouse_selected as spouse_12_90_0_,
         spouseinco0_.start_of_employment as start_o13_90_0_,
         spouseinco0_.work_place as work_pl14_90_0_,
         spouseinco0_.zip_code as zip_cod15_90_0_ 
     from
         tax_at_source_spouse_income spouseinco0_ 
     where
         spouseinco0_.id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ff36c833-8758-4ce2-893f-517cdc513a4c" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get TAS single parent (if exist)</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         singlepare0_.id as id1_89_0_,
         singlepare0_.create_by as create_b2_89_0_,
         singlepare0_.create_date as create_d3_89_0_,
         singlepare0_.update_by as update_b4_89_0_,
         singlepare0_.update_date as update_d5_89_0_,
         singlepare0_.concubinage_type as concubin6_89_0_,
         singlepare0_.living_with_another_person_in_same_household as living_w7_89_0_,
         singlepare0_.single_parent as single_p8_89_0_,
         singlepare0_.single_parent_type as single_p9_89_0_ 
     from
         tax_at_source_single_parent_info singlepare0_ 
     where
         singlepare0_.id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cc66244e-fd21-4fc8-8f00-f71da94afc19" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get contract temporal cost center</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         costcenter0_.contract_temporal_id as contract1_40_0_,
         costcenter0_.cost_center_id as cost_cen2_40_0_,
         costcenter1_.id as id1_44_1_,
         costcenter1_.create_by as create_b2_44_1_,
         costcenter1_.create_date as create_d3_44_1_,
         costcenter1_.update_by as update_b4_44_1_,
         costcenter1_.update_date as update_d5_44_1_,
         costcenter1_.valid_from as valid_fr6_44_1_,
         costcenter1_.valid_to as valid_to7_44_1_,
         costcenter1_.code as code8_44_1_,
         costcenter1_.uid as uid9_44_1_,
         costcenter1_.company_id as company10_44_1_,
         companyent2_.id as id1_32_2_,
         companyent2_.create_by as create_b2_32_2_,
         companyent2_.create_date as create_d3_32_2_,
         companyent2_.update_by as update_b4_32_2_,
         companyent2_.update_date as update_d5_32_2_,
         companyent2_.company_uri as company_6_32_2_,
         companyent2_.description as descript7_32_2_,
         companyent2_.has_vat_id as has_vat_8_32_2_,
         companyent2_.image_file_id as image_fi9_32_2_,
         companyent2_.language as languag10_32_2_,
         companyent2_.logo_fileid as logo_fi11_32_2_,
         companyent2_.logo_position as logo_po12_32_2_,
         companyent2_.representative_id as represe16_32_2_,
         companyent2_.sector_name as sector_13_32_2_,
         companyent2_.simplified_tax_at_source as simplif14_32_2_,
         companyent2_.wage_agreement as wage_ag15_32_2_,
         representa3_.id as id1_74_3_,
         representa3_.create_by as create_b2_74_3_,
         representa3_.create_date as create_d3_74_3_,
         representa3_.update_by as update_b4_74_3_,
         representa3_.update_date as update_d5_74_3_,
         representa3_.address as address6_74_3_,
         representa3_.company as company7_74_3_,
         representa3_.country as country8_74_3_,
         representa3_.email as email9_74_3_,
         representa3_.name as name10_74_3_,
         representa3_.phone as phone11_74_3_,
         representa3_.website as website12_74_3_,
         representa3_.zip_code as zip_cod13_74_3_ 
     from
         contract_temporal_info_cost_center costcenter0_ 
     inner join
         cost_center costcenter1_ 
             on costcenter0_.cost_center_id=costcenter1_.id 
     left outer join
         company companyent2_ 
             on costcenter1_.company_id=companyent2_.id 
     left outer join
         representative representa3_ 
             on companyent2_.representative_id=representa3_.id 
     where
         costcenter0_.contract_temporal_id=?
</code></pre>
</div>
</div>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="03d082f0-3a2d-49bc-9d54-ffd311c52055" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>Get TAS pension fund (if exist)</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: true; theme: Eclipse; collapse: true" data-theme="Eclipse"><code>     select
         pensionfun0_.id as id1_71_0_,
         pensionfun0_.create_by as create_b2_71_0_,
         pensionfun0_.create_date as create_d3_71_0_,
         pensionfun0_.update_by as update_b4_71_0_,
         pensionfun0_.update_date as update_d5_71_0_,
         pensionfun0_.bvg_annual_salary as bvg_annu6_71_0_,
         pensionfun0_.entry_pension_fund as entry_pe7_71_0_,
         pensionfun0_.insurance_no as insuranc8_71_0_,
         pensionfun0_.leaving_pension_fund as leaving_9_71_0_,
         pensionfun0_.part_time_pension_fund_percentage as part_ti10_71_0_ 
     from
         pension_fund pensionfun0_ 
     where
         pensionfun0_.id=?
</code></pre>
</div>
</div>
</div></td>
<td>It is trigger by <strong>getter</strong> method during convert from entity to model<span class="legacy-color-text-blue3">.</span></td>
<td>Should load these tax at source info along with contract temporal</td>
</tr>
<tr>
<td>Parse to JSON and response to server</td>
<td>~1000</td>
<td>Convert model to JSON</td>
<td>Many properties with null value are parse to JSON and respond to client</td>
<td>Skip properties with null values</td>
</tr>
</tbody>
</table>

</div>

# How to improve?

<span class="legacy-color-text-blue3">We can follow the proposals below (</span>**highest priority is at the top**<span class="legacy-color-text-blue3">)</span>

<div>

<table style="width: 80.1868%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th>Priority</th>
<th>Description</th>
<th>Estimate reduction(ms)</th>
<th>Status</th>
<th>Status<br />
(01.03.2021)</th>
</tr>
&#10;<tr>
<td>1</td>
<td><span class="legacy-color-text-blue3">Skip properties with null values when return JSON data to client<br />
(Could be less or more depend on number of contracts)</span></td>
<td>~400 to 500</td>
<td>MUST DO</td>
<td><span class="legacy-color-text-green2"><strong>DONE</strong></span></td>
</tr>
<tr>
<td>2</td>
<td>Load salary configuration including its salary item type at once</td>
<td><span class="legacy-color-text-blue3">~400</span></td>
<td>MUST DO</td>
<td><strong><span class="legacy-color-text-green2">DONE</span></strong></td>
</tr>
<tr>
<td>3</td>
<td>Remove EAGER fetch, s<span class="legacy-color-text-blue3">hould not load unnecessary data while loading contract</span></td>
<td>~20 for each contract</td>
<td>OPTIONAL</td>
<td><span class="legacy-color-text-green2"><strong>DONE</strong></span></td>
</tr>
<tr>
<td>4</td>
<td>Split query load person</td>
<td>~20</td>
<td>OPTIONAL</td>
<td>NOT YET</td>
</tr>
<tr>
<td>5</td>
<td>Load TAX dependent data (residence, spouse income, pension fund...) info along with tax at source</td>
<td>~5 to15 for each contract</td>
<td>OPTIONAL</td>
<td><span class="legacy-color-text-green2"><strong>DONE</strong></span></td>
</tr>
<tr>
<td>6</td>
<td>Load enough tax records that need for all contracts - <strong>HARD<br />
(depend on how many cantons and tariff codes are used in company)</strong></td>
<td>~250</td>
<td>Resolve by <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519730192/REST+API+calculate+1+employee+s+pay-slip">REST API calculate 1 employee's pay-slip</a></td>
<td><span class="legacy-color-text-green2"><strong>DONE</strong></span></td>
</tr>
<tr>
<td colspan="2">Estimate reduction time<br />
(Could be less or more depend on number of contracts)</td>
<td>~1450 to 1650</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[REST API calculate 1 employee's pay-slip]]
- [[Analyze N+1 queries for REST API calculate payslips for overview salary processing]]
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[Helios myKLARA app(luz-mobile) - API Response Performance Analysis]]
- [[Research Design architecture concept for the service to generate the Generic Interface File]]

%% ai-graph-end %%