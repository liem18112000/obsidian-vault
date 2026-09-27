---
title: "Upgrade Ivy - Known issues"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/X4/pages/47478866456/Upgrade+Ivy+-+Known+issues
space: "X4"
topic: programming
relevance: 0.724
depth: 2.81
updated: 2023-11-03
attachments: 11
tags:
  - confluence
  - programming
  - space/x4
---

# Upgrade Ivy - Known issues

> [!info] Imported from Confluence
> Space **X4** · updated 2023-11-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47478866456/Upgrade+Ivy+-+Known+issues)
> Relevance 0.724 · topic `programming`

This page is about to list out all issue happened when upgrade ivy and the status of these issue.

<div class="toc-macro client-side-toc-macro non-printable conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="9d8a1de2-9bb9-4b08-bcbe-0b6b010a10b0" macro-name="toc" numberedoutline="false" structure="list">

</div>

### Check all java code where has this hashtag: // `TODO upgrade ivy`

### 1. SASS compile warning

<div id="expander-1107050660" class="expand-container conf-macro output-block" hasbody="true" macro-id="b3c6e94a-17f7-4e4a-860d-23bf6354a903" macro-name="expand">

<div id="expander-control-1107050660" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1107050660" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="39d6c0fa-852c-4b85-b732-15c37ce7db0c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[INFO] --- sass-maven-plugin:2.14:update-stylesheets (default) @ eapf_web ---

[INFO] Checked 0 files for C:\work\admetos\src\ivy\admetosRepo-ivy10\eapf_web\src\main\sass

[INFO] Checked 0 files for C:\work\admetos\src\ivy\admetosRepo-ivy10\eapf_web\target\eapf_web-4.4.6.0\css

[INFO] Compiling Sass templates

[INFO] Queueing Sass template for compile: C:/work/admetos/src/ivy/admetosRepo-ivy10/eapf_web/webContent/resources/layout/styles/sass => C:/work/admetos/src/ivy/admetosRepo-ivy10/eapf_web/webContent/resources/layout/styles/css

unsupported Java version "11", defaulting to 1.5

[INFO]     >> C:/work/admetos/src/ivy/admetosRepo-iv
```

</div>

</div>

</div>

</div>

### 2. CustomVarcharField warning

<div id="expander-2045905081" class="expand-container conf-macro output-block" hasbody="true" macro-id="0c3dd1cc-8a9d-443c-81ef-15928e3dc047" macro-name="expand">

<div id="expander-control-2045905081" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-2045905081" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fb7ec858-3a8b-4d7d-8099-477e7fd18184" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
023-09-06 11:14:19.132 WARN [ch.ivyteam.ivy.deprecation.api] [RxCachedThreadScheduler-5] [request=(21.21.0.0), session=0 (SYSTEM), task=21, application=2147483647, requestId=145, executionContext=SYSTEM, pmv=designer$eapf_web$1]

  Method getCustomVarCharField1 of class ch.ivyteam.ivy.workflow.ICase is deprecated. Instead use customFields().stringField("CustomVarCharField1").getOrNull() method.

2023-09-06 11:14:19.616 WARN [ch.ivyteam.ivy.deprecation.api] [RxCachedThreadScheduler-3] [request=(24.24.0.0), session=0 (SYSTEM), task=24, application=2147483647, requestId=149, executionContext=SYSTEM, pmv=designer$eapf_web$1]

  Method setAdditionalProperty(String, String) of class ch.ivyteam.ivy.workflow.IAdditionalPropertyable is deprecated. Instead use customFields().textField(String).set(String) method.

2023-09-06 11:14:19.639 WARN [ch.ivyteam.ivy.deprecation.api] [RxCachedThreadScheduler-3] [request=(24.24.0.0), session=0 (SYSTEM), task=24, application=2147483647, requestId=149, executionContext=SYSTEM, pmv=designer$eapf_web$1]

  Method getAdditionalProperty(String) of class ch.ivyteam.ivy.workflow.IAdditionalPropertyable is deprecated. Instead use customFields().textField(String).getOrNull() method.
```

</div>

</div>

</div>

</div>

 

### 3. Null when update component on AmountDetailBean

<div id="expander-37736439" class="expand-container conf-macro output-block" hasbody="true" macro-id="6ca987c9-7c46-4a99-9c40-9bd6257846f4" macro-name="expand">

<div id="expander-control-37736439" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-37736439" class="expand-content expand-hidden">


![[47478866456-image-20230906-045637.png]]



</div>

</div>

 

### 4. JavaScript error when open TaskList “: Identifier 'TASK_FILTER_COMP_ID' has already been declared

<div id="expander-915709888" class="expand-container conf-macro output-block" hasbody="true" macro-id="b3e3ba4c-16e7-4e79-abe7-8b5bd1974144" macro-name="expand">

