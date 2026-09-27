---
ai_hash: 82cc7caf5139e10a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 2.81
entities: []
relevance: 0.755
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47489876096/SQL+script+for+populating+master+data
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: SQL script for populating master data
topic: programming
type: source
updated: 2023-12-14
---

# SQL script for populating master data

> [!info] Imported from Confluence
> Space **HACKA** · updated 2023-12-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47489876096/SQL+script+for+populating+master+data)
> Relevance 0.755 · topic `programming`

Database: **luzcompensation**.

- <span class="placeholder-inline-tasks">Tenant specific.</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f293ad6e-fdbf-4a3e-88d4-c0c9b0832854" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
INSERT INTO {schema}.accounting_interface_accounting_pattern (create_by,create_date,update_by,update_date,valid_from,valid_to,accounting_pattern_description,accounting_pattern_key,accounting_level) VALUES
     (NULL,NULL,NULL,NULL,NULL,NULL,'Test Accounting Pattern Level ACC','AP_3','ACC'),
     (NULL,NULL,NULL,NULL,NULL,NULL,'Test Accounting Pattern Level ACC_CC','AP_2','ACC_CC'),
     (NULL,NULL,NULL,NULL,NULL,NULL,'Test Accounting Pattern Level ACC_CC_CT','AP_1','ACC_CC_CT');
    
INSERT INTO {schema}.accounting_interface_default_accounting_pattern (create_by,create_date,update_by,update_date,accounting_pattern_key,from_date) VALUES
     (NULL,NULL,NULL,NULL,'AP_1','1900-01-01');

