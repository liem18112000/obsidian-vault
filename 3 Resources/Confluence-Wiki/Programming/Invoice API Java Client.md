---
title: "Invoice API Java Client"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076239/Invoice+API+Java+Client
space: "AI"
topic: programming
relevance: 0.871
depth: 3
updated: 2020-11-30
attachments: 1
tags:
  - confluence
  - programming
  - space/ai
---

# Invoice API Java Client

> [!info] Imported from Confluence
> Space **AI** · updated 2020-11-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076239/Invoice+API+Java+Client)
> Relevance 0.871 · topic `programming`

# About this page

This is a short guide for getting started with the OCR Java Client.

# Introduction

Note that the Java Client depends on Apache CXF. Apache CXF is an open source web service framework from Apache Software Foundation.

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="4c9c70f8-26d5-4f3c-85d4-f36bdfe9cf54" macro-name="toc">

</div>

# <span style="letter-spacing: 0.0px;">Maven Artifact</span>

<div>

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr>
<th colspan="3"><br />
</th>
<th colspan="3">Supported protocol version</th>
<th><br />
</th>
</tr>
<tr>
<th>Group ID</th>
<th>Artifact ID</th>
<th>Version</th>
<th>v1</th>
<th>v2</th>
<th>v3</th>
<th><br />
</th>
</tr>
&#10;<tr>
<td class="highlight-red confluenceTd"><del><span>com.axonivy.ai.luz</span></del></td>
<td class="highlight-red confluenceTd"><del><span>com.axonivy.ai.luz.ws.client</span></del></td>
<td class="highlight-red confluenceTd"><del>1.1</del></td>
<td class="highlight-red confluenceTd"><del>

![[2488076239-check.png]]

</del></td>
<td class="highlight-red confluenceTd"><br />
</td>
<td class="highlight-red confluenceTd"><br />
</td>
<td class="highlight-red confluenceTd">Deprecated</td>
</tr>
<tr>
<td>com.axonivy.ai.luz</td>
<td>com.axonivy.ai.luz.ws.client</td>
<td>1.2</td>
<td><br />
</td>
<td>

![[2488076239-check.png]]

</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz</td>
<td>com.axonivy.ai.luz.ws.client</td>
<td>1.3</td>
<td><br />
</td>
<td>

![[2488076239-check.png]]

</td>
<td>

![[2488076239-check.png]]

</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz</td>
<td>com.axonivy.ai.luz.ws.client</td>
<td>...</td>
<td><br />
</td>
<td>

![[2488076239-check.png]]

</td>
<td>

![[2488076239-check.png]]

</td>
<td><br />
</td>
</tr>
<tr>
<td class="highlight-green confluenceTd">com.axonivy.ai.luz</td>
<td class="highlight-green confluenceTd">com.axonivy.ai.luz.ws.client</td>
<td class="highlight-green confluenceTd"><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="18808a85-7904-47b5-baac-04c283e9ab2b" data-macro-name="status">RELEASED</span></p>
</div></td>
<td class="highlight-green confluenceTd"><br />
</td>
<td class="highlight-green confluenceTd">

![[2488076239-check.png]]

</td>
<td class="highlight-green confluenceTd">

![[2488076239-check.png]]

</td>
<td class="highlight-green confluenceTd">Recommended</td>
</tr>
<tr>
<td>com.axonivy.ai.luz</td>
<td>com.axonivy.ai.luz.ws.client</td>
<td><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-complete aui-lozenge-subtle conf-macro output-inline" data-hasbody="false" data-macro-id="63e3fe8b-38f5-4833-946d-81455baed59f" data-macro-name="status">UNRELEASED</span></p>
</div></td>
<td><br />
</td>
<td>

![[2488076239-check.png]]

</td>
<td>

![[2488076239-check.png]]

</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# Using the Java Client

The following sample code gives a hint on how to use the client.

## Initialize Facade

First, you have to create a LUZ facade. The LUZ facade hides the CXF REST client proxy (The username and password are just examples, and do not work).

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f65a0de5-9323-4cdc-a130-7683112a6ee4" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
LuzFacade luzFacade = LuzFacadeBuilder.newLuzFacade()
        .setAddress("https://axonivy.ai/luz")
        .setUsername("james-bond")
        .setPassword("007")
        .build();
```

</div>

</div>

## Execute analysis request

Perform document analysis. But because we use an image as input, we have to do OCR first. Everything is done in one single call.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b98a3b37-e332-4090-8cf6-2d0d7ace0be6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
File fileOfScannedDocument = new File("scan01.jpg");
BinaryEntity entityOfScannedDocument = BinaryEntityBuilder.newBinaryEntity(fileOfScannedDocument);

OcrRequest ocrRequest = OcrRequestBuilder.newOcrRequest().setId("this-is-a-unique-request-identifier")
        .addInput(entityOfScannedDocument)
        .enablePdf(true)
        .enableXml(true)
        .enableImage(true)
        .enableThumbnail(true)
        .build();

LuzRequest request = newLuzRequest().add(ocrRequest)
        .enablePrediction(true)
        .build();

LuzResponse response = luzFacade.execute(request);
```

</div>

</div>

## Access OCR results

Finally, you can get the binary results the same way as for a OCR request. Note that `BinaryEntity` implements the <a href="https://docs.oracle.com/javase/8/docs/api/javax/activation/DataSource.html" class="external-link" rel="nofollow"><code>javax.activation.DataSource</code></a> interface.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="53f1b6d4-2192-4e13-b0b7-8abcee1262d0" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
BinaryEntity ocrPdf = response.getOutput().find(OcrPdfOption.OUTPUT_NAME, BinaryEntity.class);
BinaryEntity ocrXml = response.getOutput().find(OcrXmlOption.OUTPUT_NAME, BinaryEntity.class);
BinaryEntity ocrImage = response.getOutput().find(OcrImageOption.OUTPUT_NAME, BinaryEntity.class);
BinaryEntity ocrThumbnail = response.getOutput().find(OcrThumbnailOption.OUTPUT_NAME, BinaryEntity.class);
```

</div>

</div>

## Access predictions

The set of final predictions is accessible the same way. 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dced6f07-05c4-4484-8de3-7c3982eeb1cd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
LuzPredictions predictions = output.find(LuzPredictOption.OUTPUT_NAME, LuzPredictions.class);
for (Prediction prediction : predictions) {
    System.out.println(prediction.toString());
}
```

</div>

</div>
