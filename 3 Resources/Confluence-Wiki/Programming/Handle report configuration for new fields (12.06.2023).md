---
title: "Handle report configuration for new fields (12.06.2023)"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47401730049/Handle+report+configuration+for+new+fields+12.06.2023
space: "LUZ"
topic: programming
relevance: 0.731
depth: 2.73
updated: 2023-06-15
attachments: 30
tags:
  - confluence
  - programming
  - space/luz
---

# Handle report configuration for new fields (12.06.2023)

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-06-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47401730049/Handle+report+configuration+for+new+fields+12.06.2023)
> Relevance 0.731 · topic `programming`

# **Problem statement**

When a team adds new table or fields to a service which is part of reporting then these fields should be considered automatically

1.  Currently, the entityAttribute and entityName is hardcode in java class of luz_report_service


![[47401730049-image-20230615-024630.png]]



2\. In code of luz_compensation we need to repeat the flow of query table


![[47401730049-image-20230614-090929.png]]



# **Configurations by file:**


![[47401730049-Config normal case.png]]



We are hard coding in luz_report_service for first step implementations. Now, we want to move all the config to file for entity and model of luz_abc_service(…).

Basicly, luz abc will receive a list of report_predicate from luz_report_service:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0fa639ee-fb03-423b-b438-023512492418" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "offset": 10,
    "limit": 10,
    "predicates": [
        {
            "reportAttributeName": "additionalAddress",
            "entityAttributeName": "additionalAddress",
            "entityName": "AddressEntity"
        }
    ]
}
```

</div>

</div>

In here the **entityAttributeName** and **entityName** will be give from luz_report_service so we need a file config for that in report service. Base on that., we need one more file to config for luz module.

So base on config file, we have:

1.  Database

2.  YAML

3.  Properties

4.  JSON

5.  XML

# Solutions to store configuration

## **1. Database:**

<div id="expander-1661740983" class="expand-container conf-macro output-block" hasbody="true" macro-id="94379958-740a-49b3-a757-57569fc5e0c3" macro-name="expand">

<div id="expander-control-1661740983" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1661740983" class="expand-content expand-hidden">

CREATE TABLE module_configuration (  
module VARCHAR(255) PRIMARY KEY,  
entityName VARCHAR(255) NOT NULL,  
entityAttributeName VARCHAR(255) NOT NULL,  
);

INSERT INTO module_configuration VALUES ('CRM', ‘CustomerEntity', 'priceCategory,discount,termsOfPayment’);  
INSERT INTO module_configuration VALUES ('CRM', ‘EmployeeEntity', 'emergencyAddress,emailForPayslip,epostActivated’);

</div>

</div>

## **2. YAML file**

<div id="expander-1013532651" class="expand-container conf-macro output-block" hasbody="true" macro-id="7ee40235-f585-4175-ba27-ed0b709ddc6a" macro-name="expand">

<div id="expander-control-1013532651" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1013532651" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aecb7a38-102d-47c9-9d06-185da6e23d7b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
module:
    moduleName : CRM
    entities :
        - CustomerEntity
            - priceCategory
            - discount
            - termsOfPayment
    moduleName : EMPLOYEE
    entities :
        - EmployeeEntity
            - emergencyAddress
            - emailForPayslip
            - epostActivated
```

</div>

</div>

</div>

</div>

## **3. Properties file**

<div id="expander-1212195320" class="expand-container conf-macro output-block" hasbody="true" macro-id="4d09c62e-a36a-4a96-9899-4d14ac46018b" macro-name="expand">

<div id="expander-control-1212195320" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1212195320" class="expand-content expand-hidden">

Using propertie files. For example: case CRM and EMPLOYEE

**module_config.properties**

CRM: crm_configuration

employee: employee_configuration

**employee_configuration.properties**

EmployeeEntity : emergencyAddress;emailForPayslip;epostActivated etc..

ContractEntity: entryDate;exitDate, etc…

**crm_configuration.properties**

CustomerEntity : priceCategory **;** discount **;** termsOfPayment etc …

</div>

</div>

## **4. JSON file**

<div id="expander-2011442123" class="expand-container conf-macro output-block" hasbody="true" macro-id="915680cd-5384-460b-bae6-9c4fc1157f50" macro-name="expand">

<div id="expander-control-2011442123" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-2011442123" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="476965b4-8d8b-453d-b49c-f6f869e8616d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
    {
        "moduleName": "CRM",
        "entities": [
            {
                "name": "CustomerEntity",
                "value": [
                    "priceCategory",
                    "discount",
                    "termsOfPayment"
                ]
            }
        ]
    },
    {
        "moduleName": "EMPLOPYEE",
        "entities": [
            {
                "name": "EmployeeEntity",
                "value": [
                    "emergencyAddress",
                    "emailForPayslip",
                    "epostActivated"
                ]
            },
             {
                "name": "ContractEntity",
                "value": [
                    "entryDate",
                    "exitDate"
                ]
            }
        ]
    }
]
```

</div>

</div>

</div>

</div>

## **5. XML file**

<div id="expander-234568955" class="expand-container conf-macro output-block" hasbody="true" macro-id="86fa635d-3680-4c97-b5a7-ca113aa15841" macro-name="expand">

<div id="expander-control-234568955" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-234568955" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="affa7234-8070-4865-9c1a-55dedf1cbc29" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<modules>
    <module index = "1" name = "CRM">
        <entities>
            <entity index = "1" name = "CustomerEntity">
                priceCategory,discount,termsOfPayment
            </entity>
        </entities>
    </module>
     <module index = "2" name = "EMPLOYEE">
        <entities>
            <entity index = "1" name = "EmployeeEntity">
                emergencyAddress,emailForPayslip,epostActivated
            </entity>
        </entities>
    </module>
</modules>
```