INSERT INTO {schema}.accounting_interface_account (create_by,create_date,update_by,update_date,account_description,account_number,account_number_key,valid_from,valid_to,accounting_pattern_key) VALUES
     (NULL,NULL,NULL,NULL,'Salary Account Test DR Accounting Pattern AP_2','1234 (Account Number Salary DR)','ACC_SALARY_DR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Salary Account Test CR Accounting Pattern AP_2','4321 (Account Number Salary CR)','ACC_SALARY_CR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Insurance Account Test CR Accounting Pattern AP_2','4321 (Account Number Insurance CR)','ACC_INSURANCE_CR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Insurance Account Test DR Accounting Pattern AP_2','1234 (Account Number Insurance DR)','ACC_INSURANCE_DR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Salary Account Test DR Accounting Pattern AP_3','1234 (Account Number Salary DR)','ACC_SALARY_DR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Salary Account Test CR Accounting Pattern AP_3','4321 (Account Number Salary CR)','ACC_SALARY_CR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Insurance Account Test CR Accounting Pattern AP_3','4321 (Account Number Insurance CR)','ACC_INSURANCE_CR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Insurance Account Test DR Accounting Pattern AP_3','1234 (Account Number Insurance DR)','ACC_INSURANCE_DR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Salary Account Test CR Accounting Pattern AP_1','4321 (Account Number Salary CR)','ACC_SALARY_CR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Account Test CR Accounting Pattern AP_1','4321 (Account Number Insurance CR)','ACC_INSURANCE_CR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Account Test DR Accounting Pattern AP_1','1234 (Account Number Insurance DR)','ACC_INSURANCE_DR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'SIT Overwriting Account DR','9999 (Account Number SIT Overwriting DR)','ACC_SIT_OVERWRITING_DR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Salary Account Test DR Accounting Pattern Valid To 30.09.2020 AP_1','1234 (Account Number Salary DR)','ACC_SALARY_DR',NULL,'2020-09-30','AP_1'),
     (NULL,NULL,NULL,NULL,'Salary Account Test DR Accounting Pattern Valid From 01.10.2020 AP_1','1234 (Account Number Salary DR)','ACC_SALARY_DR','2020-10-01',NULL,'AP_1');
     
INSERT INTO {schema}.accounting_interface_cost_center (create_by,create_date,update_by,update_date,cost_center_description,cost_center,cost_center_key,valid_from,valid_to,accounting_pattern_key) VALUES
     (NULL,NULL,NULL,NULL,'Salary Cost Center Test CR Accounting Pattern AP_2','Cost Center Salary CR','CC_SALARY_CR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Salary Cost Center Test DR Accounting Pattern AP_2','Cost Center Salary DR','CC_SALARY_DR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test DR Accounting Pattern AP_2','Cost Center Insurance DR','CC_INSURANCE_DR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test CR Accounting Pattern AP_2','Cost Center Insurance CR','CC_INSURANCE_CR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Salary Cost Center Test CR Accounting Pattern AP_3','Cost Center Salary CR','CC_SALARY_CR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Salary Cost Center Test DR Accounting Pattern AP_3','Cost Center Salary DR','CC_SALARY_DR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test DR Accounting Pattern AP_3','Cost Center Insurance DR','CC_INSURANCE_DR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test CR Accounting Pattern AP_3','Cost Center Insurance CR','CC_INSURANCE_CR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Salary Cost Center Test CR Accounting Pattern AP_1','Cost Center Salary CR','CC_SALARY_CR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Salary Cost Center Test DR Accounting Pattern AP_1','Cost Center Salary DR','CC_SALARY_DR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test DR Accounting Pattern AP_1','Cost Center Insurance DR','CC_INSURANCE_DR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test CR Accounting Pattern Valid From 01.10.2020 AP_1','Cost Center Insurance CR','CC_INSURANCE_CR','2020-10-01',NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Center Test CR Accounting Pattern Valid Until 30.09.2020 AP_1','Cost Center Insurance CR','CC_INSURANCE_CR',NULL,'2020-09-30','AP_1');
     
INSERT INTO {schema}.accounting_interface_cost_type (create_by,create_date,update_by,update_date,cost_type,cost_type_description,cost_type_key,valid_from,valid_to,accounting_pattern_key) VALUES
     (NULL,NULL,NULL,NULL,'Salary Cost Type Test CR','Salary Cost Type CR Accounting Pattern AP_2','CT_SALARY_CR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Salary Cost Type Test DR','Salary Cost Type DR Accounting Pattern AP_2','CT_SALARY_DR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test DR','Insurance Cost Type DR Accounting Pattern AP_2','CT_INSURANCE_DR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test CR','Insurance Cost Type CR Accounting Pattern AP_2','CT_INSURANCE_CR',NULL,NULL,'AP_2'),
     (NULL,NULL,NULL,NULL,'Salary Cost Type Test CR','Salary Cost Type CR Accounting Pattern AP_3','CT_SALARY_CR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Salary Cost Type Test DR','Salary Cost Type DR Accounting Pattern AP_3','CT_SALARY_DR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test DR','Insurance Cost Type DR Accounting Pattern AP_3','CT_INSURANCE_DR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test CR','Insurance Cost Type CR Accounting Pattern AP_3','CT_INSURANCE_CR',NULL,NULL,'AP_3'),
     (NULL,NULL,NULL,NULL,'Salary Cost Type Test CR','Salary Cost Type CR Accounting Pattern AP_1','CT_SALARY_CR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Salary Cost Type Test DR','Salary Cost Type DR Accounting Pattern AP_1','CT_SALARY_DR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test DR','Insurance Cost Type DR Accounting Pattern AP_1','CT_INSURANCE_DR',NULL,NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test CR','Insurance Cost Type CR Accounting Pattern Valid From 01.10.2020 AP_1','CT_INSURANCE_CR','2020-10-01',NULL,'AP_1'),
     (NULL,NULL,NULL,NULL,'Insurance Cost Type Test CR','Insurance Cost Type CR Accounting Pattern Valid Until 30.09.2020 AP_1','CT_INSURANCE_CR',NULL,'2020-09-30','AP_1');
    
INSERT INTO {schema}.accounting_interface_booking_rule (create_by,create_date,update_by,update_date,booking_rule_key,accounting_group_code,booking_note,booking_rule_description,account_usage_cr,account_usage_dr,vat_code_cr,vat_code_dr,account_number_key_cr,account_number_key_dr,cost_center_usage_cr,cost_center_usage_dr,cost_center_key_cr,cost_center_key_dr,cost_type_usage_cr,cost_type_usage_dr,cost_type_key_cr,cost_type_key_dr,reversal_sign,booking_allowed,accounting_pattern_key,valid_from,valid_to,using_key,building_key) VALUES
     (NULL,NULL,NULL,NULL,'BOOKING_NOT_ALLOWED','DEDUCTION_SI',NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,false,false,'AP_1',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'INSURANCE','SOCIAL_INSURANCE','Test Booking Note Social Insurance','Test Booking Rule Description Social Insurance Accounting Pattern AP_1','YES','YES',NULL,NULL,'ACC_INSURANCE_CR','ACC_INSURANCE_DR','YES','YES','CC_INSURANCE_CR','CC_INSURANCE_DR','YES','YES','CT_INSURANCE_CR','CT_INSURANCE_DR',true,true,'AP_1',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'SALARY','GROSS_SALARY','Test Booking Note Gross Salary','Test Booking Rule Description Gross Salary Accounting Pattern AP_2','YES','YES','VAT Code CR Test','VAT Code DR Test','ACC_SALARY_CR','ACC_SALARY_DR','YES','YES','CC_SALARY_CR','CC_SALARY_DR','YES','YES','CT_SALARY_CR','CT_SALARY_DR',false,true,'AP_2',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'INSURANCE','SOCIAL_INSURANCE','Test Booking Note Social Insurance','Test Booking Rule Description Social Insurance Accounting Pattern AP_2','YES','YES',NULL,NULL,'ACC_INSURANCE_CR','ACC_INSURANCE_DR','YES','YES','CC_INSURANCE_CR','CC_INSURANCE_DR','YES','YES','CT_INSURANCE_CR','CT_INSURANCE_DR',true,true,'AP_2',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'SALARY','GROSS_SALARY','Test Booking Note Gross Salary','Test Booking Rule Description Gross Salary Accounting Pattern AP_3','YES','YES','VAT Code CR Test','VAT Code DR Test','ACC_SALARY_CR','ACC_SALARY_DR','YES','YES','CC_SALARY_CR','CC_SALARY_DR','YES','YES','CT_SALARY_CR','CT_SALARY_DR',false,true,'AP_3',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'INSURANCE','SOCIAL_INSURANCE','Test Booking Note Social Insurance','Test Booking Rule Description Social Insurance Accounting Pattern AP_3','YES','YES',NULL,NULL,'ACC_INSURANCE_CR','ACC_INSURANCE_DR','YES','YES','CC_INSURANCE_CR','CC_INSURANCE_DR','YES','YES','CT_INSURANCE_CR','CT_INSURANCE_DR',true,true,'AP_3',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'BR_SIT_OVERWRITING',NULL,'Booking Note SIT Overwriting','SIT Overwriting Booking Rule AP_1','NO','YES',NULL,'VAT SIT Overwriting',NULL,'ACC_SIT_OVERWRITING_DR','NO','NO',NULL,NULL,'NO','NO',NULL,NULL,false,true,'AP_1',NULL,NULL,NULL,NULL),
     (NULL,NULL,NULL,NULL,'SALARY','GROSS_SALARY','Test Booking Note Gross Salary','Test Booking Rule Description Gross Salary Valid Until 30.09.2020 Accounting Pattern AP_1','YES','YES','VAT Code CR Test','VAT Code DR Test','ACC_SALARY_CR','ACC_SALARY_DR','YES','YES','CC_SALARY_CR','CC_SALARY_DR','YES','YES','CT_SALARY_CR','CT_SALARY_DR',false,true,'AP_1',NULL,'2020-09-30',NULL,NULL),
     (NULL,NULL,NULL,NULL,'SALARY','GROSS_SALARY','Test Booking Note Gross Salary','Test Booking Rule Description Gross Salary Valid From 01.10.2020 Accounting Pattern AP_1','YES','YES','VAT Code CR Test','VAT Code DR Test','ACC_SALARY_CR','ACC_SALARY_DR','YES','YES','CC_SALARY_CR','CC_SALARY_DR','YES','YES','CT_SALARY_CR','CT_SALARY_DR',false,true,'AP_1','2020-10-01',NULL,NULL,NULL);
     
INSERT INTO {schema}.accounting_interface_sit_accounting_information (create_by,create_date,update_by,update_date,valid_from,valid_to,sit_overwriting_key,sit_overwriting_description,sit_code,booking_rule_key,accounting_pattern_key) VALUES
     (NULL,NULL,NULL,NULL,NULL,NULL,'SIT_1971','SIT Overwriting Code 1971 Accounting Pattern AP_1','1971','BR_SIT_OVERWRITING','AP_1');
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Generic Interface JSON file]]
- [[LUZ-102459 Implement physical delete for INDIVIDUAL tenant]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[14. Create companies by tenant id]]
- [[Script to list all the information of the tenants]]

%% ai-graph-end %%