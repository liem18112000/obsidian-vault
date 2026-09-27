---
ai_hash: f0fcaa51acbd8087
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 11
depth: 2.73
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47453143184/Generic+Interface+JSON+file
space: HACKA
status: reference
tags:
- confluence
- programming
- space/hacka
title: Generic Interface JSON file
topic: programming
type: source
updated: 2024-02-20
---

# Generic Interface JSON file

> [!info] Imported from Confluence
> Space **HACKA** · updated 2024-02-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/HACKA/pages/47453143184/Generic+Interface+JSON+file)
> Relevance 0.731 · topic `programming`

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="5fb962bd-5f74-4f14-b8ce-bde459de318d" macro-name="view-file"><a href="../_attachments/47453143184-generic_interface_example.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47453143184/generic_interface_example.json?version=4&amp;modificationDate=1692842646561&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47453143184-generic_interface_example.json]]

</a></span> <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="584aee36-a311-44a0-8478-68b5316388f8" macro-name="view-file"><a href="../_attachments/47453143184-generic_interface.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47453143184/generic_interface.json?version=10&amp;modificationDate=1692842646583&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47453143184-generic_interface.json]]

</a></span> Payroll data will be exported to various accounting systems in the future, so we need to create a generic interface file in JSON format that contain information can be used in a later step.

### Rough structure:


![[47453143184-image-20230809-083103.png]]



### JSON File

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="02cedadf-ec12-4ac9-9346-a61c91e8361f" macro-name="view-file"><a href="../_attachments/47453143184-generic_interface_example.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47453143184/generic_interface_example.json?version=4&amp;modificationDate=1692842646561&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47453143184-generic_interface_example.json]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="56b9bacc-a9a5-4ae6-a1bf-aff514e3139a" macro-name="view-file"><a href="../_attachments/47453143184-generic_interface.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47453143184/generic_interface.json?version=10&amp;modificationDate=1692842646583&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47453143184-generic_interface.json]]

</a></span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6d1e2ae4-9fe8-4ed1-8c03-00720a1506b2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "create_date": "String",
    "create_by": "String",
    "company": {
        "id": "Long",
        "name": "String"
    },
    "payslips": [
        {
            "id": "Long",
            "state": "PayslipState",
            "salaryRunId": "Long",
            "contract": {
                "id": "Long",
                "employee": {
                    "id": "Long",
                    "firstName": "String",
                    "lastName": "String",
                    "employeeNumber": "String"
                },
                "status": "ContractStatus",
                "contractType": "ContractType"
            },
            "periodFrom": "String",
            "periodToExclusive": "String",
            "salaryItems": [
                {
                    "id": "Long",
                    "code": "String",
                    "name": "String",
                    "remark": "String",
                    "description": "String",
                    "nameModifier": "String",
                    "paySlipGroup": "Integer",
                    "payingOutSit": "Integer",
                    "paymentTypeSit": "PaymentType",
                    "validFrom": "String",
                    "validTo": "String",
                    "value": "BigDecimal",
                    "baseValue": "BigDecimal",
                    "quantity": "BigDecimal",
                    "rate": "BigDecimal",
                    "salaryItemTypeId": "Long",
                    "accountingGroup": "String",
                    "bookingRuleDescription": "String",
                    "accountNumberDR": "String",
                    "accountDescriptionDR": "String",
                    "vatCodeDR": "String",
                    "accountNumberCR": "String",
                    "accountDescriptionCR": "String",
                    "vatCodeCR": "String",
                    "bookingNote": "String"
                    "costCenterDR": "String",
                    "costCenterDescriptionDR": "String",
                    "costCenterCR": "String",
                    "costCenterDescriptionCR": "String",
                    "costTypeDR": "String",
                    "costTypeDescriptionDR": "String",
                    "costTypeCR": "String",
                    "costTypeDescriptionCR": "String"
                }
            ],
            "sealedTime": "String",
            "createDate": "String"
        }
    ],
    "salaryRun": {
        "id": "Long",
        "paymentDate": "String",
        "bookingLink": "String",
        "state": "SalaryRunState"
    }
}
```

</div>

</div>

Date example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b98138ad-f89d-44c4-88cd-556628be7883" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "create_date": "2023-05-01T00:00:00Z",
    "create_by": "hcmc-hacka@axonactive.com",
    "company": {
        "id": 1,
        "name": "Hitz Accounting"
    },
    "payslips": [
        {
            "id": 6,
            "state": "CALCULATED",
            "salaryRunId": 3,
            "contract": {
                "id": 2,
                "employee": {
                    "id": 2,
                    "firstName": "Thanh",
                    "lastName": "Nguyen",
                    "employeeNumber": "2"
                },
                "status": "ACTIVE",
                "contractType": "TEMPORAL_CONTRACT"
            },
            "periodFrom": "2023-05-01T00:00:00Z",
            "periodToExclusive": "2023-06-01T00:00:00Z",
            "salaryItems": [
                {
                    "id": 1195,
                    "code": "5060",
                    "name": "QST_DEDUCTION",
                    "remark": null,
                    "description": "qst deduction",
                    "nameModifier": null,
                    "paySlipGroup": 3,
                    "payingOutSit": null,
                    "paymentTypeSit": null,
                    "validFrom": null,
                    "validTo": null,
                    "value": 0.00,
                    "baseValue": null,
                    "quantity": null,
                    "rate": null,
                    "salaryItemTypeId": 90,
                    "accountingGroup": "DEDUCTION_SI_QST"
                },
                {
                    "id": 1097,
                    "code": "5033",
                    "name": "AG_FAK_BEITRAG",
                    "remark": null,
                    "description": "ag fak beitrag",
                    "nameModifier": null,
                    "paySlipGroup": 3,
                    "payingOutSit": null,
                    "paymentTypeSit": null,
                    "validFrom": null,
                    "validTo": null,
                    "value": 100.00,
                    "baseValue": 10000.00,
                    "quantity": null,
                    "rate": 0.010000000,
                    "salaryItemTypeId": 133,
                    "accountingGroup": "FAK_ACCOUNTING"
                    "bookingRuleDescription": "Rule 1",
                    "accountNumberDR": "123",
                    "accountDescriptionDR": "Test DR",
                    "vatCodeDR": "abc",
                    "accountNumberCR": "456",
                    "accountDescriptionCR": "Test CR",
                    "vatCodeCR": "def",
                    "bookingNote": "Testing",
                    "costCenterDR": "Test DR",
                    "costCenterDescriptionDR": "Test DR",
                    "costCenterCR": "Test CR",
                    "costCenterDescriptionCR": "Test CR",
                    "costTypeDR": "Test DR",
                    "costTypeDescriptionDR": "Test DR",
                    "costTypeCR": "Test CR",
                    "costTypeDescriptionCR": "Test CR"
                }
            ],
            "sealedTime": "2023-08-04T01:23:45Z",
            "createDate": "2023-04-27T09:31:23Z"
        }
    ],
    "salaryRun": {
        "id": 3,
        "paymentDate": "2023-08-04T00:00:00Z",
        "bookingLink": "/luz_accounting/api/ea1190d1-2148-4c13-82f0-3101f0f7ca53/companies/1/bookings/57",
        "state": "FINISHED"
    }
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Research Design architecture concept for the service to generate the Generic Interface File]]
- [[SQL script for populating master data]]
- [[Analyze performance for REST API calculate payslips for overview salary processing]]
- [[How to call generic interface document API on dev]]
- [[APF Provided Bookings]]

%% ai-graph-end %%