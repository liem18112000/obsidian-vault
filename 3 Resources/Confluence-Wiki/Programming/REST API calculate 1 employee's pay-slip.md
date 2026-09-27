---
ai_hash: 03662f8fa345e135
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.56
entities: []
relevance: 0.775
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519730192/REST+API+calculate+1+employee+s+pay-slip
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: REST API calculate 1 employee's pay-slip
topic: programming
type: source
updated: 2021-01-29
---

# REST API calculate 1 employee's pay-slip

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-01-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20519730192/REST+API+calculate+1+employee+s+pay-slip)
> Relevance 0.775 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="06b254a5-3eae-48bb-aceb-829f85d4b998" macro-name="toc">

</div>

# Problem statement

The API calculate 1 employee's pay-slip had been introduced long time ago, it seemed to work perfectly in past few years.

However, data will never stop growing up, and the slowing down of pay-slip calculation is now obvious.

This document is about to analyze what happens inside the pay-slip calculation and point out things we may improve to get better performance.

# About the API

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cd5cf6b4-355c-42f4-ad50-4df458b71fe7" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**API calculate 1 employee's pay-slip**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
/luz_compensation/api/{company-tenant-id}/companies/{company-id}/contracts/{contract-id}/payslips/latest?recalculate=true&Month=MM.yyyy
```

</div>

</div>

# What happens inside?

Environment: localhost

Setup data:

- Company has around 30 employees, 3 work-places Luzern, Bern, Zug
- Test employee has tax at source, has special tax district Zug, has no retrograded pay-slips, no children, no spouses, no 13th month salary
- Calculating period: 02.2021

<span class="legacy-color-text-red2">**Please note that:**</span>

- **The estimated total time may not be equal to sum of all sub-steps**. It is because we may have some calculations not listed in the below table, some extra time taken by the EJB container, CDI container, Hibernate framework, etc.
- The estimated time for each sub-step consists of some calculation, query to DB, etc. So, never think that time is just only the query part.

<div>

<table style="width: 100.0%;">
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
<th>Main step</th>
<th>Estimated total time (ms)</th>
<th>What's happening in this step</th>
<th><p>Estimated execution time (ms)</p></th>
<th>Note</th>
<th>Proposal</th>
</tr>
&#10;<tr>
<td rowspan="3"><p>Find the latest pay-slip in requested month.</p>
<br />
<br />
</td>
<td rowspan="3">118<br />
<br />
</td>
<td><div class="content-wrapper">
<p>This query includes a join among of those following table: payslip, company, contract, employee, salary_run, salary_item.</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="4bd49e80-9b2a-4694-b490-a5d732824edc" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - get latest pay-slip</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     distinct payslipent0_.id as id1_69_0_,
     salaryrune5_.id as id1_80_1_,
     salaryitem6_.id as id1_77_2_,
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
     salaryrune5_.create_by as create_b2_80_1_,
     salaryrune5_.create_date as create_d3_80_1_,
     salaryrune5_.update_by as update_b4_80_1_,
     salaryrune5_.update_date as update_d5_80_1_,
     salaryrune5_.booking_link as booking_6_80_1_,
     salaryrune5_.company_id as company_8_80_1_,
     salaryrune5_.payment_date as payment_7_80_1_,
     salaryitem6_.create_by as create_b2_77_2_,
     salaryitem6_.create_date as create_d3_77_2_,
     salaryitem6_.update_by as update_b4_77_2_,
     salaryitem6_.update_date as update_d5_77_2_,
     salaryitem6_.final_base_value as final_ba6_77_2_,
     salaryitem6_.final_quantity as final_qu7_77_2_,
     salaryitem6_.final_rate as final_ra8_77_2_,
     salaryitem6_.final_value as final_va9_77_2_,
     salaryitem6_.from_formula_base_value as from_fo10_77_2_,
     salaryitem6_.from_formula_quantity as from_fo11_77_2_,
     salaryitem6_.from_formula_rate as from_fo12_77_2_,
     salaryitem6_.from_formula_value as from_fo13_77_2_,
     salaryitem6_.full_month_base_value as full_mo14_77_2_,
     salaryitem6_.full_month_quantity as full_mo15_77_2_,
     salaryitem6_.full_month_rate as full_mo16_77_2_,
     salaryitem6_.full_month_value as full_mo17_77_2_,
     salaryitem6_.name_modifier as name_mo18_77_2_,
     salaryitem6_.overwrite_base_value as overwri19_77_2_,
     salaryitem6_.overwrite_quantity as overwri20_77_2_,
     salaryitem6_.overwrite_rate as overwri21_77_2_,
     salaryitem6_.overwrite_value as overwri22_77_2_,
     salaryitem6_.payslip_id as payslip28_77_2_,
     salaryitem6_.reimbursement_id as reimbur29_77_2_,
     salaryitem6_.reimbursement_request_uri as reimbur23_77_2_,
     salaryitem6_.reimbursement_resource_uri as reimbur24_77_2_,
     salaryitem6_.remark as remark25_77_2_,
     salaryitem6_.salary_item_code_string as salary_26_77_2_,
     salaryitem6_.salary_item_type_id as salary_27_77_2_,
     salaryitem6_.payslip_id as payslip28_77_0__,
     salaryitem6_.id as id1_77_0__ 
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
     salary_run salaryrune5_ 
         on payslipent0_.salary_run_id=salaryrune5_.id 
 left outer join
     salary_item salaryitem6_ 
         on payslipent0_.id=salaryitem6_.payslip_id 
 where
     companyent3_.id=1 
     and contracten1_.id=35 
     and payslipent0_.period_from&gt;=? 
     and payslipent0_.period_to_exclusive&lt;=? 
     and (
         payslipent0_.id in (
             select
                 max(payslipent4_.id) 
             from
                 payslip payslipent4_ 
             group by
                 payslipent4_.contract_id ,
                 payslipent4_.period_from
         )
     )</code></pre>
</div>
</div>
</div></td>
<td>35</td>
<td><br />
</td>
<td><p>Should it join with salary_run table?</p>
<p>Should index column payslip_id for salary_item table</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Get contract corresponding to the above pay-slip involving lots of other associations</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="fb623b6d-2826-4ad7-9682-5f9d9c4f7d9e" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - Get contract</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     contracten0_.id as id1_35_0_,
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
     employeeen1_.id as id1_48_1_,
     employeeen1_.create_by as create_b2_48_1_,
     employeeen1_.create_date as create_d3_48_1_,
     employeeen1_.update_by as update_b4_48_1_,
     employeeen1_.update_date as update_d5_48_1_,
     employeeen1_.avatar_uri as avatar_u6_48_1_,
     employeeen1_.person_uri as person_u7_48_1_,
     employeeen1_.abbreviation as abbrevia8_48_1_,
     employeeen1_.company as company13_48_1_,
     employeeen1_.email_for_payslip as email_fo9_48_1_,
     employeeen1_.emergency_address as emergen10_48_1_,
     employeeen1_.employee_number as employe11_48_1_,
     employeeen1_.payment as payment14_48_1_,
     employeeen1_.is_payslip_sent_by_email as is_pays12_48_1_,
     civilinfos2_.employee_id as employe14_31_2_,
     civilinfos2_.id as id1_31_2_,
     civilinfos2_.id as id1_31_3_,
     civilinfos2_.create_by as create_b2_31_3_,
     civilinfos2_.create_date as create_d3_31_3_,
     civilinfos2_.update_by as update_b4_31_3_,
     civilinfos2_.update_date as update_d5_31_3_,
     civilinfos2_.valid_from as valid_fr6_31_3_,
     civilinfos2_.valid_to as valid_to7_31_3_,
     civilinfos2_.concubinage_type as concubin8_31_3_,
     civilinfos2_.employee_id as employe14_31_3_,
     civilinfos2_.spouse_first_name as spouse_f9_31_3_,
     civilinfos2_.spouse_last_name as spouse_10_31_3_,
     civilinfos2_.spouse_uri as spouse_11_31_3_,
     civilinfos2_.state as state12_31_3_,
     civilinfos2_.valid_date as valid_d13_31_3_,
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
     companyent3_.simplified_tax_at_source as simplif14_32_4_,
     companyent3_.wage_agreement as wage_ag15_32_4_,
     insurancec4_.company_id as company15_56_5_,
     insurancec4_.id as id1_56_5_,
     insurancec4_.id as id1_56_6_,
     insurancec4_.create_by as create_b2_56_6_,
     insurancec4_.create_date as create_d3_56_6_,
     insurancec4_.update_by as update_b4_56_6_,
     insurancec4_.update_date as update_d5_56_6_,
     insurancec4_.valid_from as valid_fr6_56_6_,
     insurancec4_.valid_to as valid_to7_56_6_,
     insurancec4_.additional_info as addition8_56_6_,
     insurancec4_.business_case as business9_56_6_,
     insurancec4_.company_id as company15_56_6_,
     insurancec4_.customer_number as custome10_56_6_,
     insurancec4_.global_insurer_id as global_11_56_6_,
     insurancec4_.insurance_policy_number as insuran12_56_6_,
     insurancec4_.insurance_policy_rate as insuran13_56_6_,
     insurancec4_.insurance_short_code as insuran14_56_6_,
     insurancec4_.insurer_id as insurer16_56_6_,
     insurerent5_.id as id1_57_7_,
     insurerent5_.create_by as create_b2_57_7_,
     insurerent5_.create_date as create_d3_57_7_,
     insurerent5_.update_by as update_b4_57_7_,
     insurerent5_.update_date as update_d5_57_7_,
     insurerent5_.canton_uri as canton_u6_57_7_,
     insurerent5_.company_id as company11_57_7_,
     insurerent5_.company_uri as company_7_57_7_,
     insurerent5_.insurer_type as insurer_8_57_7_,
     insurerent5_.insurer_name as insurer_9_57_7_,
     insurerent5_.insurer_number as insurer10_57_7_,
     companyent6_.id as id1_32_8_,
     companyent6_.create_by as create_b2_32_8_,
     companyent6_.create_date as create_d3_32_8_,
     companyent6_.update_by as update_b4_32_8_,
     companyent6_.update_date as update_d5_32_8_,
     companyent6_.company_uri as company_6_32_8_,
     companyent6_.description as descript7_32_8_,
     companyent6_.has_vat_id as has_vat_8_32_8_,
     companyent6_.image_file_id as image_fi9_32_8_,
     companyent6_.language as languag10_32_8_,
     companyent6_.logo_fileid as logo_fi11_32_8_,
     companyent6_.logo_position as logo_po12_32_8_,
     companyent6_.representative_id as represe16_32_8_,
     companyent6_.sector_name as sector_13_32_8_,
     companyent6_.simplified_tax_at_source as simplif14_32_8_,
     companyent6_.wage_agreement as wage_ag15_32_8_,
     representa7_.id as id1_74_9_,
     representa7_.create_by as create_b2_74_9_,
     representa7_.create_date as create_d3_74_9_,
     representa7_.update_by as update_b4_74_9_,
     representa7_.update_date as update_d5_74_9_,
     representa7_.address as address6_74_9_,
     representa7_.company as company7_74_9_,
     representa7_.country as country8_74_9_,
     representa7_.email as email9_74_9_,
     representa7_.name as name10_74_9_,
     representa7_.phone as phone11_74_9_,
     representa7_.website as website12_74_9_,
     representa7_.zip_code as zip_cod13_74_9_,
     workplaces8_.company_id as company11_120_10_,
     workplaces8_.id as id1_120_10_,
     workplaces8_.id as id1_120_11_,
     workplaces8_.create_by as create_b2_120_11_,
     workplaces8_.create_date as create_d3_120_11_,
     workplaces8_.update_by as update_b4_120_11_,
     workplaces8_.update_date as update_d5_120_11_,
     workplaces8_.code as code6_120_11_,
     workplaces8_.companyUri as companyU7_120_11_,
     workplaces8_.description as descript8_120_11_,
     workplaces8_.company_id as company11_120_11_,
     workplaces8_.weekly_hours as weekly_h9_120_11_,
     workplaces8_.weekly_lessons as weekly_10_120_11_,
     workplaceq9_.workplace_id as workplac9_121_12_,
     workplaceq9_.id as id1_121_12_,
     workplaceq9_.id as id1_121_13_,
     workplaceq9_.create_by as create_b2_121_13_,
     workplaceq9_.create_date as create_d3_121_13_,
     workplaceq9_.update_by as update_b4_121_13_,
     workplaceq9_.update_date as update_d5_121_13_,
     workplaceq9_.commission_rate as commissi6_121_13_,
     workplaceq9_.qst_id as qst_id7_121_13_,
     workplaceq9_.state_uri as state_ur8_121_13_,
     workplaceq9_.workplace_id as workplac9_121_13_,
     employeepa10_.id as id1_49_14_,
     employeepa10_.create_by as create_b2_49_14_,
     employeepa10_.create_date as create_d3_49_14_,
     employeepa10_.update_by as update_b4_49_14_,
     employeepa10_.update_date as update_d5_49_14_,
     employeepa10_.additional_address as addition6_49_14_,
     employeepa10_.address as address7_49_14_,
     employeepa10_.bank_account_scope as bank_acc8_49_14_,
     employeepa10_.bank_name as bank_nam9_49_14_,
     employeepa10_.bic_code as bic_cod10_49_14_,
     employeepa10_.cash_payment as cash_pa11_49_14_,
     employeepa10_.iban_number as iban_nu12_49_14_,
     employeepa10_.intended_use as intende13_49_14_,
     employeepa10_.owner_name as owner_n14_49_14_,
     employeepa10_.zip_city as zip_cit15_49_14_,
     workpermit11_.employee_id as employe10_117_15_,
     workpermit11_.id as id1_117_15_,
     workpermit11_.id as id1_117_16_,
     workpermit11_.create_by as create_b2_117_16_,
     workpermit11_.create_date as create_d3_117_16_,
     workpermit11_.update_by as update_b4_117_16_,
     workpermit11_.update_date as update_d5_117_16_,
     workpermit11_.valid_from as valid_fr6_117_16_,
     workpermit11_.valid_to as valid_to7_117_16_,
     workpermit11_.employee_id as employe10_117_16_,
     workpermit11_.withholding_tax_duty_despite_settledC as withhold8_117_16_,
     workpermit11_.work_permit as work_per9_117_16_ 
 from
     contract contracten0_ 
 left outer join
     employee employeeen1_ 
         on contracten0_.employee_id=employeeen1_.id 
 left outer join
     civil_info civilinfos2_ 
         on employeeen1_.id=civilinfos2_.employee_id 
 left outer join
     company companyent3_ 
         on employeeen1_.company=companyent3_.id 
 left outer join
     insurance_contract insurancec4_ 
         on companyent3_.id=insurancec4_.company_id 
 left outer join
     insurer insurerent5_ 
         on insurancec4_.insurer_id=insurerent5_.id 
 left outer join
     company companyent6_ 
         on insurerent5_.company_id=companyent6_.id 
 left outer join
     representative representa7_ 
         on companyent6_.representative_id=representa7_.id 
 left outer join
     workplace workplaces8_ 
         on companyent6_.id=workplaces8_.company_id 
 left outer join
     workplace_qst workplaceq9_ 
         on workplaces8_.id=workplaceq9_.workplace_id 
 left outer join
     employee_payment employeepa10_ 
         on employeeen1_.payment=employeepa10_.id 
 left outer join
     work_permit_info workpermit11_ 
         on employeeen1_.id=workpermit11_.employee_id 
 where
     contracten0_.id=?</code></pre>
</div>
</div>
</div></td>
<td>14</td>
<td>It seems to be that this call to DB is unexpected. It's triggered by the eager fetch type annotated in Contract Entity, I guessed</td>
<td><p>Should it join with employee_payment, workplace QST, insurer, representative tables?</p>
<p>Anyway, we will load it in later step. We could also remove it here</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>N + 1 queries to get workplace</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e0d84290-d339-4db1-be6c-5aa0f0e5b59d" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - Get workplace</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     workplaces0_.company_id as company11_120_0_,
     workplaces0_.id as id1_120_0_,
     workplaces0_.id as id1_120_1_,
     workplaces0_.create_by as create_b2_120_1_,
     workplaces0_.create_date as create_d3_120_1_,
     workplaces0_.update_by as update_b4_120_1_,
     workplaces0_.update_date as update_d5_120_1_,
     workplaces0_.code as code6_120_1_,
     workplaces0_.companyUri as companyU7_120_1_,
     workplaces0_.description as descript8_120_1_,
     workplaces0_.company_id as company11_120_1_,
     workplaces0_.weekly_hours as weekly_h9_120_1_,
     workplaces0_.weekly_lessons as weekly_10_120_1_ 
 from
     workplace workplaces0_ 
 where
     workplaces0_.company_id=?</code></pre>
</div>
</div>
</div></td>
<td>10</td>
<td><br />
</td>
<td><p>Load by one query</p>
<p>select *</p>
<p>from workplace</p>
<p>where company_id = ?</p></td>
</tr>
<tr>
<td rowspan="6">Get contract overview information<br />
<br />
<br />
<br />
<br />
</td>
<td rowspan="6">223<br />
<br />
<br />
<br />
<br />
</td>
<td><div class="content-wrapper">
<p>Find contract by given id</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="908627a7-bd3f-4989-82a9-ebfe62f50235" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - Find contract by id</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     distinct contracten0_.id as id1_35_0_,
     employeeen1_.id as id1_48_1_,
     contractte2_.id as id1_39_2_,
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
     employeeen1_.create_by as create_b2_48_1_,
     employeeen1_.create_date as create_d3_48_1_,
     employeeen1_.update_by as update_b4_48_1_,
     employeeen1_.update_date as update_d5_48_1_,
     employeeen1_.avatar_uri as avatar_u6_48_1_,
     employeeen1_.person_uri as person_u7_48_1_,
     employeeen1_.abbreviation as abbrevia8_48_1_,
     employeeen1_.company as company13_48_1_,
     employeeen1_.email_for_payslip as email_fo9_48_1_,
     employeeen1_.emergency_address as emergen10_48_1_,
     employeeen1_.employee_number as employe11_48_1_,
     employeeen1_.payment as payment14_48_1_,
     employeeen1_.is_payslip_sent_by_email as is_pays12_48_1_,
     contractte2_.create_by as create_b2_39_2_,
     contractte2_.create_date as create_d3_39_2_,
     contractte2_.update_by as update_b4_39_2_,
     contractte2_.update_date as update_d5_39_2_,
     contractte2_.valid_from as valid_fr6_39_2_,
     contractte2_.valid_to as valid_to7_39_2_,
     contractte2_.bvg_deduction as bvg_dedu8_39_2_,
     contractte2_.bvg_deduction_of_employer as bvg_dedu9_39_2_,
     contractte2_.contract_id as contrac21_39_2_,
     contractte2_.cost_center as cost_ce10_39_2_,
     contractte2_.employment_type as employm11_39_2_,
     contractte2_.engagement_level as engagem12_39_2_,
     contractte2_.function_title_id as functio22_39_2_,
     contractte2_.granted_rate as granted13_39_2_,
     contractte2_.hourly_salary_rate as hourly_14_39_2_,
     contractte2_.job_activity as job_act15_39_2_,
     contractte2_.manual_tax_rate as manual_16_39_2_,
     contractte2_.monthly_salary as monthly17_39_2_,
     contractte2_.pension_fund_id as pension23_39_2_,
     contractte2_.single_parent as single_18_39_2_,
     contractte2_.special_tax_city_uri as special19_39_2_,
     contractte2_.tariff_code as tariff_20_39_2_,
     contractte2_.tax_at_source_id as tax_at_24_39_2_,
     contractte2_.workplace_id as workpla25_39_2_,
     contractte2_.contract_id as contrac21_39_0__,
     contractte2_.id as id1_39_0__ 
 from
     contract contracten0_ 
 left outer join
     employee employeeen1_ 
         on contracten0_.employee_id=employeeen1_.id 
 left outer join
     contract_temporal_info contractte2_ 
         on contracten0_.id=contractte2_.contract_id 
 where
     contracten0_.id=35</code></pre>
</div>
</div>
</div></td>
<td>0.178</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Call to luz_person: /fdbfb46b-87ba-4275-8d17-ca4f4f0ecc17/companies get information about company and workplaces.</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a12db1ad-68e8-428e-942b-47b1ac3d814b" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - ContractService.getFullCompanyInfoFromContract()</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code>Select *
from company companyent0_ left outer join company_email emails1_ on companyent0_.id=emails1_.company_id left outer join email emailentit2_ on emails1_.email_id=emailentit2_.id left outer join person_email emailentit2_1_ on emailentit2_.id=emailentit2_1_.email_id left outer join company_email emailentit2_2_ on emailentit2_.id=emailentit2_2_.email_id left outer join company_address addresses3_ on companyent0_.id=addresses3_.company_id left outer join address addressent4_ on addresses3_.address_id=addressent4_.id left outer join person_address addressent4_1_ on addressent4_.id=addressent4_1_.address_id left outer join company_address addressent4_2_ on addressent4_.id=addressent4_2_.address_id left outer join public.city cityentity5_ on addressent4_.city_id=cityentity5_.id left outer join public.country countryent6_ on cityentity5_.country_id=countryent6_.id left outer join public.state stateentit7_ on cityentity5_.state_id=stateentit7_.id left outer join public.community communitye8_ on cityentity5_.community_id=communitye8_.id left outer join company_custom_field customfiel9_ on companyent0_.id=customfiel9_.company_id left outer join company_phone phones10_ on companyent0_.id=phones10_.company_id left outer join phone phoneentit11_ on phones10_.phone_id=phoneentit11_.id left outer join company_phone phoneentit11_1_ on phoneentit11_.id=phoneentit11_1_.phone_id left outer join person_phone phoneentit11_2_ on phoneentit11_.id=phoneentit11_2_.phone_id left outer join company_category categories12_ on companyent0_.id=categories12_.company_id left outer join company_online_platform onlineplat13_ on companyent0_.id=onlineplat13_.company_id where companyent0_.id in (1 , 1 , 2)</code></pre>
</div>
</div>
</div></td>
<td>50</td>
<td>In the call below, it queries company based on given id (1, 1, 2). Why do we have a duplication? The company id is also the head workplace id, so the current implementation query duplicate.</td>
<td><p>There are many associations in this SQL even we don't need all information.</p>
<p>It could be a potential problem with high volume company (10000 customers/partners with 10000 addresses, phones, emails, etc.). With my test again high volume data above, the total time consuming may increase 50-100 ms</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Convert from the Company entity of compensation and person modules to model CompanyCompensation. This data is prepared for further pay-slip calculation.</p>
<p>N +1 queries to get working day and special day for each workplace</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="cc32d230-8d6f-4156-94e8-98aac813181f" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - Get working day and special day</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     workingday0_.workplace_id as workplac9_118_0_,
     workingday0_.id as id1_118_0_,
     workingday0_.id as id1_118_1_,
     workingday0_.create_by as create_b2_118_1_,
     workingday0_.create_date as create_d3_118_1_,
     workingday0_.update_by as update_b4_118_1_,
     workingday0_.update_date as update_d5_118_1_,
     workingday0_.date_status as date_sta6_118_1_,
     workingday0_.opened as opened7_118_1_,
     workingday0_.workplace_id as workplac9_118_1_,
     workingday0_.day_of_week as day_of_w8_118_1_ 
 from
     working_day workingday0_ 
 where
     workingday0_.workplace_id=?
 &#10; select
     specialday0_.workplace_id as workpla11_84_0_,
     specialday0_.id as id1_84_0_,
     specialday0_.id as id1_84_1_,
     specialday0_.create_by as create_b2_84_1_,
     specialday0_.create_date as create_d3_84_1_,
     specialday0_.update_by as update_b4_84_1_,
     specialday0_.update_date as update_d5_84_1_,
     specialday0_.date_status as date_sta6_84_1_,
     specialday0_.opened as opened7_84_1_,
     specialday0_.workplace_id as workpla11_84_1_,
     specialday0_.from_date as from_dat8_84_1_,
     specialday0_.name as name9_84_1_,
     specialday0_.to_date as to_date10_84_1_ 
 from
     special_day specialday0_ 
 where
     specialday0_.workplace_id=?</code></pre>
</div>
</div>
</div></td>
<td>14</td>
<td><p>They are caused by following method call<br />
extract Workplaces Data From Luz Person() in CompanyService.<br />
In WorkplaceEntity, we have lazy fetch and fetch type subselect with special day, working day</p></td>
<td><p>This information seems to be loaded unexpectedly by eager fetch annotation in Workplace Entity. </p>
<p>This also has nothing to do with pay-slip calculation. We should get rid of them</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>While converting to CompanyCompensation model, the insurance contracts are also fetched</p>
<p>N + 1 issue with insurance code for each insurance contract</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="ddbeeb57-5caa-409d-a5d2-f9d2abc1c4c1" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - get insurance code by insurance contract</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     insurancec0_.insurance_contract_id as insuran18_55_0_,
     insurancec0_.id as id1_55_0_,
     insurancec0_.id as id1_55_1_,
     insurancec0_.create_by as create_b2_55_1_,
     insurancec0_.create_date as create_d3_55_1_,
     insurancec0_.update_by as update_b4_55_1_,
     insurancec0_.update_date as update_d5_55_1_,
     insurancec0_.valid_from as valid_fr6_55_1_,
     insurancec0_.valid_to as valid_to7_55_1_,
     insurancec0_.emp_rate_for_men as emp_rate8_55_1_,
     insurancec0_.emp_rate_for_women as emp_rate9_55_1_,
     insurancec0_.employer_secondary_rate as employe10_55_1_,
     insurancec0_.insurance_code_description as insuran11_55_1_,
     insurancec0_.insurance_code_string as insuran12_55_1_,
     insurancec0_.insurance_contract_id as insuran18_55_1_,
     insurancec0_.rate_for_men as rate_fo13_55_1_,
     insurancec0_.rate_for_women as rate_fo14_55_1_,
     insurancec0_.salary_item_type_code as salary_15_55_1_,
     insurancec0_.salary_range_maximum as salary_16_55_1_,
     insurancec0_.salary_range_minimum as salary_17_55_1_ 
 from
     insurance_code insurancec0_ 
 where
     insurancec0_.insurance_contract_id=?</code></pre>
</div>
</div>
</div></td>
<td>48</td>
<td>It caused by class CompanyService, method call binding General Information Of Company Compensation<br />
-&gt; insurance Contract Service .convert Entities To Models</td>
<td>This information will be used later while calculating pay-slip. However, we can load it at once</td>
</tr>
<tr>
<td><p>While converting to CompanyCompensation model, they also fetch the canton from uri in WorkplaceQSTEntity</p>
<p>Filtering request path: /cities</p></td>
<td>15</td>
<td><br />
</td>
<td>It definitely no need to load workplace QST data. Should remove this</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Next is to build up the Employee model, the employee, children (if any), spouse (if any) are loaded from DB and call to luz_person to get person information</p>
</div></td>
<td>50</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td rowspan="3"><p>SalaryCalculator</p>
<p>init data once before use</p>
<br />
<br />
</td>
<td rowspan="3">100<br />
<br />
</td>
<td><div class="content-wrapper">
<p>Load all latest pay-slips of current contract</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="4641b4d3-ac50-47e5-8917-f3aa4d49cae3" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - latest pay-slips current contract</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     distinct payslipent0_.id as id1_69_0_,
     salaryrune6_.id as id1_80_1_,
     salaryitem7_.id as id1_77_2_,
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
     salaryrune6_.create_by as create_b2_80_1_,
     salaryrune6_.create_date as create_d3_80_1_,
     salaryrune6_.update_by as update_b4_80_1_,
     salaryrune6_.update_date as update_d5_80_1_,
     salaryrune6_.booking_link as booking_6_80_1_,
     salaryrune6_.company_id as company_8_80_1_,
     salaryrune6_.payment_date as payment_7_80_1_,
     salaryitem7_.create_by as create_b2_77_2_,
     salaryitem7_.create_date as create_d3_77_2_,
     salaryitem7_.update_by as update_b4_77_2_,
     salaryitem7_.update_date as update_d5_77_2_,
     salaryitem7_.final_base_value as final_ba6_77_2_,
     salaryitem7_.final_quantity as final_qu7_77_2_,
     salaryitem7_.final_rate as final_ra8_77_2_,
     salaryitem7_.final_value as final_va9_77_2_,
     salaryitem7_.from_formula_base_value as from_fo10_77_2_,
     salaryitem7_.from_formula_quantity as from_fo11_77_2_,
     salaryitem7_.from_formula_rate as from_fo12_77_2_,
     salaryitem7_.from_formula_value as from_fo13_77_2_,
     salaryitem7_.full_month_base_value as full_mo14_77_2_,
     salaryitem7_.full_month_quantity as full_mo15_77_2_,
     salaryitem7_.full_month_rate as full_mo16_77_2_,
     salaryitem7_.full_month_value as full_mo17_77_2_,
     salaryitem7_.name_modifier as name_mo18_77_2_,
     salaryitem7_.overwrite_base_value as overwri19_77_2_,
     salaryitem7_.overwrite_quantity as overwri20_77_2_,
     salaryitem7_.overwrite_rate as overwri21_77_2_,
     salaryitem7_.overwrite_value as overwri22_77_2_,
     salaryitem7_.payslip_id as payslip28_77_2_,
     salaryitem7_.reimbursement_id as reimbur29_77_2_,
     salaryitem7_.reimbursement_request_uri as reimbur23_77_2_,
     salaryitem7_.reimbursement_resource_uri as reimbur24_77_2_,
     salaryitem7_.remark as remark25_77_2_,
     salaryitem7_.salary_item_code_string as salary_26_77_2_,
     salaryitem7_.salary_item_type_id as salary_27_77_2_,
     salaryitem7_.payslip_id as payslip28_77_0__,
     salaryitem7_.id as id1_77_0__ 
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
     salary_run salaryrune6_ 
         on payslipent0_.salary_run_id=salaryrune6_.id 
 left outer join
     salary_item salaryitem7_ 
         on payslipent0_.id=salaryitem7_.payslip_id 
 where
     companyent3_.id=1 
     and contracten1_.id=35 
     and payslipent0_.period_from&gt;=? 
     and payslipent0_.period_to_exclusive&lt;=? 
     and (
         payslipent0_.id in (
             select
                 max(payslipent4_.id) 
             from
                 payslip payslipent4_ 
             group by
                 payslipent4_.contract_id ,
                 payslipent4_.period_from ,
                 payslipent4_.state
         ) 
         or payslipent0_.id in (
             select
                 min(payslipent5_.id) 
             from
                 payslip payslipent5_ 
             where
                 payslipent5_.state=? 
             group by
                 payslipent5_.contract_id ,
                 payslipent5_.period_from ,
                 payslipent5_.state
         )
     )</code></pre>
</div>
</div>
</div></td>
<td>65</td>
<td><p><br />
</p>
<p><br />
</p></td>
<td><p>Join column to company and employee can be removed</p>
<p>The number of salary items in salary_item table will grow up quickly</p>
<p>→ should have index for payslip_id column of salary_item</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Load reimbursement for given employee</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="9cfa27f6-cae4-416e-b020-f8488deec3ec" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - load reimbursement</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     reimbursem0_.id as id1_73_,
     reimbursem0_.create_by as create_b2_73_,
     reimbursem0_.create_date as create_d3_73_,
     reimbursem0_.update_by as update_b4_73_,
     reimbursem0_.update_date as update_d5_73_,
     reimbursem0_.amount as amount6_73_,
     reimbursem0_.employee_id as employee7_73_,
     reimbursem0_.payment_month as payment_8_73_,
     reimbursem0_.reference_uri as referenc9_73_,
     reimbursem0_.reimbursement_request_uri as reimbur10_73_,
     reimbursem0_.request_type as request11_73_ 
 from
     reimbursement reimbursem0_ 
 where
     reimbursem0_.employee_id=?</code></pre>
</div>
</div>
</div></td>
<td>1</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Load terminated contracts</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="0046543a-afdf-44e1-9d82-58e9bc0d1062" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - load terminated contracts</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     contracten0_.id as id1_35_,
     contracten0_.create_by as create_b2_35_,
     contracten0_.create_date as create_d3_35_,
     contracten0_.update_by as update_b4_35_,
     contracten0_.update_date as update_d5_35_,
     contracten0_.contract_type as contract6_35_,
     contracten0_.education as educatio7_35_,
     contracten0_.employee_id as employe23_35_,
     contracten0_.employment_contract as employme8_35_,
     contracten0_.entry_date as entry_da9_35_,
     contracten0_.exit_date as exit_da10_35_,
     contracten0_.first_payment_date as first_p11_35_,
     contracten0_.first_working_day as first_w12_35_,
     contracten0_.holidays as holiday13_35_,
     contracten0_.holidays_weeks as holiday14_35_,
     contracten0_.informal_entry_date as informa15_35_,
     contracten0_.job_title as job_tit16_35_,
     contracten0_.last_working_day as last_wo17_35_,
     contracten0_.planned_work_hours as planned18_35_,
     contracten0_.planned_work_weeks as planned19_35_,
     contracten0_.position as positio20_35_,
     contracten0_.weekly_hours as weekly_21_35_,
     contracten0_.weekly_lessons as weekly_22_35_ 
 from
     contract contracten0_ 
 inner join
     employee employeeen1_ 
         on contracten0_.employee_id=employeeen1_.id 
 inner join
     company companyent2_ 
         on employeeen1_.company=companyent2_.id 
 where
     (
         employeeen1_.id in (
             34
         )
     ) 
     and companyent2_.id=1</code></pre>
</div>
</div>
</div></td>
<td>10</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td rowspan="7">Recalculate pay-slip<br />
<br />
<br />
<br />
<br />
<br />
</td>
<td rowspan="7">927<br />
<br />
<br />
<br />
<br />
<br />
</td>
<td><div class="content-wrapper">
<p>Load contract_salary_item_type_temporal, contract_salary_item_type_group_temporal, contract_salary_configuration_temporal, contract_variable for given contract</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c0353bcf-574f-4b64-9be6-93bfc7c0ce37" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - Load data for given contract</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     salaryitem0_.contract_id as contract9_38_0_,
     salaryitem0_.id as id1_38_0_,
     salaryitem0_.id as id1_38_1_,
     salaryitem0_.create_by as create_b2_38_1_,
     salaryitem0_.create_date as create_d3_38_1_,
     salaryitem0_.update_by as update_b4_38_1_,
     salaryitem0_.update_date as update_d5_38_1_,
     salaryitem0_.valid_from as valid_fr6_38_1_,
     salaryitem0_.valid_to as valid_to7_38_1_,
     salaryitem0_.contract_id as contract9_38_1_,
     salaryitem0_.salary_item_type_id as salary_i8_38_1_ 
 from
     contract_salary_item_type_temporal salaryitem0_ 
 where
     salaryitem0_.contract_id=?
&#10; 
 select
     salaryitem0_.contract_id as contract9_37_0_,
     salaryitem0_.id as id1_37_0_,
     salaryitem0_.id as id1_37_1_,
     salaryitem0_.create_by as create_b2_37_1_,
     salaryitem0_.create_date as create_d3_37_1_,
     salaryitem0_.update_by as update_b4_37_1_,
     salaryitem0_.update_date as update_d5_37_1_,
     salaryitem0_.valid_from as valid_fr6_37_1_,
     salaryitem0_.valid_to as valid_to7_37_1_,
     salaryitem0_.contract_id as contract9_37_1_,
     salaryitem0_.salary_item_type_group_id as salary_i8_37_1_ 
 from
     contract_salary_item_type_group_temporal salaryitem0_ 
 where
     salaryitem0_.contract_id=?
&#10;
 select
     salaryconf0_.contract_id as contrac10_36_0_,
     salaryconf0_.id as id1_36_0_,
     salaryconf0_.id as id1_36_1_,
     salaryconf0_.create_by as create_b2_36_1_,
     salaryconf0_.create_date as create_d3_36_1_,
     salaryconf0_.update_by as update_b4_36_1_,
     salaryconf0_.update_date as update_d5_36_1_,
     salaryconf0_.valid_from as valid_fr6_36_1_,
     salaryconf0_.valid_to as valid_to7_36_1_,
     salaryconf0_.contract_id as contrac10_36_1_,
     salaryconf0_.salary_configuration_id as salary_c8_36_1_,
     salaryconf0_.salary_configuration_variable_name as salary_c9_36_1_ 
 from
     contract_salary_configuration_temporal salaryconf0_ 
 where
     salaryconf0_.contract_id=?
&#10; 
 select
     variables0_.holder_id as holder_12_41_0_,
     variables0_.id as id1_41_0_,
     variables0_.id as id1_41_1_,
     variables0_.create_by as create_b2_41_1_,
     variables0_.create_date as create_d3_41_1_,
     variables0_.update_by as update_b4_41_1_,
     variables0_.update_date as update_d5_41_1_,
     variables0_.value as value6_41_1_,
     variables0_.data_type as data_typ7_41_1_,
     variables0_.is_final as is_final8_41_1_,
     variables0_.name as name9_41_1_,
     variables0_.valid_from as valid_f10_41_1_,
     variables0_.valid_to as valid_t11_41_1_ 
 from
     contract_variable variables0_ 
 where
     variables0_.holder_id=?
&#10;Hibernate: 
 select
     variables0_.holder_id as holder_12_122_0_,
     variables0_.id as id1_122_0_,
     variables0_.id as id1_122_1_,
     variables0_.create_by as create_b2_122_1_,
     variables0_.create_date as create_d3_122_1_,
     variables0_.update_by as update_b4_122_1_,
     variables0_.update_date as update_d5_122_1_,
     variables0_.value as value6_122_1_,
     variables0_.data_type as data_typ7_122_1_,
     variables0_.is_final as is_final8_122_1_,
     variables0_.name as name9_122_1_,
     variables0_.valid_from as valid_f10_122_1_,
     variables0_.valid_to as valid_t11_122_1_ 
 from
     workplace_variable variables0_ 
 where
     variables0_.holder_id=?
&#10;Hibernate: 
 select
     variables0_.holder_id as holder_12_34_0_,
     variables0_.id as id1_34_0_,
     variables0_.id as id1_34_1_,
     variables0_.create_by as create_b2_34_1_,
     variables0_.create_date as create_d3_34_1_,
     variables0_.update_by as update_b4_34_1_,
     variables0_.update_date as update_d5_34_1_,
     variables0_.value as value6_34_1_,
     variables0_.data_type as data_typ7_34_1_,
     variables0_.is_final as is_final8_34_1_,
     variables0_.name as name9_34_1_,
     variables0_.valid_from as valid_f10_34_1_,
     variables0_.valid_to as valid_t11_34_1_ 
 from
     company_variable variables0_ 
 where
     variables0_.holder_id=?
&#10;Hibernate: 
 select
     globalvari0_.id as id1_2_,
     globalvari0_.create_by as create_b2_2_,
     globalvari0_.create_date as create_d3_2_,
     globalvari0_.update_by as update_b4_2_,
     globalvari0_.update_date as update_d5_2_,
     globalvari0_.value as value6_2_,
     globalvari0_.data_type as data_typ7_2_,
     globalvari0_.is_final as is_final8_2_,
     globalvari0_.name as name9_2_,
     globalvari0_.valid_from as valid_f10_2_,
     globalvari0_.valid_to as valid_t11_2_ 
 from
     public.global_variable globalvari0_ 
 where
     (
         globalvari0_.valid_to is null 
         or globalvari0_.valid_to&gt;?
     ) 
     and (
         globalvari0_.valid_from is null 
         or globalvari0_.valid_from&lt;?
     )</code></pre>
</div>
</div>
<p>Load workplace_variable by current workplace, company_variable by current company, global_variable</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="64174e88-e646-48d1-8f29-164b69accfc0" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - load variables for workplace, company, global</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
     variables0_.holder_id as holder_12_122_0_,
     variables0_.id as id1_122_0_,
     variables0_.id as id1_122_1_,
     variables0_.create_by as create_b2_122_1_,
     variables0_.create_date as create_d3_122_1_,
     variables0_.update_by as update_b4_122_1_,
     variables0_.update_date as update_d5_122_1_,
     variables0_.value as value6_122_1_,
     variables0_.data_type as data_typ7_122_1_,
     variables0_.is_final as is_final8_122_1_,
     variables0_.name as name9_122_1_,
     variables0_.valid_from as valid_f10_122_1_,
     variables0_.valid_to as valid_t11_122_1_ 
 from
     workplace_variable variables0_ 
 where
     variables0_.holder_id=?
&#10;
 select
     variables0_.holder_id as holder_12_34_0_,
     variables0_.id as id1_34_0_,
     variables0_.id as id1_34_1_,
     variables0_.create_by as create_b2_34_1_,
     variables0_.create_date as create_d3_34_1_,
     variables0_.update_by as update_b4_34_1_,
     variables0_.update_date as update_d5_34_1_,
     variables0_.value as value6_34_1_,
     variables0_.data_type as data_typ7_34_1_,
     variables0_.is_final as is_final8_34_1_,
     variables0_.name as name9_34_1_,
     variables0_.valid_from as valid_f10_34_1_,
     variables0_.valid_to as valid_t11_34_1_ 
 from
     company_variable variables0_ 
 where
     variables0_.holder_id=?
&#10;
 select
     globalvari0_.id as id1_2_,
     globalvari0_.create_by as create_b2_2_,
     globalvari0_.create_date as create_d3_2_,
     globalvari0_.update_by as update_b4_2_,
     globalvari0_.update_date as update_d5_2_,
     globalvari0_.value as value6_2_,
     globalvari0_.data_type as data_typ7_2_,
     globalvari0_.is_final as is_final8_2_,
     globalvari0_.name as name9_2_,
     globalvari0_.valid_from as valid_f10_2_,
     globalvari0_.valid_to as valid_t11_2_ 
 from
     public.global_variable globalvari0_ 
 where
     (
         globalvari0_.valid_to is null 
         or globalvari0_.valid_to&gt;?
     ) 
     and (
         globalvari0_.valid_from is null 
         or globalvari0_.valid_from&lt;?
     )</code></pre>
</div>
</div>
</div></td>
<td>30</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Load salary_configuration with salary_configuration_variable, salary_item_type_salary_configuration, salary_item_type, salary_item_type_group_salary_configuration and salary_item_type_group from public schema</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e6b07f09-869a-4362-a883-d0412c829820" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - load SIT from public</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
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
         on salaryitem4_.salary_item_type_group_id=salaryitem5_.id</code></pre>
</div>
</div>
</div></td>
<td>6</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>N + 1 queries to load salary_item_type_i18n from given salary_item_type_id. The number of iterations is equal to the number of salary item types we have</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a62277c5-3120-4fbe-9854-36855cafd77b" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - load salary_item_type_i18n</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code> select
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
     i18n0_.salary_item_type_id=?</code></pre>
</div>
</div>
</div></td>
<td>400</td>
<td>It caused by equals and hashCode method</td>
<td>Get rid of this unexpected things. We anyway load them later by issuing another SQL query to load all at once</td>
</tr>
<tr>
<td><p>Load all salary_item_type, load salary_item_type_i18n by given salary item type id, salary_item_type_variable</p></td>
<td>20</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>If employee has tax at source, also preload tax records (from public schema) with given canton, tariff code and validity.</p>
<p>This also triggers to load tax_at_source, tax_at_source_residence tables.</p>
<p>If there has defined special tax district, one API call to luz_person to get canton from special tax district</p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="7c4270cd-3b02-4905-8327-fcacc746d1af" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom" style="border-bottom-width: 1px;">
<strong>SQL - load tax records</strong><span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: sql; gutter: true; theme: DJango; collapse: true" data-theme="DJango"><code>   select
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
&#10; select
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
&#10; select
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
             ? , ? , ?
         )
     ) 
     and (
         taxrecorde0_.tariff in (
             ?
         )
     ) 
 order by
     taxrecorde0_.taxable_income desc
 &#10; select
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
             ? , ? , ?
         )
     ) 
     and (
         taxrecorde0_.tariff in (
             ?
         )
     ) 
 order by
     taxrecorde0_.taxable_income desc</code></pre>
</div>
</div>
</div></td>
<td>216</td>
<td><p>In my setup data, there are 3 cantons, 1 tariff B0N need to be preloaded in tax record. (50ms)</p>
<p>This is very strange that it runs twice, even though given same canton, tariff code and validity. (50ms x 2 = 100ms)</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p></td>
<td><p>The tax record table in public schema has milions of records. For current query, with each canton, we load at least 3000 records to memory. With such huge amount of loading records, it also slows down the performance.</p>
<p>Instead, we can find exactly the tax record matching our criteria. This only takes 1-2ms.</p>
<p>select *<br />
from public.tax_record taxrecorde0_<br />
where (taxrecorde0_.canton in ('BE' , 'AG' , 'SZ'))<br />
and (taxrecorde0_.tariff in ('B0N'))<br />
and taxrecorde0_.valid_from &lt;= '2019-06-01'<br />
and taxrecorde0_.valid_to is null<br />
and taxable_income &lt; 9900.00<br />
order by taxrecorde0_.taxable_income desc<br />
limit 1</p>
<p>We can also cache this result, so if further call with same parameters, we don't need to hit the DB again</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>Calculate all salary items, one by one (using GroovyClassLoader to run the formula).</p>
<p>During calculation, it also loaded canton_configuration from public schema, third_party_receiver, tax_at_source, tax_at_source_residence, fak_allocation, representative, employee_payment, insurance_allocation</p>
<p>Note that: in my setup data, all pay-slip before current month are in state of SEALED. It means only current pay-slip is re-calculating</p>
</div></td>
<td>180</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>Update new value of salary items to DB by persisting PayslipEntity</td>
<td>5</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

  

**Summary**: estimated total execution time of whole API is 1468ms

# How to improve?

We can follow the proposals below (**highest priority is at the top**)

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
<th>Priority</th>
<th>Description</th>
<th>Estimated time reduction (ms)</th>
<th>Action</th>
</tr>
&#10;<tr>
<td><span class="legacy-color-text-blue1">1</span></td>
<td><p>Remove N + 1 queries to load salary_item_type_i18n from given salary_item_type_id</p></td>
<td>400</td>
<td>MUST DO</td>
</tr>
<tr>
<td><span class="legacy-color-text-blue1">2</span></td>
<td>Load tax record directly from DB instead of loading to memory</td>
<td>200</td>
<td>MUST DO</td>
</tr>
<tr>
<td>3</td>
<td>Index for column payslip_id in salary_item table</td>
<td>50</td>
<td>HAVE TO</td>
</tr>
<tr>
<td>4</td>
<td><p>Remove N + 1 issue with insurance code for each insurance contract</p></td>
<td>40</td>
<td>HAVE TO</td>
</tr>
<tr>
<td>5</td>
<td>Should not load state from Workplace QST</td>
<td>15</td>
<td>OPTIONAL</td>
</tr>
<tr>
<td>6</td>
<td>Remove N + 1 issue loading workplace by company id</td>
<td>10</td>
<td>OPTIONAL</td>
</tr>
<tr>
<td>7</td>
<td>Should not load unnecessary data while loading contract</td>
<td>10</td>
<td>OPTIONAL</td>
</tr>
<tr>
<td colspan="2">Estimated total time reduction</td>
<td>725</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

**Updated 29.01.2021**

After finishing 4 points (1,2,3,4), the API response time is currently 650ms (with same test environment above)

%% ai-graph-start %%

**Related notes:**
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[Analyze N+1 queries for REST API calculate payslip for 1 employee]]
- [[Analyze N+1 queries for REST API calculate payslips for overview salary processing]]
- [[Helios myKLARA app(luz-mobile) - API Response Performance Analysis]]
- [[Research Design architecture concept for the service to generate the Generic Interface File]]

%% ai-graph-end %%