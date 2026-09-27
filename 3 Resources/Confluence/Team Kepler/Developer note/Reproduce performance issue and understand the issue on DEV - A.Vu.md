---
title: "Reproduce performance issue and understand the issue on DEV - A.Vu"
created: 2026-05-04
updated: 2026-05-04
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/49384718375/Reproduce+performance+issue+and+understand+the+issue+on+DEV+-+A.Vu
confluence_id: "49384718375"
confluence_path: "Team Kepler > Developer note"
tags: [confluence, performance]
---

# Reproduce performance issue and understand the issue on DEV - A.Vu

*Confluence source · Team Kepler › Developer note · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/49384718375/Reproduce+performance+issue+and+understand+the+issue+on+DEV+-+A.Vu) · updated 2026-05-04*

Dear team,
As a DBA role, I capture database queries and behavior for each page, then analyze them with AI. There are results.

- Env: Dev, Tenant ID: bee43438-3eaf-4800-9e50-1eaf6626b1cc, Total records: 128.000

- Tool: MongoDB Compass

![[image-20260428-104259.png]]

- *Ps: Open the attachment file for more details.*

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p>**Page action**</p></th>
<th><p>**Database queries capture**</p></th>
<th><p>**Analyzing with AI**</p></th>
<th><p>**Summary**</p></th>
</tr>
&#10;<tr>
<td><p>The main page:<br />
</p></td>
<td><p>[[bee43438-3eaf-4800-9e50-1eaf6626b1cc.mongo_queries_eArchive_page.json|bee43438-3eaf-4800-9e50-1eaf6626b1cc.mongo_queries_eArchive_page.json]]</p>
<p> </p></td>
<td><p>[[eArchive_Page_Query_Analysis.md|eArchive_Page_Query_Analysis.md]]</p>
<p> </p></td>
<td>![[image-20260428-103332.png]]
<p> </p></td>
</tr>
<tr>
<td><p>The detail page:<br />
</p></td>
<td><p>[[bee43438-3eaf-4800-9e50-1eaf6626b1cc.mongo_queries_eArchive_detail_page.json|bee43438-3eaf-4800-9e50-1eaf6626b1cc.mongo_queries_eArchive_detail_page.json]]</p>
<p> </p></td>
<td><p>[[eArchive_Detail_Page_Query_Analysis.md|eArchive_Detail_Page_Query_Analysis.md]]</p>
<p> </p></td>
<td>![[image-20260428-103403.png]]
<p> </p></td>
</tr>
<tr>
<td><p>The folder page:<br />
</p></td>
<td><p>[[bee43438-3eaf-4800-9e50-1eaf6626b1cc.mongo_queries_eArchive_folder_page.json|bee43438-3eaf-4800-9e50-1eaf6626b1cc.mongo_queries_eArchive_folder_page.json]]</p>
<p> </p></td>
<td><p>[[eArchive_Folder_Page_Query_Analysis.md|eArchive_Folder_Page_Query_Analysis.md]]</p>
<p> </p></td>
<td>![[image-20260428-103430.png]]
<p> </p></td>
</tr>
<tr>
<td><p>The search page:<br />
</p></td>
<td><p>![[unknown-attachment.png]]</p>
<p> </p></td>
<td><p>[[eArchive_Search_Page_Query_Analysis.md|eArchive_Search_Page_Query_Analysis.md]]</p>
<p> </p></td>
<td>![[image-20260428-103452.png]]
<p> </p></td>
</tr>
</tbody>
</table>