<div id="expander-control-915709888" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-915709888" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="04caf05d-a385-4727-ab73-f99b3ab28059" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
VM6983:1 Uncaught SyntaxError: Identifier 'TASK_FILTER_COMP_ID' has already been declared
    at b (jquery.js?ln=primefaces&v=7.0.26:2:839)
    at Function.globalEval (jquery.js?ln=primefaces&v=7.0.26:2:2878)
    at Object.dataFilter (jquery.js?ln=primefaces&v=7.0.26:2:80619)
    at jquery.js?ln=primefaces&v=7.0.26:2:79084
    at l (jquery.js?ln=primefaces&v=7.0.26:2:79486)
    at XMLHttpRequest.<anonymous> (jquery.js?ln=primefaces&v=7.0.26:2:82254)
    at Object.send (jquery.js?ln=primefaces&v=7.0.26:2:82613)
    at Function.ajax (jquery.js?ln=primefaces&v=7.0.26:2:78223)
    at S._evalUrl (jquery.js?ln=primefaces&v=7.0.26:2:80485)
    at Pe (jquery.js?ln=primefaces&v=7.0.26:2:48477)
```

</div>

</div>

</div>

</div>

### 5. Sorting on tasklist

<div id="expander-1725246354" class="expand-container conf-macro output-block" hasbody="true" macro-id="c4f09bde-d918-408c-9bca-b4556f535187" macro-name="expand">

<div id="expander-control-1725246354" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-1725246354" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1418fdb7-1ba7-4219-a741-97b70c60d3e4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
Uncaught TypeError: headerColumns.size is not a function
    at TableUtil.getColumnsWidth (common.js?ln=xpertivy-5-webContent&xv=235862123403:226:39)
    at DataList.resize (columns.js?ln=xpertivy-5-webContent&xv=235862123403:4:32)
    at Object.onco (<anonymous>:1:1217)
    at Object.<anonymous> (core.js?ln=primefaces&v=7.0.26:3:8446)
    at c (jquery.js?ln=primefaces&v=7.0.26:2:28294)
    at Object.fireWith [as resolveWith] (jquery.js?ln=primefaces&v=7.0.26:2:29039)
    at l (jquery.js?ln=primefaces&v=7.0.26:2:79800)
    at XMLHttpRequest.<anonymous> (jquery.js?ln=primefaces&v=7.0.26:2:82254)
```

</div>

</div>

</div>

</div>

### 6. PDF view on ItemHead detail

<div id="expander-953214665" class="expand-container conf-macro output-block" hasbody="true" macro-id="f9830955-c991-45ca-ac54-0493d553c6cc" macro-name="expand">

<div id="expander-control-953214665" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-953214665" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="91317e8c-658b-4fd8-9678-aced11b7ca67" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
jquery.js?ln=primefaces&v=7.0.26:2 Uncaught TypeError: e.indexOf is not a function
    at S.fn.load (jquery.js?ln=primefaces&v=7.0.26:2:84831)
    at save-pdfview-dimension.js?ln=xpertivy-5-webContent&xv=235862123403:16:11
