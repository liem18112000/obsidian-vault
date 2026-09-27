---
title: "Rethink: eArchive access right concept"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47167176920/Rethink+eArchive+access+right+concept
space: "TP2020"
topic: architecture
relevance: 0.76
depth: 2.59
updated: 2022-08-19
attachments: 0
tags:
  - confluence
  - architecture
  - space/tp2020
---

# Rethink: eArchive access right concept

> [!info] Imported from Confluence
> Space **TP2020** · updated 2022-08-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47167176920/Rethink+eArchive+access+right+concept)
> Relevance 0.76 · topic `architecture`

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
<th><p><strong>Concern</strong></p></th>
<th><p><strong>Approach: Security classes belong to folder DIRECTLY</strong></p></th>
<th><p><strong>Approach: Security classes belong to folder INDIRECTLY</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>How to know the access right of a folder?</p></td>
<td><p>get security class directly from the folder metadata</p></td>
<td><p>calculate by all security classes of all documents (recursively) inside that folder.</p></td>
</tr>
<tr>
<td>2</td>
<td><p>What is a public vs non-public document?</p></td>
<td><ul>
<li><p><strong>public</strong> document is a document having <strong>no</strong> security class in the metadata/inherited from containing folders.</p></li>
<li><p><strong>non-public</strong> document is a document having security classes in the metadata/inherited from containing folders.</p></li>
</ul></td>
<td><ul>
<li><p><strong>public</strong> document is a document having <strong>no</strong> security class in the metadata</p></li>
<li><p><strong>non-public</strong> document is a document having security classes in the metadata.</p></li>
</ul></td>
</tr>
<tr>
<td>3</td>
<td><p>What is a public vs non-public folder?</p></td>
<td><ul>
<li><p><strong>public</strong> folder is a folder having <strong>no</strong> security class in the metadata.</p></li>
<li><p><strong>non-public</strong> folder is a folder having security classes in the metadata.</p></li>
</ul></td>
<td><ul>
<li><p><strong>public</strong> folder is a folder having <strong>no</strong> security class inherited from all documents (recursively) inside that folder.</p></li>
<li><p><strong>non-public</strong> folder is a folder having security classes inherited from all documents (recursively) inside that folder.</p></li>
</ul></td>
</tr>
<tr>
<td>4</td>
<td><p>What are the affects when archiving a <strong>public document to public folders</strong>?</p></td>
<td><p>no affect</p></td>
<td><p>no affect</p></td>
</tr>
<tr>
<td>5</td>
<td><p>What are the affects when archiving a <strong>non-public document to public folders</strong>?</p></td>
<td><p>non-public document → public document:</p>
<ul>
<li><p>security classes of document (and its copies) are cleared.</p></li>
<li><p>access rights of document (and its copies) are inherited from the containing folders.</p></li>
</ul></td>
<td><p>public folder → non-public folder</p></td>
</tr>
<tr>
<td>6</td>
<td><p>What are the affects when archiving a <strong>public document to non-public folders</strong>?</p></td>
<td><p>public document → non-public document</p>
<ul>
<li><p>security classes of document (and its copies) are still empty.</p></li>
<li><p>access rights of document (and its copies) are inherited from the containing folders.</p></li>
</ul></td>
<td><p>no affect</p></td>
</tr>
<tr>
<td>7</td>
<td><p>What are the affects when archiving a <strong>non-public document to non-public folders</strong>?</p></td>
<td><p>non-public document’s access right is changed:</p>
<ul>
<li><p>security classes of document (and its copies) are still empty/cleared.</p></li>
<li><p>access rights of document (and its copies) are inherited from the containing folders.</p></li>
</ul></td>
<td><p>non-public folder’s access right is changed:</p>
<ul>
<li><p>access rights of the folder are inherited from the contained documents.</p></li>
</ul></td>
</tr>
<tr>
<td>8</td>
<td><p>Access right: View</p></td>
<td><p>Folder:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
<td><p>Folder:</p>
<ul>
<li><p>Everyone</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
</tr>
<tr>
<td>9</td>
<td><p>Access right: Edit</p></td>
<td><p>Folder:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
<td><p>Folder:</p>
<ul>
<li><p>Everyone</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
</tr>
<tr>
<td>10</td>
<td><p>Access right: Store/Copy</p></td>
<td><p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
<td><p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
</tr>
<tr>
<td>11</td>
<td><p>Access right: Delete</p></td>
<td><p>Folder:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
<td><p>Folder:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
</tr>
<tr>
<td>12</td>
<td><p>Access right: Restore</p></td>
<td><p>Folder:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
<td><p>Folder:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul>
<p>Document:</p>
<ul>
<li><p>Based on security classes</p></li>
</ul></td>
</tr>
<tr>
<td>13</td>
<td><p>What are the affects to “copy” behavior?</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>14</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>15</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>16</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>17</td>
<td><p>***Note</p></td>
<td><ul>
<li><p>Cannot manage access rights for documents individually after storing to a folder.</p></li>
<li><p>Need to have the solution for existing documents having security classes (clear or keep).</p></li>
</ul></td>
<td><ul>
<li><p>Calculate by all security classes of all documents (recursively) inside that folder.</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>