</div>

</div>

</div>

</div>

<div>

<table>
<tbody>
<tr>
<th></th>
<th><p><strong>Database</strong></p></th>
<th><p><strong>YAML</strong></p></th>
<th><p><strong>Properties</strong></p></th>
<th><p><strong>JSON</strong></p></th>
<th><p><strong>XML</strong></p></th>
</tr>
&#10;<tr>
<td colspan="6"><p><strong>The complexity and structure of the configuration data.</strong></p></td>
</tr>
<tr>
<td><p>Simple and flat</p></td>
<td></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
</tr>
<tr>
<td><p>complex and hierarchical</p></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
</tr>
<tr>
<td><p>Relational or non-relational</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="6"><p><strong>The readability and maintainability of the configuration data</strong></p></td>
</tr>
<tr>
<td><p>Human-friendly and easy to read/write</p></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Machine-friendly and easy to parse/generate</p></td>
<td></td>
<td></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
</tr>
<tr>
<td><p>Requires security or transactions</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="6"><p><strong>The compatibility and interoperability of the configuration data</strong></p></td>
</tr>
<tr>
<td><p>Widely supported by all platforms and libraries</p></td>
<td></td>
<td></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
</tr>
<tr>
<td><p>Specific to Java or Spring framework</p></td>
<td></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Independent of any platform or library</p></td>
<td><p>

![[47401730049-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><strong>Summary</strong></p></td>
<td><p>relational or non-relational, secure, independent</p></td>
<td><p>complex, hierarchical, human-friendly, Java-specific</p></td>
<td><p>simple, flat, human-friendly, Java-specific</p></td>
<td><p>simple, flat, machine-friendly, widely-supported</p></td>
<td><p>complex, hierarchical, machine-friendly, widely-supported</p></td>
</tr>
</tbody>
</table>

</div>


![[47401730049-image-20230612-101758.png]]



Based on the factors above, we will choose properties to do the config file.

And system not only using this configuration to build report_predicate in others module. We also using it to build column group configuration.

Example:

In ColumnGroupConfigurationService, when build the column optional (column configuration as attachments). then system also get all the con figuration for each column configuration. This way all the configs will be loaded as 1 time.

<div id="expander-2124753585" class="expand-container conf-macro output-block" hasbody="true" macro-id="14621c6b-a98e-41a7-a2db-ef47a32a89b7" macro-name="expand">

<div id="expander-control-2124753585" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-2124753585" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="80542103-7999-4683-9aee-ca30447cf1a6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre

"columnConfigurations": [
        {
            "attributeName": "employeeId",
            "bundleKey": "employeeId",
            "columnIndex": 0,
            "criteria": [],
            "displayName": "Id",
            "entityAttributeName": "id",
            "entityName": "EmployeeEntity"
        },
        {
            "attributeName": "firstName",
            "bundleKey": "firstName",
            "columnIndex": 1,
            "criteria": [],
            "displayName": "First Name",
            "entityAttributeName": "firstName",
            "entityName": "PersonEntity"
        },...
        {
            "attributeName": "levelOfEmployment",
            "bundleKey": "levelOfEmployment",
            "columnIndex": 17,
            "criteria": [
                {
                    "bundleKey": "current",
                    "criteriaType": "RANGE_TIME",
                    "criteriaValue": "CURRENT",
                    "criteriaValueType": "COMBINE_TIME_TYPE",
                    "displayName": "Current"
                },
                {
                    "bundleKey": "month",
                    "criteriaType": "RANGE_TIME",
                    "criteriaValue": "MONTH",
                    "criteriaValueType": "PERIOD_TYPE",
                    "displayName": "Month"
                },
                ...
            ],
            "displayName": "Level of employment",
            "entityAttributeName": "engagementLevel",
            "entityName": "ContractTemporalInfoEntity"
        },
        ...
]
```

</div>

</div>

</div>

</div>

# Configure for new field:

In case we are having 1 new field then:

1.  Change configuration file so that system can build the configuration such as column configuration.

2.  Add resource bundle key / value for translations of new fields.

3.  Handle new service in luz_service to get data for that fields. If table in a module already existing in luz_report then this step will be modifying service to adapt for new field.

# **Confiuration by annotation:**

Create a new annotation in report_common then the entity / attribute want to display in luz_report_service.


![[47401730049-using annotation.png]]

![[47401730049-image-20230614-050129.png]]



If we handle in this way then the relationship of entity must config correctly.

And inside luz_report_service still need 1 properties file to config relations for each entity which mean the config will be like:

<div id="expander-1393842355" class="expand-container conf-macro output-block" hasbody="true" macro-id="7538dfe4-b111-4912-9ec9-eecb8c741feb" macro-name="expand">

<div id="expander-control-1393842355" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1393842355" class="expand-content expand-hidden">

**module_config.properties**

CRM: crm_configuration

employee: employee_configuration

**employee_configuration.properties**

emergencyAddress: EmployeeEntity

entryDate : EmployeeEntity.ContractEntity

levelOfEmployment : EmployeeEntity.ContractEntity.ContractTemporalInfoEntity

</div>

</div>

So incase add 1 new field then

1.  luz_service add annotation for entity / attribute that you want to display it in luz_report_service

2.  luz_report_service add new relationship configurtations.
