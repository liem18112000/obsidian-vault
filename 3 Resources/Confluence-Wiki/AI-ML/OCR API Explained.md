---
ai_hash: ab0ba538ec68b7c9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 2.72
entities: []
relevance: 0.782
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488075963/OCR+API+Explained
space: AI
status: reference
tags:
- confluence
- ai-ml
- space/ai
title: OCR API Explained
topic: ai_ml
type: source
updated: 2019-02-11
---

# OCR API Explained

> [!info] Imported from Confluence
> Space **AI** · updated 2019-02-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488075963/OCR+API+Explained)
> Relevance 0.782 · topic `ai_ml`

# About this page

Explains the most relevant concepts and provides background and context.

# Introduction

The OCR API provides generic OCR capabilities through an XML based REST interface.

It accepts PDF and Image input and is able to generate

- **Searchable PDF**
- Image – a **quality enhanced multi-page image** (de-skewed, reduced noise, enhanced contrast, correction distortions, etc).
- Thumbnail – a multi-page image with tiny representations of the individual pages (configurable dimension)
- XML Report – details about **document content and layout structure**

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="eb02c215-597a-4f88-a06b-32cc4f4fa1a6" macro-name="toc">

</div>

# UML Diagram

The following diagrams display the structure of the OCR request and response messages.


![[2488075963-ocr-uml.png]]



## OCR Request

The OCR request consists of one or more input document fragments (`AnalyzeEntity`) and a set of OCR process options (`OcrOption`). There are four OCR process options available:

- PDF option, to generate a searchable PDF
- Image option, to generate a quality enhanced multi page Image
- Thumbnail option, to generate multi-page thumbnail Image with configurable dimension
- XML option, to report document layout structure

## OCR Response

The OCR response is a container of binary results. Find a resulting document (`BinaryEntity`) for every particular OCR process option:

- A PDF, according to the PDF request option
- An image, according to the Image request option
- A thumbnail, according to the Thumbnail request option
- An XML document, according to the XML request option

## OCR Options

An OCR option represents an analysis output.

<div>

<table>
<tbody>
<tr>
<th rowspan="2">Name</th>
<th colspan="2" style="text-align: center;">Entity</th>
<th rowspan="2" style="text-align: center;">Enabled (default)</th>
</tr>
<tr>
<th style="text-align: center;">name</th>
<th style="text-align: center;">content type</th>
</tr>
&#10;<tr>
<td>Image</td>
<td style="text-align: center;">ocr.tiff</td>
<td style="text-align: center;">image/tiff</td>
<td style="text-align: center;">false</td>
</tr>
<tr>
<td>Pdf</td>
<td style="text-align: center;">ocr.pdf</td>
<td style="text-align: center;">application/pdf</td>
<td style="text-align: center;">false</td>
</tr>
<tr>
<td>Thumbnail</td>
<td style="text-align: center;">ocr-thumbnail.tiff</td>
<td style="text-align: center;">image/tiff</td>
<td style="text-align: center;">false</td>
</tr>
<tr>
<td>Xml</td>
<td style="text-align: center;">ocr.xml</td>
<td style="text-align: center;">application/vnd.abbyy-frengine+xml</td>
<td style="text-align: center;">false</td>
</tr>
</tbody>
</table>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Invoice API Reference]]
- [[OCR API Java Client]]
- [[Invoice API Explained]]
- [[Analyze API Explained]]
- [[OCR Command Line Interface]]

%% ai-graph-end %%