---
ai_hash: a3b4388d6c81b845
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 15
depth: 3
entities: []
relevance: 0.887
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47317321514/Reporting+-+Java+class+configuration
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: Reporting - Java class configuration
topic: programming
type: source
updated: 2023-03-16
---

# Reporting - Java class configuration

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-03-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47317321514/Reporting+-+Java+class+configuration)
> Relevance 0.887 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="30aa38f7-145f-4cd3-ad9d-a6633d56f1eb" macro-name="toc">

</div>

## Discuss & Open point


![[47317321514-check.png]]

 Database - Postgres


![[47317321514-check.png]]

 1 field of selected field by user that will store as 1 record row in table database

<a href="https://www.figma.com/file/0tkjggnbhwY36DcCHcPSpy/Company-reporting-Architecture?node-id=0-1&amp;t=WMqfv1Jc4N3Pf5BI-0" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.figma.com/file/0tkjggnbhwY36DcCHcPSpy/Company-reporting-Architecture?node-id=0-1&amp;t=WMqfv1Jc4N3Pf5BI-0</a>


![[47317321514-warning.png]]

 JDK 17

## Diagram:


![[47317321514-Reporting class diagram.png]]



#### <span class="inline-comment-marker" ref="5fed2175-dfe3-44b2-85ad-6cd8633c95f3">Description</span> and example: <a href="https://www.figma.com/file/0tkjggnbhwY36DcCHcPSpy/Company-reporting-Architecture?node-id=0-1&amp;t=WMqfv1Jc4N3Pf5BI-0" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.figma.com/file/0tkjggnbhwY36DcCHcPSpy/Company-reporting-Architecture?node-id=0-1&amp;t=WMqfv1Jc4N3Pf5BI-0</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8d19f70b-0894-4803-a726-ddc9b062933e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
EmployeeReport.java

ReportColumn firstName;
ReportColumn lastName;
ReportColumn telephone;
ReportColumn levelOfEmployee;
ReportColumn salary;
......................4

initAllReportColumn() {
  firstName.setAttributeName("firstName");
  firstName.setDisplayName(ResourceBundle.getName());
  
  
  
  salary.setColumns(Arrays.asList(
      new ReportColumn("1000Monthly", .....);
      new ReportColumn("1001Fee",.......);
    ));
  .......

}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9a05e3f4-62a9-421b-be21-3e5427a08e73" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
ReportColumn.java

String attributeName;
String displayName;
String[] predefined;
int index;
List<ReportColumn> reportColumns;
```

</div>

</div>

## Database


![[47317321514-database.png]]



**Report:**

<div>

|  |  |  |  |
|----|----|----|----|
| **id** | **report_header_id** | **name** | **description** |
| 1 | 1 | Employee master data | This is a report showing all master data of your employee |
| 2 | 2 | New report |  |

</div>

- **name :** this is report table name

- **description :** description of report


![[47317321514-image-20230307-110841.png]]



**Report_template_item** (this just small data for example)

<div>

|        |               |           |           |                  |                  |
|--------|---------------|-----------|-----------|------------------|------------------|
| **id** | **report_id** | **name**  | **index** | **option_value** | **filter_value** |
| 1      | 1             | firstName | 1         |                  |                  |
| 2      | 1             | lastName  | 2         |                  |                  |
| 3      | 1             | email     | 3         | PRIVATE          |                  |
| 4      | 1             | salary    | 4         | 183              | CURRENT, MONTH   |
| 5      | 1             | salary    | 5         | 183 + 192        |                  |

</div>

- **name :** selected field name

- **index:** order of column in report

- **option_value :** selected option of field from user (if have). Normal case this is option come from field having enum type or field having options to select. In case of salary this will be stored selected salary type id.

- **filter_value :** selected filter option of field (if have)

Note:

**183** and **192** is salary item id in table **public.salary_item_type**.

%% ai-graph-start %%

**Related notes:**
- [[Handle report configuration for new fields (12.06.2023)]]
- [[Employee Report Implementation (10.05.2023)]]
- [[Generic Interface JSON file]]
- [[Let the owning service hold the report field mapping as config, not the reporting service in code]]
- [[Persistence layer implementation]]

%% ai-graph-end %%