---
ai_hash: 56d346581f248bed
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 3
entities: []
relevance: 0.878
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48576168343/Error+handling+for+delete+and+undo
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Error handling for delete and undo
topic: programming
type: source
updated: 2025-07-14
---

# Error handling for delete and undo

> [!info] Imported from Confluence
> Space **Helios** · updated 2025-07-14 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/48576168343/Error+handling+for+delete+and+undo)
> Relevance 0.878 · topic `programming`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>UIB api</strong></p></th>
<th><p><strong>luz_doc api</strong></p></th>
<th><p><strong>Error code</strong></p></th>
<th><p><strong>UIB Exception</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td></td>
<td><p>General error</p></td>
<td><p>401</p>
<p>403</p>
<p>500, 502, 503</p>
<p>other error</p></td>
<td><ul>
<li><p>401 =&gt; Throw 401</p></li>
<li><p>403 =&gt; Throw 403</p></li>
<li><p>5xx =&gt; throw 500</p></li>
<li><p>Other error =&gt; throw 500 internal error</p></li>
</ul></td>
</tr>
<tr>
<td>2</td>
<td rowspan="3"><p>delete group</p>
<p><code>messages/groups/{group-id}/soft-delete</code></p>
<p>→ <code>/groups/{group-id}/soft-delete</code>(move resource)</p></td>
<td><p>@GET // Get group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>404 =&gt; throw 404 group already hard deleted<br />
=&gt; cannot be undo, moved to trash</p></li>
</ul></td>
</tr>
<tr>
<td>3</td>
<td><p>@DELETE // soft delete documents</p>
<p><code>{tenant-id}/documents</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
</ul></td>
</tr>
<tr>
<td>4</td>
<td><p>@PATCH // alter group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
<li><p>retry flow =&gt; need story to implement later 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td>5</td>
<td rowspan="3"><p>undo group</p></td>
<td><p>@GET // Get group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>throw 404</p></li>
</ul></td>
</tr>
<tr>
<td>6</td>
<td><p>@POST // undo documents</p>
<p><code>{tenant-id}/documents/recovery</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
</ul></td>
</tr>
<tr>
<td>7</td>
<td><p>@PATCH // alter group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
<li><p>retry flow =&gt; need story to implement later 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td>8</td>
<td rowspan="5"><p>hard delete group in message box</p></td>
<td><p>@GET // Get group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>404 =&gt; Return 200 OK assume group is deleted</p></li>
</ul></td>
</tr>
<tr>
<td>9</td>
<td><p>@DELETE // hard delete documents</p>
<p><code>{tenant-id}/documents</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
</ul></td>
</tr>
<tr>
<td>10</td>
<td><p>@PATCH // alter group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
<li><p>retry flow =&gt; need story to implement later 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td>11</td>
<td><p>@GET // Get group</p>
<p><code>{tenant-id}/groups/{group-id} (will remove reuse response line 10)</code></p></td>
<td></td>
<td></td>
</tr>
<tr>
<td>12</td>
<td><p>@DELETE // delete group</p>
<p><code>{tenant-id}/group/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>corner case: it may delete new incoming message 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td>13</td>
<td rowspan="4"><p>delete message</p></td>
<td><p>@GET //get document</p>
<p><code>{tenant-id}/documents/{document-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>404 =&gt; throw 404 message already hard deleted<br />
=&gt; cannot be undo, moved to trash</p></li>
</ul></td>
</tr>
<tr>
<td>14</td>
<td><p>@GET // Get group</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>corner case: what business need to handle 

![[48576168343-help_16.png]]

</p>
<ul>
<li><p>create new group</p></li>
<li><p>ignore the error</p></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td>15</td>
<td><p>@DELETE <code>{tenant-id}/documents/{document-id}</code></p>
<p>=&gt; move to use api delete documents with single document in list</p>
<p><code>{tenant-id}/documents</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
</ul></td>
</tr>
<tr>
<td>16</td>
<td><p>@PATCH</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
<li><p>retry flow =&gt; need story to implement later 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td>17</td>
<td rowspan="4"><p>undo message</p></td>
<td><p>@GET</p>
<p><code>{tenant-id}/documents/{document-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>404 =&gt; throw 404 message already hard deleted</p></li>
</ul></td>
</tr>
<tr>
<td>18</td>
<td><p>@GET</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>corner case: what business need to handle 

![[48576168343-help_16.png]]

</p>
<ul>
<li><p>create new group</p></li>
<li><p>ignore the error</p></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td>19</td>
<td><p>@DELETE</p>
<p><code>letters/{id}/deletion-undoing</code></p>
<p>=&gt; move to use api delete documents with single document in list</p>
<p><code>{tenant-id}/documents</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
</ul></td>
</tr>
<tr>
<td>20</td>
<td><p>@PATCH</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
<li><p>retry flow =&gt; need story to implement later 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
<tr>
<td>21</td>
<td rowspan="4"><p>hard delete message</p></td>
<td><p>@GET</p>
<p><code>{tenant-id}/documents/{document-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>404 =&gt; return 200 ok (assume another client already updated group)</p></li>
</ul></td>
</tr>
<tr>
<td>22</td>
<td><p>@GET</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>corner case: what business need to handle 

![[48576168343-help_16.png]]

</p>
<ul>
<li><p>create new group</p></li>
<li><p>ignore the error</p></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td>23</td>
<td><p>@DELETE</p>
<p><code>{tenant-id}/documents/{id}</code></p></td>
<td><p>404</p></td>
<td><ul>
<li><p>assume deleted successful</p></li>
</ul></td>
</tr>
<tr>
<td>24</td>
<td><p>@PATCH</p>
<p><code>{tenant-id}/groups/{group-id}</code></p></td>
<td></td>
<td><ul>
<li><p>throw general error</p></li>
<li><p>retry flow =&gt; need story to implement later 

![[48576168343-help_16.png]]

</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div class="columnLayout two-equal">

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

</div>

</div>

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[LUZ-146746 Use correct API's for delete, restore and their undo]]
- [[Enhancements for API Delete and Restore]]
- [[REST API for deleting EXPENSES documents]]
- [[Public API - letterbox - API get deleted letters from trash]]
- [[Empty Trash APIs]]

%% ai-graph-end %%