S.fn.load @ jquery.js?ln=primefaces&v=7.0.26:2
(anonymous) @ save-pdfview-dimension.js?ln=xpertivy-5-webContent&xv=235862123403:16
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4650d837-130a-4f34-8f52-2bff0c42282c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
pdf.viewer.js?ln=pri…sions&v=7.0.3:18396 offsetParent is not set -- cannot scroll
scrollIntoView  @   pdf.viewer.js?ln=pri…sions&v=7.0.3:18396
_scrollIntoView @   pdf.viewer.js?ln=pri…sions&v=7.0.3:24876
scrollPageIntoView  @   pdf.viewer.js?ln=pri…sions&v=7.0.3:25424
_setScaleUpdatePages    @   pdf.viewer.js?ln=pri…sions&v=7.0.3:25268
_setScale   @   pdf.viewer.js?ln=pri…sions&v=7.0.3:25318
set @   pdf.viewer.js?ln=pri…sions&v=7.0.3:25670
webViewerResize @   pdf.viewer.js?ln=pri…sions&v=7.0.3:20383
(anonymous) @   pdf.viewer.js?ln=pri…sions&v=7.0.3:18722
dispatch    @   pdf.viewer.js?ln=pri…sions&v=7.0.3:18721
_boundEvents.windowResize   @   pdf.viewer.js?ln=pri…sions&v=7.0.3:20049
resize (async)
```

</div>

</div>

</div>

</div>

### 7. Cannot display supplier dropdown list and selected data

<div id="expander-888375103" class="expand-container conf-macro output-block" hasbody="true" macro-id="86466a5e-e34a-4bcc-b0c6-3422c155367c" macro-name="expand">

<div id="expander-control-888375103" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>

</div>

<div id="expander-content-888375103" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b0fd0813-b6b8-4bfe-bc22-9d0a55e6f31a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
2023-09-06 13:39:22.996 ERROR [ch.soreco.admetos.eapf.web.application.masterdata.crosscompany.util.CrossCompanyMasterDataUtil] [http-nio-8081-exec-7] [request=/ivy/faces/instances/designer/eapf_web$1/18A6937742742625/itemHead.ItemHeadUI/ItemHeadUI.xhtml, session=1 (Lisa), task=60, application=2147483647, requestId=919, executionContext=SYSTEM, pmv=designer$eapf_web$1, client=0:0:0:0:0:0:0:1, hd=itemHead.ItemHeadUI] 
  Something wrong when retrieve supplier
    java.lang.ClassCastException: class java.util.Optional cannot be cast to class java.util.List (java.util.Optional and java.util.List are in module java.base of loader 'bootstrap')
        at IvyProjectClassLoader [pmv=designer$eapf_web$1,generation=1]//ch.soreco.admetos.eapf.web.application.masterdata.service.MasterDataPaginationService.loadMasterDataFrom(MasterDataPaginationService.java:123)
        at IvyProjectClassLoader [pmv=designer$eapf_web$1,generation=1]//ch.soreco.admetos.eapf.web.application.masterdata.service.MasterDataPaginationService.loadMasterDataForPartAndDataType(MasterDataPaginationService.java:107)
        at IvyProjectClassLoader [pmv=designer$eapf_web$1,generation=1]//ch.soreco.admetos.eapf.web.application.masterdata.service.MasterDataPaginationService.lambda$1(MasterDataPaginationService.java:88)
        at java.base/java.util.stream.ReferencePipeline$3$1.accept(Unknown Source)
        at java.base/java.util.ArrayList$ArrayListSpliterator.forEachRemaining(Unknown Source)
        at java.base/java.util.stream.AbstractPipeline.copyInto(Unknown Source)
        at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(Unknown Source)
        at java.base/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(Unknown Source)
        at java.base/java.util.stream.AbstractPipeline.evaluate(Unknown Source)
```

</div>

</div>

</div>

</div>

### 8. DedicatedTaskQueryUtil.java are using native query to search Ivy task

`TaskAdditionalProperty` has been removed on Ivy 8, please use `customField` instead.

Example

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5f29a12a-fbb2-4dd7-918e-2e0eb01db881" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private static final String TASK_BY_ITEMHEAD_ID_ORDERED_DESC_INCLUDE_DESTROYED_TASK = "SELECT TOP(1) IWA_TASK.TaskId FROM IWA_Task LEFT JOIN IWA_TaskAdditionalProperty TaskAddPrp_isDoneTask ON IWA_Task.TaskId = TaskAddPrp_isDoneTask.TaskId LEFT JOIN IWA_AdditionalProperty AddPrpTask_isDoneTask ON TaskAddPrp_isDoneTask.AdditionalPropertyId = AddPrpTask_isDoneTask.AdditionalPropertyId\n" +
        "WHERE (\n" +
        "\tIWA_Task.State in (%s)\n" +
        "\tOR (IWA_Task.State = 7 AND AddPrpTask_isDoneTask.Name = 'isDoneTask' AND AddPrpTask_isDoneTask.Value LIKE 'true')\n" +
        ") AND IWA_TASK.Name LIKE 'ItemHead ID: %s' ORDER BY TaskId DESC";
```

</div>

</div>

### 9. Broken code caused by Primefaces 11

- Constructor of Primefaces LazyDataModel changed

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c2b0d3c1-718f-405c-a357-f155cde9a08a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public abstract List<T> load(int first, int pageSize, Map<String, SortMeta> sortBy, Map<String, FilterMeta> filterBy);Constructor of DefaultStreamedContent has been removed, need to use builder.
```

</div>

</div>

- Constructor of DefaultStreamedContent has been removed, need to use builder.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b0f72d66-7683-4c56-93d0-a967bc04b6cb" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
  return DefaultStreamedContent.builder()
                    .stream(() -> inputStream)
                    .contentType(contentType)
                    .name(fileName)
                    .build();
