---
title: "Invoice API Reference"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076222/Invoice+API+Reference
space: "AI"
topic: programming
relevance: 0.784
depth: 2.8
updated: 2020-12-02
attachments: 0
tags:
  - confluence
  - programming
  - space/ai
---

# Invoice API Reference

> [!info] Imported from Confluence
> Space **AI** · updated 2020-12-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2488076222/Invoice+API+Reference)
> Relevance 0.784 · topic `programming`

<div hasbody="true" macro-id="2c69054a-fa09-4c31-a5f2-9c1859dcb2fd" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

Do not use any Invoice API endpoint to test validity of a request. Always validate your request against the XML schemas first.

</div>

</div>

The Invoice API consists of a single call to analyze a document. <span style="letter-spacing: 0.0px;">But for convenience reasons, it can be used to do OCR (See OCR API) and document analysis in one single call.</span>

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3" macro-id="aa85f563-d11b-4113-ae78-1346322398da" macro-name="toc">

</div>

# POST /

## Summary

Perform document analysis on a single document.

<div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Method</th>
<td colspan="2"><strong>POST</strong></td>
</tr>
<tr>
<th>URL</th>
<td colspan="2"><strong>/</strong></td>
</tr>
<tr>
<th>Headers</th>
<td>Authorization</td>
<td>Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==</td>
</tr>
<tr>
<th>Request Body</th>
<td>Content-Type</td>
<td>application/xml (<span class="nolink">https://axonivy.ai/xmlns/luz/Request/3.0</span>)</td>
</tr>
<tr>
<th>Response Body</th>
<td>Content-Type</td>
<td><span>application/xml (<span class="nolink">https://axonivy.ai/xmlns/luz/Response/2.0</span></span><span>)</span></td>
</tr>
<tr>
<th rowspan="4">Status Codes</th>
<td>200 OK</td>
<td>OK</td>
</tr>
<tr>
<td>400 Bad Request</td>
<td>Invalid Input</td>
</tr>
<tr>
<td>500 Internal Server Error</td>
<td>Analysis failed because of an unexpected error</td>
</tr>
<tr>
<td>503 Service Unavailable</td>
<td><p>Timeout. Execution exceeded maximum processing time.</p>
<p>See 'Retry-After' header.</p></td>
</tr>
</tbody>
</table>

</div>

Use a request timeout of 11 minutes on the client side in order to let the server always return a result within the limit of 10 minutes.

## Request Body

This snipped shows the body of an analysis request that uses the result of a previous OCR request as input.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="835a7740-6b54-4641-9705-af472e394583" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<?xml version="1.0" encoding="UTF-8"?>
<ns2:request xmlns="https://axonivy.ai/xmlns/ocr/Options/2.0"
        xmlns:ns2="https://axonivy.ai/xmlns/luz/Request/3.0"
        xmlns:ns3="https://axonivy.ai/xmlns/common/Entity/1.0"
        id="417f5357-89d4-4e6e-b6a2-4b1f30b72ff8">
    <ns2:options>
        <ns2:predictions enabled="true" ocrXmlEntityName="ocr.xml"/>
    </ns2:options>
    <ns2:input>
        <ns3:binary name="ocr.xml" contentType="application/vnd.abbyy-frengine+xml">
            <ns3:data>REZzaGFycCBWZXJzaW9uIDEuMzIuMjYwOCJVBERi0xLjQKJdP0zOEKJSBQ...</ns3:data>
        </ns3:binary>
    </ns2:input>
</ns2:request>
```

</div>

</div>

This snipped shows the body of an analysis request that embeds an OCR request.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6cc894a8-2ef2-41c9-b836-3741fcdbb3a2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<?xml version="1.0" encoding="UTF-8"?>
<ns2:request xmlns="https://axonivy.ai/xmlns/ocr/Options/2.0"
        xmlns:ns2="https://axonivy.ai/xmlns/luz/Request/3.0"
        xmlns:ns3="https://axonivy.ai/xmlns/common/Entity/1.0"
        id="417f5357-89d4-4e6e-b6a2-4b1f30b72ff8">
    <ns2:options>
        <ns2:ocr>
            <image enabled="false"/>
            <pdf enabled="true"/>
            <thumbnail enabled="false" width="64" height="90"/>
            <xml enabled="true"/>
            <properties>
                <property name="baz" value="123"/>
            </properties>
        </ns2:ocr>
        <ns2:predictions enabled="true" ocrXmlEntityName="ocr.xml"/>
        <ns2:annotations enabled="true" ocrXmlEntityName="ocr.xml"/>
        <ns2:properties>
            <ns2:property name="foo" value="123"/>
            <ns2:property name="bar" value="123"/>
        </ns2:properties>
    </ns2:options>
    <ns2:input>
        <ns3:binary name="scan-001.pdf" contentType="application/pdf">
            <ns3:data>JVBERi0xLjQKJdP0zOEKJSBQREZzaGFycCBWZXJzaW9uIDEuMzIuMjYwOC...</ns3:data>
        </ns3:binary>
    </ns2:input>
</ns2:request>
```

</div>

</div>

## Response Body

<span style="letter-spacing: 0.0px;">This snipped shows the body of an analysis response.</span>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bc58e96d-61c4-4f38-b271-472c53f76ed5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:response xmlns="https://axonivy.ai/xmlns/common/Entity/1.0"
        xmlns:ns2="https://axonivy.ai/xmlns/luz/Response/2.0"
        xmlns:ns3="https://axonivy.ai/xmlns/luz/Objects/1.0"
        id="417f5357-89d4-4e6e-b6a2-4b1f30b72ff8">
    <ns2:binaries>
        <binary name="digitec.pdf" contentType="application/pdf">
            <data>JVBERi0xLjQKJdP0zOEKJSBQREZzaGFycCBWZXJzaW9uIDEuMzIuMjYwOC4wICh2...</data>
        </binary>
    </ns2:binaries>
    <ns2:predictions>
        <ns3:prediction label="Item" score="0.0" ocrScore="1.0">
            <ns3:predictionSetValue>
                <ns3:prediction label="dueDate" score="0.3" ocrScore="0.7">
                    <ns3:dateValue>2017-01-01</ns3:dateValue>
                </ns3:prediction>
                <ns3:prediction label="ItemAmount" score="0.0" ocrScore="1.0">
                    <ns3:decimalValue>14.90</ns3:decimalValue>
                </ns3:prediction>
            </ns3:predictionSetValue>
        </ns3:prediction>
        <ns3:prediction label="ItemAmountList" score="0.0" ocrScore="1.0">
            <ns3:predictionListValue>
                <ns3:prediction label="ItemAmount" score="0.0" ocrScore="1.0">
                    <ns3:decimalValue>14.90</ns3:decimalValue>
                </ns3:prediction>
                <ns3:prediction label="ItemAmount" score="0.0" ocrScore="1.0">
                    <ns3:decimalValue>14.90</ns3:decimalValue>
                </ns3:prediction>
            </ns3:predictionListValue>
        </ns3:prediction>
        <ns3:prediction label="creditorUid" score="0.4" ocrScore="0.6">
            <ns3:textValue>CHE-133-444-444</ns3:textValue>
        </ns3:prediction>
        <ns3:prediction label="currency" score="0.1" ocrScore="0.9">
            <ns3:textValue>CHF</ns3:textValue>
        </ns3:prediction>
        <ns3:prediction label="documentId" score="0.2" ocrScore="0.8">
            <ns3:textValue>Doc-123</ns3:textValue>
        </ns3:prediction>
        <ns3:prediction label="dueDate" score="0.3" ocrScore="0.7">
            <ns3:dateValue>2017-01-01</ns3:dateValue>
        </ns3:prediction>
        <ns3:prediction label="invoiceDate" score="0.5" ocrScore="0.5">
            <ns3:dateValue>2017-01-01</ns3:dateValue>
        </ns3:prediction>
        <ns3:prediction label="isrCodeLine" score="1.0" ocrScore="0.0">
            <ns3:textValue>Unknown+ 123</ns3:textValue>
        </ns3:prediction>
        <ns3:prediction label="totalAmount" score="0.0" ocrScore="1.0">
            <ns3:decimalValue>14.90</ns3:decimalValue>
        </ns3:prediction>
        <ns3:prediction label="vatRate" score="0.0" ocrScore="1.0">
            <ns3:decimalValue>8.0</ns3:decimalValue>
        </ns3:prediction>
    </ns2:predictions>
    <ns2:annotations>
        <ns2:annotation>
            <ns2:page index="2">
                <ns2:rect x="100.0" y="200.0" width="300.0" height="500.0"/>
            </ns2:page>
            <ns2:node id="1-2-3" startIndex="9" endIndex="14">JUnit</ns2:node>
            <ns2:predictions>
                <ns3:prediction label="dueDate" score="0.3" ocrScore="0.7">
                    <ns3:dateValue>2017-01-01</ns3:dateValue>
                </ns3:prediction>
            </ns2:predictions>
        </ns2:annotation>
        <ns2:annotation>
            <ns2:page index="3">
                <ns2:rect x="100.0" y="200.0" width="300.0" height="500.0"/>
            </ns2:page>
            <ns2:node id="1"/>
            <ns2:predictions>
                <ns3:prediction label="Item" score="0.0" ocrScore="1.0">
                    <ns3:predictionSetValue>
                        <ns3:prediction label="dueDate" score="0.3" ocrScore="0.7">
                            <ns3:dateValue>2017-01-01</ns3:dateValue>
                        </ns3:prediction>
                        <ns3:prediction label="ItemAmount" score="0.0" ocrScore="1.0">
                            <ns3:decimalValue>14.90</ns3:decimalValue>
                        </ns3:prediction>
                    </ns3:predictionSetValue>
                </ns3:prediction>
            </ns2:predictions>
        </ns2:annotation>
    </ns2:annotations>
</ns2:response>
```

</div>

</div>

# XML Schemas

Find the XML Schemas in our Bitbucket repository:

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Common Util</th>
<td><p><a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.common.util/src/master/src/main/resources/com/axonivy/ai/common/util/DataType-1.0.xsd" class="external-link" rel="nofollow">com/axonivy/ai/common/util/DataType-1.0.xsd</a></p>
<p><a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.common.util/src/master/src/main/resources/com/axonivy/ai/common/util/Entity-1.0.xsd" class="external-link" rel="nofollow">com/axonivy/ai/common/util/Entity-1.0.xsd</a></p></td>
</tr>
<tr>
<th>OCR v2</th>
<td><p><a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.ocr/src/master/com.axonivy.ai.ocr.ws.api/src/main/resources/com/axonivy/ai/ocr/ws/api/Options-2.0.xsd" class="external-link" rel="nofollow">com/axonivy/ai/ocr/ws/api/Options-2.0.xsd</a></p></td>
</tr>
<tr>
<th>LUZ v3</th>
<td><p><a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.luz/src/master/com.axonivy.ai.luz.ws.api/src/main/resources/com/axonivy/ai/luz/ws/api/Objects-1.0.xsd" class="external-link" rel="nofollow">com/axonivy/ai/luz/ws/api/Objects-1.0.xsd</a></p>
<p><a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.luz/src/master/com.axonivy.ai.luz.ws.api/src/main/resources/com/axonivy/ai/luz/ws/api/Request-3.0.xsd" class="external-link" rel="nofollow">com/axonivy/ai/luz/ws/api/Request-3.0.xsd</a></p>
<p><a href="https://bitbucket.org/axonivy-prod/com.axonivy.ai.luz/src/master/com.axonivy.ai.luz.ws.api/src/main/resources/com/axonivy/ai/luz/ws/api/Response-2.0.xsd" class="external-link" rel="nofollow">com/axonivy/ai/luz/ws/api/Response-2.0.xsd</a></p></td>
</tr>
</tbody>
</table>

</div>
