---
title: "OCR API Java Client"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488075820/OCR+API+Java+Client
space: "AI"
topic: programming
relevance: 0.871
depth: 3
updated: 2020-11-30
attachments: 3
tags:
  - confluence
  - programming
  - space/ai
---

# OCR API Java Client

> [!info] Imported from Confluence
> Space **AI** · updated 2020-11-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488075820/OCR+API+Java+Client)
> Relevance 0.871 · topic `programming`

# About this page

This is a short guide for getting started with the OCR Java Client.

# Introduction

Note that the Java Client depends on Apache CXF. Apache CXF is an open source web service framework from Apache Software Foundation.

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="eb02c215-597a-4f88-a06b-32cc4f4fa1a6" macro-name="toc">

</div>

# <span style="letter-spacing: 0.0px;">Maven Artifact</span>

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<tbody>
<tr>
<th colspan="3"><br />
</th>
<th colspan="2">Supported protocol version</th>
<th><br />
</th>
</tr>
<tr>
<th>Group ID</th>
<th>Artifact ID</th>
<th>Version</th>
<th>v1</th>
<th>v2</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td class="highlight-red confluenceTd"><del><span>com.axonivy.ai.ocr</span></del></td>
<td class="highlight-red confluenceTd"><del><span>com.axonivy.ai.ocr.ws.client</span></del></td>
<td class="highlight-red confluenceTd"><del>1.1</del></td>
<td class="highlight-red confluenceTd"><del>

![[2488075820-check.png]]

</del></td>
<td class="highlight-red confluenceTd"><br />
</td>
<td class="highlight-red confluenceTd">Deprecated</td>
</tr>
<tr>
<td class="highlight-red confluenceTd"><del>com.axonivy.ai.ocr</del></td>
<td class="highlight-red confluenceTd"><del>com.axonivy.ai.ocr.ws.client</del></td>
<td class="highlight-red confluenceTd"><del>1.2</del></td>
<td class="highlight-red confluenceTd"><del>

![[2488075820-check.png]]

</del></td>
<td class="highlight-red confluenceTd"><br />
</td>
<td class="highlight-red confluenceTd">Deprecated</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr</td>
<td>com.axonivy.ai.ocr.ws.client</td>
<td>1.3</td>
<td>

![[2488075820-check.png]]

</td>
<td>

![[2488075820-check.png]]

</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr</td>
<td>com.axonivy.ai.ocr.ws.client</td>
<td>...</td>
<td>

![[2488075820-check.png]]

</td>
<td>

![[2488075820-check.png]]

</td>
<td><br />
</td>
</tr>
<tr>
<td class="highlight-green confluenceTd">com.axonivy.ai.ocr</td>
<td class="highlight-green confluenceTd">com.axonivy.ai.ocr.ws.client</td>
<td class="highlight-green confluenceTd"><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="4570c5f6-025a-4b06-b08f-1750e08270f7" data-macro-name="status">RELEASED</span></p>
</div></td>
<td class="highlight-green confluenceTd">

![[2488075820-check.png]]

</td>
<td class="highlight-green confluenceTd">

![[2488075820-check.png]]

</td>
<td class="highlight-green confluenceTd">Recommended</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr</td>
<td>com.axonivy.ai.ocr.ws.client</td>
<td><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete aui-lozenge-subtle conf-macro output-inline" data-hasbody="false" data-macro-id="f50154bd-0841-4241-b033-ca1f40180ec4" data-macro-name="status">UNRELEASED</span></p>
</div></td>
<td>

![[2488075820-check.png]]

</td>
<td>

![[2488075820-check.png]]

</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# Using the Java Client

The following sample code gives a hint on how to use the OCR client.

There is a helper class named OcrFacadeBuilder that can be used to automatically create CXF REST proxies and to encapsulate those web services within the public OCR Facade API. Using that API has the big advantage that it is independent of the underlying protocol.

## Initialize client

First, you have to create an OCR facade. The OCR facade hides the CXF REST client proxy. (The username and password are just examples, and do not work.)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f65a0de5-9323-4cdc-a130-7683112a6ee4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.ocr.facade.api.OcrFacade;
import com.axonivy.ai.ocr.ws.client.OcrFacadeBuilder;


OcrFacade ocrFacade = OcrFacadeBuilder.newOcrFacade()
        .setAddress("https://axonivy.ai/ocr")
        .setUsername("james-bond")
        .setPassword("007")
        .build();
```

</div>

</div>

## Execute OCR request

Perform OCR and let the OCR engine create a searchable PDF, an image and a thumbnail.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b98a3b37-e332-4090-8cf6-2d0d7ace0be6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.common.util.analyze.BinaryEntity;
import com.axonivy.ai.common.util.analyze.BinaryEntityBuilder;
import com.axonivy.ai.ocr.common.OcrRequestBuilder;
import com.axonivy.ai.ocr.common.OcrRequest;
import com.axonivy.ai.ocr.common.OcrResponse;


File fileOfScannedDocument = new File("scan01.jpg");
BinaryEntity entityOfScannedDocument = BinaryEntityBuilder.newBinaryEntity(fileOfScannedDocument);

OcrRequest request = OcrRequestBuilder.newOcrRequest().setId("this-is-a-client-specific-request-identifier")
        .addInput(entityOfScannedDocument)
        .enablePdf(true)
        .enableImage(true)
        .enableThumbnail(true)
        .build();

OcrResponse response = ocrFacade.execute(request);
```

</div>

</div>

## Access OCR results

Finally, access the results as binary data from the output.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="53f1b6d4-2192-4e13-b0b7-8abcee1262d0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.ocr.common.OcrImageOption;
import com.axonivy.ai.ocr.common.OcrPdfOption;
import com.axonivy.ai.ocr.common.OcrThumbnailOption;


BinaryEntity ocrPdf = response.getOutput().find(OcrPdfOption.OUTPUT_NAME, BinaryEntity.class);
BinaryEntity ocrImage = response.getOutput().find(OcrImageOption.OUTPUT_NAME, BinaryEntity.class);
BinaryEntity ocrThumbnail = response.getOutput().find(OcrThumbnailOption.OUTPUT_NAME, BinaryEntity.class);
```

</div>

</div>
