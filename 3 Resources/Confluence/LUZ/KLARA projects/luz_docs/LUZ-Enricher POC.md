---
title: "LUZ-Enricher POC"
created: 2024-03-13
updated: 2025-11-21
type: source
status: reference
source: "Confluence · LUZ - LUZ"
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47715024923/LUZ-Enricher+POC
confluence_id: "47715024923"
confluence_path: "LUZ Home > KLARA projects > luz_docs"
tags: [confluence, luz-docs, enricher]
---

# LUZ-Enricher POC

*Confluence source · LUZ Home › KLARA projects › luz_docs · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47715024923/LUZ-Enricher+POC) · updated 2025-11-21*

## Objective

Split the enricher flows from luz-docs into a distinct service module, serving three primary objectives:

1.  Offload luz-docs worker thread to other service module to enhance stabilitty.

2.  Separate down the enricher feature to reduce luz_docs code complexity.

3.  Allow other service modules independently invoke the complete enricher flow.

## Overview

![[image-20240314-064848.png]]

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p>**Type**</p></th>
<th><p>**Description**</p></th>
<th><p>**Input**</p></th>
<th><p>**Output**</p></th>
</tr>
&#10;<tr>
<td><p>ContentTypeEnricher</p></td>
<td><p>Detect content type of the document using Apache Tika™ toolkit</p></td>
<td><ol>
<li><p>File input stream</p></li>
<li><p>File name</p></li>
</ol></td>
<td><p>Content type string</p></td>
</tr>
<tr>
<td><p>DocumentDiscoverEnricher</p></td>
<td><p>Submit an job to the AI server to analyze the document</p></td>
<td><ol>
<li><p>File input stream</p></li>
<li><p>File metadata</p></li>
</ol></td>
<td><p>Analyze job result</p></td>
</tr>
<tr>
<td><p>EmailEnricher</p></td>
<td><p>…</p></td>
<td><p>…</p></td>
<td><p>…</p></td>
</tr>
<tr>
<td><p>ThumbnailEnricher</p></td>
<td><p>Generate thumbnails for the document</p></td>
<td><ol>
<li><p>File input stream</p></li>
<li><p>content type</p></li>
</ol></td>
<td><p>A zip file of thumbnail files and their recieved time in UTC format</p></td>
</tr>
<tr>
<td><p>TimeStampEnricher</p></td>
<td><p>Sign timestamp of the document using TSA server</p></td>
<td><ol>
<li><p>File input stream</p></li>
<li><p>number of retry</p></li>
</ol></td>
<td><p>Timestamp query file, timestamp reply file and their received time in UTC format</p></td>
</tr>
</tbody>
</table>

> [!note]
>
>
> # Tech stack:
>
> - Java 17 and Quarkus Framework
>
> - LUZ-JWT: utilized for authentication
>
> - LUZ-JSON Store (multi-tenancy): utilized for storing enrichment text results, with TTL config
>
> - LUZ-Storage (public directory): utilized for storing enrichment binary file results, with CleanUp cron-job config
>
> - Pub/Sub (LUZ-Messege Broker vs LUZ-Messege Receiver): utilized as a notification channel for LUZ-Enricher to communicate with other service module
>
>

## Idea

![[image-20240314-095705.png]]![[image-20251121-112445.png]]
