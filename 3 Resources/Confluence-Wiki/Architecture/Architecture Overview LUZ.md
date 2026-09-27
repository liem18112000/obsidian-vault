---
title: "Architecture Overview LUZ"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2507907231/Architecture+Overview+LUZ
space: "AI"
topic: architecture
relevance: 0.75
depth: 2.39
updated: 2020-02-11
attachments: 0
tags:
  - confluence
  - architecture
  - space/ai
---

# Architecture Overview LUZ

> [!info] Imported from Confluence
> Space **AI** · updated 2020-02-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2507907231/Architecture+Overview+LUZ)
> Relevance 0.75 · topic `architecture`

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
<th>Module</th>
<th>Description</th>
<th>Internal dependencies</th>
<th>Comment</th>
</tr>
&#10;<tr>
<td>com.axonivy.ai.checkstyle</td>
<td>Contains checkstyle and other settings for Eclipse</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.common.root</td>
<td>Root pom.xml for other packages</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.common.test</td>
<td>Module contains helper classes for tests</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.common.util</td>
<td>Global utility functions</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.dev</td>
<td>Scripts for development</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonviy.ai.karaf.auth</td>
<td>Authorization for Karaf</td>
<td>com.axonivy.ai.common.[test,util]</td>
<td><br />
</td>
</tr>
<tr>
<td><em><strong>com.axonivy.ai.karaf.feature</strong></em></td>
<td><em><strong>Karaf Feature</strong></em></td>
<td>com.axonivy.ai.karaf.[auth,osgi,mvn,ws]</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.karaf.mvn</td>
<td>Maven resolver</td>
<td>com.axonivy.ai.common.[test,util]</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.karaf.osgi</td>
<td>Karaf utilities for OSGi</td>
<td>com.axonivy.ai.common.[test,util]</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.karaf.ws</td>
<td>Karaf utilities for web services</td>
<td>com.axonivy.ai.common.[util,auth]</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.common</td>
<td>LUZ Common Library</td>
<td>com.axonivy.ai.common.[test,util]</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.common.avro</td>
<td>Avro add-on</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.common.xml</td>
<td>XML add-on</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.facade.api</td>
<td>Facade API</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.ocr.common</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.facade.local</td>
<td>Local LUZ Facade</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.luz.service.api</p>
<p>com.axonivy.ai.luz.facade.api</p>
<p>com.axonivy.ai.ocr.facade.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td><strong><em>com.axonivy.ai.luz.feature</em></strong></td>
<td><strong><em>AXON IVY AI LUZ Feature</em></strong></td>
<td><p>com.axonivy.ai.karaf.feature</p>
<p>com.axonivy.ai.ocr.feature</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.luz.facade.[api,local]</p>
<p>com.axonivy.ai.luz.service.[api,impl,pdf]</p>
<p>com.axonivy.ai.luz.service.uid.admin</p>
<p>com.axonivy.ai.luz.ui</p>
<p>com.axonivy.ai.luz.websocket</p>
<p>com.axonivy.ai.luz.ws.[api,client,server]</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.service.api</td>
<td>Service API</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.common</p>
<p>com.axonivy.ai.luz.common</p></td>
<td><br />
</td>
</tr>
<tr>
<td><p>com.axonivy.ai.luz.service.impl</p></td>
<td>Service implementation</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.luz.service.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.service.pdf</td>
<td>PDF parser library</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.luz.service.[api,impl]</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.service.uid.admin</td>
<td>UID lookup</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.luz.service.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.ui</td>
<td>Web application</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.facade.api</p>
<p>com.axonivy.ai.luz.common</p>
<p>com.axonivy.ai.luz.service.api</p>
<p>com.axonivy.ai.luz.facade.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.websocket</td>
<td>WebSocket Configuration</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.facade.api</p>
<p>com.axonivy.ai.ocr.ws.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.ws.api</td>
<td>LUZ Web Service API</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.ocr.common</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.ws.client</td>
<td>LUZ Web Service Client</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.facade.api</p>
<p>com.axonivy.ai.luz.ws.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.luz.ws.server</td>
<td>LUZ Web Service Server</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.luz.facade.api</p>
<p>com.axonivy.ai.luz.ws.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.common</td>
<td>OCR Common Library</td>
<td>com.axonivy.ai.common.[test,util]</td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.facade.api</td>
<td>OCR Facade API</td>
<td><p>com.axonivy.ai.common.util</p>
<p>com.axonivy.ai.ocr.common</p></td>
<td><p>Contains the interface OcrFacade.</p>
<p><strong><em>Not clear why dependency to com.axonivy.ai.common.util is needed, no usage found in interface.</em></strong></p></td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.facade.local</td>
<td>Local OCR Facade</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.common</p>
<p>com.axonivy.ai.ocr.facade.api</p>
<p>com.axonivy.ai.ocr.service.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td><strong><em>com.axonivy.ai.ocr.feature</em></strong></td>
<td><strong><em>AXON IVY AI OCR Feature</em></strong></td>
<td><p>com.axonivy.ai.karaf.feature</p>
<p>com.axonivy.ai.ocr.common</p>
<p>com.axonivy.ai.ocr.facade.[api,local]</p>
<p>com.axonivy.ai.ocr.service.[api,fre,nil]</p>
<p>com.axonivy.ai.ocr.ws.[api,client,server]</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.service.api</td>
<td>OCR Service API</td>
<td><p>com.axonivy.ai.common.util</p>
<p>com.axonivy.ai.ocr.common</p></td>
<td><p>Contains the interface OcrService.</p>
<p><strong><em>Not clear why dependency to com.axonivy.ai.common.util is needed, no usage found in interface.</em></strong></p></td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.service.fre</td>
<td>FineReader Engine OCR Service</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.common</p>
<p>com.axonivy.ai.ocr.service.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.service.nil</td>
<td>Skeleton OCR Service</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.common</p>
<p>com.axonivy.ai.ocr.service.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.ws.api</td>
<td>OCR Web Service API</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.common</p></td>
<td>Contains version 1.0 and 2.0 of the Web Service</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.ws.client</td>
<td>OCR Web Service Client</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.facade.api</p>
<p>com.axonivy.ai.ocr.ws.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ocr.ws.server</td>
<td>OCR Web Service Server</td>
<td><p>com.axonivy.ai.common.[test,util]</p>
<p>com.axonivy.ai.ocr.facade.api</p>
<p>com.axonivy.ai.ocr.ws.api</p></td>
<td><br />
</td>
</tr>
<tr>
<td>com.axonivy.ai.ops.exoscale</td>
<td>Exoscale tools/configs/scripts</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>