```

</div>

</div>

### 10. Broken code caused by Ivy 10

- `ch.ivyteam.ivy.workflow.IPageArchive` has been removed

- Public API IPermission.SESSION_READ_SESSION_USER has been removed on Absence management

### 11. Deprecated Ivy API

- The method executeAsSystemUser(Callable\<Boolean\>) from the type ISecurityContext has been deprecated since version 9.3 and marked for removal.

- IPermission.SESSION_READ_SESSION_USER

### 12. Trouble when sync master code into Ivy Upgrade branch

From ivy 10, the project structure has changed

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Ivy 7</strong></p></th>
<th><p><strong>Ivy 10</strong></p></th>
</tr>
&#10;<tr>
<td><p>CMS</p></td>
<td><p>file .data</p>

![[47478866456-image-20230912-091817.png]]

</td>
<td><p>centralized cms_en.yaml</p>

![[47478866456-image-20230912-092108.png]]

</td>
</tr>
<tr>
<td><p>Process file</p></td>
<td><p>file .mod</p></td>
<td><p>file .p.json</p>

![[47478866456-image-20230912-092213.png]]

</td>
</tr>
</tbody>
</table>

</div>

Because the file format has been changed, so when sync master code, the latest cms and mod file of Project ivy 7 is not able to merge into Project ivy 10.

For example

- Mix style on MOD file:


![[47478866456-image-20230912-093015.png]]



Mix style on cms file


![[47478866456-image-20230912-093338.png]]



It could cause some issues like latest CMS and Process files are missing, so please aware when sync master code.

Work around: Please reference [Upgrade Ivy - Technical notes](https://axonivy.atlassian.net/wiki/spaces/X4/pages/47473492078/Upgrade+Ivy+-+Developers+Technical+notes) to know how to covert the whole ivy project to new version format. For single component/ cms we need to investigate it 

![[47478866456-smile.png]]



### 13. Font-awesome has upgraded from version 4.7 → 6.1


![[47478866456-image-20231025-034528.png]]



ref: <a href="https://fontawesome.com/docs/web/setup/upgrade/upgrade-from-v4" class="external-link" data-card-appearance="inline" rel="nofollow">https://fontawesome.com/docs/web/setup/upgrade/upgrade-from-v4</a>

### 14. Sorting and filtering of component p:dataTable and p:treeTable


![[47478866456-image-20231025-034813.png]]



Issue: Sorting on Error not working caused by it always show no record found when filter.


![[47478866456-image-20231101-112657.png]]



How to fix:

Override DataTable widget of PrimeFace.

<div id="expander-534077303" class="expand-container conf-macro output-block" hasbody="true" macro-id="0f06f012-ca12-497c-abdd-9e70e4ff7bdc" macro-name="expand">

<div id="expander-control-534077303" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">datatable.js</span>

</div>

<div id="expander-content-534077303" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dea35b25-fe55-483d-a8a6-59315aaa3d8a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
if (PrimeFaces.widget.DataTable) {
    PrimeFaces.widget.DataTable.prototype.shouldSort = function(event, column) {
       ......
    };
    
     /**
     * Sets the given HTML string as the content of the body of this DataTable. Afterwards, sets up all required event
     * listeners etc.
     * @protected
     * @param {string} data HTML string to set on the body.
     * @param {boolean} [clear] Whether the contents of the table body should be removed beforehand.
     */
    PrimeFaces.widget.DataTable.prototype.updateData = function(data, clear) {
        ....
    };
  
}
```

</div>

</div>

</div>

</div>

### 15 Deprecated javascript and jQuery API

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
<th><p><strong>Issue</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>How to fix</strong></p></th>
<th><p><strong>Note</strong></p></th>
</tr>
&#10;<tr>
<td><p>Window onload</p></td>
<td><p>$(window).load(function</p></td>
<td><p>$(window).on('load', function() {}</p></td>
<td><p>Related to Jquery upgrade 3.x</p></td>
</tr>
<tr>
<td><p>Cookie not working</p></td>
<td><p>$.cookie(CENTER_LAYOUT_LEFT_KEY);</p></td>
<td><p>Cookies.get(CENTER_LAYOUT_LEFT_KEY)</p></td>
<td></td>
</tr>
<tr>
<td><p>Using <strong>widgetVar</strong> leads to <strong>Widget for var 'abc' not available!</strong></p></td>
<td><p>With new version of PrimeFaces, this call return null and if you continue operating like PF('inputWidgetVar').close(), the error will occur.</p></td>
<td><p>Check null by calling</p>
<p><code>PrimeFaces.widgets['widgetVarName']</code> before PF('….).close();</p></td>
<td><p>Introduced utility method <code>hideComponentByWidgetVar()</code> on common.js</p></td>
</tr>
</tbody>
</table>

</div>

### 16 DataTable

<div>

|  |  |  |  |
|----|----|----|----|
| **Issue** | **Description** | **How to fix** | **Note** |
| Cannot display \<p:summaryRow\> |  | 

![[47478866456-image-20231103-094030.png]]

 | Move sortBy attribute to column instead of dataTable |
|  |  |  |  |

</div>
