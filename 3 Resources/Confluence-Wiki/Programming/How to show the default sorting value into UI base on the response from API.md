---
title: "How to show the default sorting value into UI base on the response from API"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47337802734/How+to+show+the+default+sorting+value+into+UI+base+on+the+response+from+API
space: "TP2020"
topic: programming
relevance: 0.762
depth: 2.73
updated: 2023-03-28
attachments: 0
tags:
  - confluence
  - programming
  - space/tp2020
---

# How to show the default sorting value into UI base on the response from API

> [!info] Imported from Confluence
> Space **TP2020** · updated 2023-03-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47337802734/How+to+show+the+default+sorting+value+into+UI+base+on+the+response+from+API)
> Relevance 0.762 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="6e2691b1-c9ba-4aa8-be1e-7e9204d2cf9a" macro-name="toc">

</div>

## 1. Questions

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
<th></th>
<th><p><strong>Questions</strong></p></th>
<th><p><strong>Proposal</strong></p></th>
<th><p><strong>Decision</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td colspan="3"><p><strong>Luz_docs_view_controller</strong></p></td>
</tr>
<tr>
<td>2</td>
<td><p>What endpoint will return that sortBy?</p>
<ol>
<li><p>Can we add query parameters to existing endpoint?</p></li>
<li><p>what are potential side effect for that decision?</p>
<ol>
<li><p>Can make the old API slow?</p></li>
<li><p>Can break the SRP of the endpoint?</p></li>
<li><p>other team might need to adapt to use the same API with us =&gt; create task in future story to check</p></li>
</ol></li>
</ol></td>
<td><p>isStored</p></td>
<td></td>
</tr>
<tr>
<td>3</td>
<td><p>How to add the sortBy information to the API response without breaking SOLID principles? =&gt; consider in detail implementation</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>4</td>
<td><p>Is it configurable at runtime, and why? =&gt; NO, because we need to get this value many times, calling to get configuration will be slow</p></td>
<td><p>Using hard code &amp; return by input value</p></td>
<td></td>
</tr>
<tr>
<td>5</td>
<td><p>[performance] What is the maximum number of requests per second that can be supported? =&gt; consider in detail implementation</p></td>
<td><p>Using hard code &amp; return by input value</p></td>
<td></td>
</tr>
</tbody>
</table>

</div>

## 2. Solution

### 2.1 Luz_epost_business_web

What are potential working solutions?

**For API communication**

Using `isStored`

**For performance (call 1 API)**

Include the sorting in search result to reduce number of API call onlylist=false

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f45a1bc-f96e-4f05-b07b-ee20f9a00dd7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "totalRecordCount": 150,
    "size": 1,
    "sortingParam": {
      "sortBy": "_createdDate",
      "sortMode": "DESC"
    },
    "letters": [ { "id": "63c617963f082b7e3460d719", ... } ]
}
```

</div>

</div>

**For updating components**

Update value of sorting bean in Java

1.  When searching complete, update value of SortingBean (same place getting document counter)

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2a86326c-6713-4a70-b312-f0f5efbf174e" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    SortingBeanFactory.get().setDefaultSortBy((e.g. SortBy.REFERENCE_DATE) letterSearchResult.getSortBy());
    ```

    </div>

    </div>

2.  only display the filter bar after that =\> done

### 2.2 Luz_docs_view_controller
