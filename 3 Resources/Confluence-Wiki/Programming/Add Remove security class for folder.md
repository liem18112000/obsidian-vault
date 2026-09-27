---
title: "Add/Remove security class for folder"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47216854272/Add+Remove+security+class+for+folder
space: "LUZ"
topic: programming
relevance: 0.837
depth: 3
updated: 2023-03-22
attachments: 8
tags:
  - confluence
  - programming
  - space/luz
---

# Add/Remove security class for folder

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-03-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47216854272/Add+Remove+security+class+for+folder)
> Relevance 0.837 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" csslistindent="15px" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="422c6a637f1dc19ed9d330371ac677ca" macro-name="toc" midseparator="none" postseparator="" preseparator="">

</div>

# 1. API


![[47216854272-image-20221118-030506.png]]

![[47216854272-image-20221118-030531.png]]



# 2. Flow


![[47216854272-update security class on folder.png]]



# 3. Description

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Steps</strong></p></th>
<th><p><strong>Description</strong></p></th>
<th><p><strong>Explanation/Notes</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><p>Update security-classes</p></td>
<td><p>Update security class for a folder including nested child folders.</p></td>
</tr>
<tr>
<td><p>2, 3 &amp; 4</p></td>
<td><p>get folder metadata by id</p></td>
<td><p>luz_docs gets the requested folder metadata from luz_jsonstore.</p>
<p>If the folder could not be found the client of the API gets informed (errormessage).</p></td>
</tr>
<tr>
<td><p>5, 6 &amp; 7</p></td>
<td><p>Find subfolder ids</p></td>
<td><p>luz_docs rescursively loops on the requested folder to find all its nested subfolders and collect their IDs.</p></td>
</tr>
<tr>
<td><p>8 &amp; 9</p></td>
<td><p>Update folder metadata</p></td>
<td><p>luz_docs loops over all folders which are found by steps #7 and do update securityClassCodes field as requested. Collect folder IDs which has failed to update if any.</p>
<p>Main folder will be update with <code>securityClassCodes</code></p>
<p>All sub folder will be update with <code>inheritedSecurityClassCodes</code></p></td>
</tr>
<tr>
<td><p>10</p></td>
<td><p>Return final response</p></td>
<td><p>luz_docs will check if any failed IDs from steps #8 and #9 then build the final response object.</p>
<p>If no failure, empty reponse with code 200 will be returned.</p>
<p>If there is any failure, a JSON object with details will be returned .</p></td>
</tr>
</tbody>
</table>

</div>

# 4. Sample requests and responses


![[47216854272-add_request_multi.PNG]]

![[47216854272-add_request_single.PNG]]

![[47216854272-remove_request_multi.PNG]]
