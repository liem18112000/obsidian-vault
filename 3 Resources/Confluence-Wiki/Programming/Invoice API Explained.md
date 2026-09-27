---
ai_hash: f6e5031e9dd57fb1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.75
entities: []
relevance: 0.816
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076258/Invoice+API+Explained
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: Invoice API Explained
topic: programming
type: source
updated: 2019-02-11
---

# Invoice API Explained

> [!info] Imported from Confluence
> Space **AI** · updated 2019-02-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076258/Invoice+API+Explained)
> Relevance 0.816 · topic `programming`

# About this page

Explains the most relevant concepts and provides background and context.

# Introduction

The Invoice API provides invoice specific document analysis capabilities through an XML based REST interface.

It accepts OCR XML input and is able to predict invoice details and information about the creditor.

For convenience reasons, the Invoice API can be used to do OCR (See OCR API) and document analysis in one single call.

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="4c9c70f8-26d5-4f3c-85d4-f36bdfe9cf54" macro-name="toc">

</div>

# <span style="letter-spacing: 0.0px;">Limitations</span>

Version 1.x assumes that an invoice has only single VAT rate.

# <span style="letter-spacing: 0.0px;">UML Diagram</span>

The following diagrams display the structure of the LUZ request and response messages. Note that the LUZ request and response has the same structure as an OCR request and reponse. Have a look at the [OCR UML diagram](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2461991605/OCR+API) too.


![[2488076258-unknown-macro.png]]



The green Value classes are available since Version 1.1.


![[2488076258-unknown-macro.png]]



## LUZ Request

The LUZ request consists of one or more input entities (`AnalyzeEntity`) and a set of LUZ process options (`LuzOption`). There are two LUZ process options available:

- Predict option, to predict information about an Invoice
- Annotate option, to send back all annotation details

Both option require the ABBYY XML document as input. But because the LUZ request is able to incorporate an OCR request it can do the predictions based on the OCR result.

## LUZ Response

The LUZ response is a container of analyze results (`AnalyzeEntity`). There is a corresponding output entity for every particular LUZ process option:

- The predictions, according to the predict request option
- The annotations, according to the Image request option

Note that the LUZ response can contain the OCR results too, as described in the [OCR API](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2461991605/OCR+API).

## LUZ Predictions

The LUZ predictions contains all final predictions.

The prediction's **label** is a string that uniquely identifies the predicted type of information (e.g. TotalAmount or VatRate).

The prediction's **score** is a value between 0 and 1 that represents the probability that the predicted value is true.

The prediction's **ocrScore** is a value between 0 and 1 that represents the probability that the value has been extracted correctly. This value indicates potential OCR uncertanties: is a specific character an I (upper case i) or an l (lower case L)? Poor image quality may, e.g., also make an "8" look like a "6".

## LUZ Annotations

The LUZ annotations contains all document annotations that have been created during the analysis process.

An annotation references a spatial area in the document that has been enhanced with additional information.

## LUZ Options

A LUZ option represents an analysis output.

<div>

|          |                 |                   |                               |
|----------|:----------------|:-----------------:|-------------------------------|
| Name     | Entity name     | Enabled (default) | OCR XML Entity Name (default) |
| Annotate | luz-annotations |       false       | ocr.xml                       |
| Predict  | luz-predictions |       true        | ocr.xml                       |

</div>

%% ai-graph-start %%

**Related notes:**
- [[Invoice API Reference]]
- [[OCR API Explained]]
- [[Invoice API]]
- [[Analyze API v2 for Invoice prediction]]
- [[Invoice API Java Client]]

%% ai-graph-end %%