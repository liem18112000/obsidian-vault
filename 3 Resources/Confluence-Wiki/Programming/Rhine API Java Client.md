---
ai_hash: 82ff8c6640747b37
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 7
depth: 3
entities: []
relevance: 0.871
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479942222/Rhine+API+Java+Client
space: AI
status: reference
tags:
- confluence
- programming
- space/ai
title: Rhine API Java Client
topic: programming
type: source
updated: 2021-12-06
---

# Rhine API Java Client

> [!info] Imported from Confluence
> Space **AI** · updated 2021-12-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479942222/Rhine+API+Java+Client)
> Relevance 0.871 · topic `programming`

# About this page

This is a short guide for getting started with the Rhine Java Client.

# Introduction

Note that the Java Client depends on Apache CXF. Apache CXF is an open source web service framework from Apache Software Foundation.

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="4c9c70f8-26d5-4f3c-85d4-f36bdfe9cf54" macro-name="toc">

</div>

# Maven Artifact

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
<th colspan="3"><p><br />
</p></th>
<th><p>Supported protocol version</p></th>
</tr>
<tr>
<th><p>Group ID</p></th>
<th><p>Artifact ID</p></th>
<th><p>Version</p></th>
<th><p>v1</p></th>
</tr>
&#10;<tr>
<td><p>com.axonivy.ai.rhine</p></td>
<td><p>com.axonivy.ai.rhine.ws.client</p></td>
<td><p>[1.0,2)</p></td>
<td><p>

![[2479942222-check.png]]

</p></td>
</tr>
</tbody>
</table>

</div>

# Using the Java Client

The following sample code gives a hint on how to use the Rhine client.

There is a helper class named RhineClient that can be used to automatically create CXF REST proxies and to encapsulate those web services within the public Rhine Facade API. Using that API has the big advantage that it is independent of the underlying protocol.

## Initialize client

First, you have to setup the RhineClient. Be aware that you need to know your personal credentials.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7a801bcf-820a-48d2-ad47-0120b24025fa" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.rhine.ws.client.RhineClient;

RhineClient rhineClient = new RhineClient();
rhineClient.setAddress("https://axonivy.ai/rhine/v1");
rhineClient.setUsername("alladin");
rhineClient.setPassword("open sesame");
```

</div>

</div>

## Retrieve a specific schema

With the above RhineClient we can try to access a schema. Be aware that you need to now the name of the schema.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="57461bb8-780c-45b5-97d2-812093318a1f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.rhine.facade.api.SchemaRegistry;
import org.apache.avro.Schema;

SchemaRegistry schemaRegistry = rhineClient.getSchemaRegistry();
Schema schema = schemaRegistry.getSchema("foo");
```

</div>

</div>

## Produce a message

Use the same RhineClient to create a Producer and to send a Message.

The following code will produce a single message and does not reuse the producer but closes it immediately after sending the message.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="023c0761-1fc8-4f72-8960-526b44688a84" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.rhine.common.Message;
import com.axonivy.ai.rhine.facade.api.Producer;
import com.axonivy.ai.rhine.facade.api.ProducerFactory;

Message message = createMessage("foo", schema);

ProducerFactory producerFactory = rhineClient.getProducerFactory();
try (Producer producer = producerFactory.getProducer()) {
    producer.send(message).get();
}
```

</div>

</div>

## Consume messages

Use the same RhineClient to create a Consumer and to poll for messages.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="437648a8-5fb8-4000-b331-e459f8ca23ab" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import com.axonivy.ai.rhine.common.Message;
import com.axonivy.ai.rhine.facade.api.Consumer;
import com.axonivy.ai.rhine.facade.api.ConsumerFactory;

ConsumerFactory consumerFactory = rhineClient.getConsumerFactory();
try (Consumer consumer = consumerFactory.getConsumer()) {
    consumer.subscribe(Arrays.asList("foo"));

    final List<Message> messageList = consumer.receive(5000L);
    messageList.forEach(m -> System.out.println(m.toString()));

    consumer.commit();
}
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Rhine API's Open API Documents]]
- [[Invoice API Java Client]]
- [[OCR API Java Client]]
- [[Rhine API Explained]]
- [[Invoice API]]

%% ai-graph-end %%