---
title: "OCR Command Line Interface"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2468094476/OCR+Command+Line+Interface
space: "AI"
topic: programming
relevance: 0.818
depth: 3
updated: 2019-02-08
attachments: 0
tags:
  - confluence
  - programming
  - space/ai
---

# OCR Command Line Interface

> [!info] Imported from Confluence
> Space **AI** · updated 2019-02-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2468094476/OCR+Command+Line+Interface)
> Relevance 0.818 · topic `programming`

The OCR Runner command line interface is available for developers to convert documents into searchable PDFs, XML for further content analysis and Images. The OCR Runner CLI is designed to process document corpora but can also be used to test various OCR tuning parameters.

## Prerequisites

You need the following tools

- Maven (Version 3.5.0)
- Java SE (Version 1.8.0)
- Git (Version 2.13)

You also need access to

- Bitbucket Repository (<a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.luz" class="external-link" rel="nofollow">AXON IVY PRODUCTION / Artificial Intelligence / com.axonivy.ai.luz</a>)
- Artifactory (<a href="https://repo.axonivy.io/" class="external-link" rel="nofollow">https://repo.axonivy.io/</a>)
- Invoice API Endpoint

You need to configure Maven to use Artifactory (settings.xml)

## Step 1 – Get AXON IVY :: AI :: LUZ

Clone the repository

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f589831b-4ac2-4760-b5ff-2ed3628f9b4c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$ git clone git@bitbucket.org:axonivy-prod/com.axonivy.ai.luz.git
```

</div>

</div>

Build playground module

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a819cbc4-09f2-435a-a65e-7b7b14e3adda" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$ cd com.axonivy.ai.luz/com.axonivy.ai.luz.playground
$ mvn -Pfast
```

</div>

</div>

## Step 2 – Prepare your Data

The OCR Runner needs to know the following information

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>basedir</th>
<td><p>The base directory that contains your input data and where the output data gets written to</p>
<p>e.g. /home/james/corpora</p></td>
</tr>
<tr>
<th>corpus</th>
<td><p>The name of the corpus to use. This name denotes a subfolder in <code>basedir</code>.</p>
<p>e.g. ComplexInvoices_v1</p>
<p>The full qualified path to the corpus will then be /home/james/corpora/ComplexInvoices_v1</p></td>
</tr>
<tr>
<th>sourceDir</th>
<td><p>The relative path to the input data</p>
<p>e.g. Source</p>
<p>The full qualified path to the input data will then be /home/james/corpora/ComplexInvoices_v1/Source</p></td>
</tr>
<tr>
<th>targetDir</th>
<td><p>The relative path where the OCR Runner should write its output to</p>
<p>e.g. OCR_TextOnly</p>
<p><span>The full qualified path to the output data will then be /home/james/corpora/ComplexInvoices_v1/OCR_TextOnly</span></p></td>
</tr>
</tbody>
</table>

</div>

## Step 3 – Execute the OCR runner without any parameters

You can execute the OCR runner without any paramters in order to get the usage help.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f7c0d939-dd3c-4a02-ad4f-921e65d7b122" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$ mvn exec:java@ocr
```

</div>

</div>

Note that you also need to know the following information

- serverUrl
- username
- password

## Step 4 – Execute OCR runner to get Text Only PDF documents

The OCR runner is executed with Maven. Therefore we have to embed all the arguments into a `-Dexec.args`.

Use a 'tuning parameter' to get Text Only PDF documents. 

Use the `limit` parameter to process only a few documents.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c35996b8-75c3-49fc-8756-189fa808516b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
$ mvn exec:java@ocr
        -Dcom.abbyy.FREngine.PDFExportMode=PEM_TextOnly
        -Dexec.args="
                --serverUrl https://unstable.axonivy.ai/luz
                --username james
                --password secretagent
                --baseDir /home/james/corpora
                --corpus ComplexInvoices_v1
                --sourceDir Source
                --targetDir OCR_TextOnly
                --limit 3"
```

</div>

</div>

Note that the line breaks are just for readability. You need to remove them for execution.

Note that the user 'james' with password 'secretagent' does not exist.

## Related articles

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/49102913648/How+to+check+the+availability+of+ABBYY+Licensing+Service" id="49102913648">How to check the availability of ABBYY Licensing Service</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479922084/ABBYY+FREngine+Reporting" id="2479922084">ABBYY FREngine Reporting</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2472018480/Rhine+API" id="2472018480">Rhine API</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2530751362/Analyze+API" id="2530751362">Analyze API</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/47350808577/SchemaRegistry+API" id="47350808577">SchemaRegistry API</a>

  </div>
