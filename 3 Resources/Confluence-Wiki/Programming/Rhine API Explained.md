---
title: "Rhine API Explained"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479941083/Rhine+API+Explained
space: "AI"
topic: programming
relevance: 0.796
depth: 2.68
updated: 2025-09-22
attachments: 3
tags:
  - confluence
  - programming
  - space/ai
---

# Rhine API Explained

> [!info] Imported from Confluence
> Space **AI** · updated 2025-09-22 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479941083/Rhine+API+Explained)
> Relevance 0.796 · topic `programming`

<div class="contentLayout2">

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

# About this page

Explains the most relevant concepts and provides background and context.

# Introduction

The Rhine API is a gateway to ingest data, such as real-time streaming data, into the data lake.

**Topics** are the logical pathways to transport data. A topic is **a named stream of messages**. Messages can be published to topics using a <a href="https://axonivy.atlassian.net/wiki/display/AI/Rhine+API+Spec#RhineAPISpec-Producer" rel="nofollow">Producer</a>. <a href="https://axonivy.atlassian.net/wiki/display/AI/Rhine+API+Spec#RhineAPISpec-Consumer" rel="nofollow">Consumers</a> will receive all messages published to the topics to which they subscribe. A list of available topics can be found here: [Topics](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2473963955/Topics). A **message** is some sort of data structure.

# Table of Contents

<div class="toc-macro client-side-toc-macro conf-macro output-block" excludeheaderregex="Table of Contents" hasbody="false" headerelements="H1,H2,H3,H4" macro-id="efbf015f-9c29-44e3-82b8-3dea748fc9ad" macro-name="toc">

</div>

# Topic

All messages are organized into topics. If you wish to send a message you send it to a specific topic and if you wish to read a message you read it from a specific topic.

**A topic is bound to a data schema** so that all messages in a topic conform to a certain record type. This is similar to relational databases, where a table is a collection of records with the same type (i.e. the same set of columns), so we have an analogy between a relational table and a topic.

The data schema of a topic is allowed to evolve over time. Multiple compatible versions can exist in parallel.

</div>

</div>

</div>

<div class="columnLayout two-left-sidebar">

<div class="cell aside" data-type="aside">

<div class="innerCell">


![[2479941083-RhineApi_Topics.png]]



</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

*The figure shows a three streams of messages named A, B and C. Every messages must conform to the associated schema, but could reference an old version.*

*Messages can be published to topics using a Producer.*

*Consumers will receive all messages published to the topics to which they subscribe.*

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

# Message

A message is sent to a named topic and its data must conform to a specific version of the corresponding schema. A message is a data structure encoded as <a href="http://avro.apache.org/docs/current/spec.html" class="external-link" rel="nofollow">Apache Avro™ record</a>.

Every message represents an **event** and describes the new **state** of an **entity**. Three basic functions can change the state of an entity: **create**, **update** and **delete**.

> *Let's assume we have a topic for user session related events. We're interested in the creation of a new session when a user signs in and when the session is closed because of sign-out or timeout. Based on that stream of session events, we're able to calculate "how often KLARA is used", "the average amount of time a user spends on KLARA" and "how many different users are using KLARA per day".*

</div>

</div>

</div>

<div class="columnLayout two-left-sidebar">

<div class="cell aside" data-type="aside">

<div class="innerCell">


![[2479941083-RhineApi_Message.png]]



</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

*The figure shows a message that is sent to topic A. It contains a data record that uses version 2 of the associated schema A.*

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

A message is an umbrella for data that is sent to a topic. That data is encoded in Apache Avro format and has the technical name ‘record’. It must conform to the corresponding schema.

All records consists of a meta struct; an ID that uniquely identifies the entity, a method that identifies the cause of the message and a timestamp that indicates the point in time when the state change occurred. Other fields of the record are entity specific and depend on the schema.

# Record

A record contains generic [**meta** data](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2495686113/common.lake.Meta) and entity specific information. The meta data allows to perform generic preprocessing of the data stream. The entity specific information is needed to compute business related deep insights.

The record's meta data contains the field "**id**". The value of the field "id" uniquely identifies the entity that changed it's state.

> *Imaging a stream of events about user sessions. The* `meta.id` *field of a record contains the unique identifier of that user session. Every message containing data about the same user session has to use the identifier of that session as value for the* `meta.id` *field.*

The record's meta data contains the field "**method**". The value of that field identifies the trigger of the state change (create, update, delete).

> *One single user session can produce multiple events during its existence. The first message will represent the creation of the session upon user sign on and the last message will represent the deletion of the session because of sign-out or timeout. Any other event in between session start and end will indicate other relevant state changes. But messages that belong to the same user session use the same record identifier.*

The record's meta data contains the field "**timestamp**". The value of that field indicates the point in time when the state change occurred.

> The user session's CREATE message provides the real time when that session was created regardless of any latency caused by messaging. The session's UPDATE and DELETE messages provide the real time as well.

A record might provide business specific information as well. Every record will represent the relevant state of the entity at event time. The record represents the state, not the state change.

> *A session belongs to a user. Therefore a record about session events needs hold information about the user in its body.*

A record's meta data contains even more information, but that is described later.

# Subject

The term subject is used to name a specific set of objects of the same type.

There are different topics to ingest data for the same subject. Every topic has it’s own schema, but that schema references the same subject as body.

# Unique Identifiers

With reference to a set of objects, a unique identifier is used to **uniquely distinguish one object from another**. An object might change its state over time, **but its identifier must remain the same**.

An identifier that has once been assigned to an object **cannot be reused for another object** at any time later.

## <span id="RhineAPIExplained-SID" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="22b88582091886d3314df6ea02b38970" macro-name="anchor"><span id="SID" class="confluence-anchor-link"> </span></span>Subject Uniqueness (SID)

The main scope of uniqueness of an object identifier is its natural group, the set of objects of the same type (→ SID).

The identifier of data that is sent to a topic requires subject uniqueness.

The default form (non-qualified) looks as follows:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="154b65ea-258d-4123-9a9e-c9e267eb9072" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<SUBJECT ID>
```

</div>

</div>

or in full qualified form:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2ed25b68-d488-4296-9073-45c87d19b213" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SID : <SUBJECT ID>
```

</div>

</div>

If not explicitly specified, the short form (non-qualified form) is used.

## <span id="RhineAPIExplained-GID" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="f765aa40-015f-4cdf-8a22-670c59135ab1" macro-name="anchor"><span id="GID" class="confluence-anchor-link"> </span></span>Global Uniqueness (GID)

Identifiers that have to identify an object in a set of sets have to be globally unique (→ GID).

The definition of global uniqueness is primarily used the other way round. Referencing an object of an other type requires a global identifier. This global identifier has the form

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4bf74e40-210d-4165-b543-e842a974bb1c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
GID : <SUBJECT NAME> : <SUBJECT ID>
```

</div>

</div>

## <span id="RhineAPIExplained-REFID" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="17e207d9-51ca-4242-8193-6d06286ef445" macro-name="anchor"><span id="REFID" class="confluence-anchor-link"> </span></span><span id="RhineAPIExplained-REFIDS" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="f5b2d284977dc7414833088f49fffdb6" macro-name="anchor"><span id="REFIDS" class="confluence-anchor-link"> </span></span><span id="RhineAPIExplained-REFIDG" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="8f1413f9-2423-462c-a56e-7bba6c283b0e" macro-name="anchor"><span id="REFIDG" class="confluence-anchor-link"> </span></span>Reference Identifiers (REFID)

A reference identifier is pointing to another object using its identifier. Either the scope is <a href="#" rel="nofollow">subject (REFID:S)</a> or <a href="#" rel="nofollow">global (REFID:G)</a>.

# Serialization / Deserialization

All records are encoded in <a href="https://avro.apache.org/" class="external-link" rel="nofollow">Apache Avro™ format</a>. Avro has a JSON like data model, but can be represented as either JSON (please take care of the Avro specific encoding rules) or in a compact binary form. One of the critical features of Avro is the ability to define a schema for your data.

Read more about Apache Avro here:

<div>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p><strong>Apache Avro™ 1.11.1 Specification</strong>, <strong></strong> Apache Software Foundation, Aug 2025<br />
<a href="https://avro.apache.org/docs/1.11.1/specification/" class="external-link" rel="nofollow">https://avro.apache.org/docs/1.11.1/specification/</a></p></td>
</tr>
<tr>
<td><p><strong>Apache Avro™ 1.11.1 Getting Started (Java)</strong>, Apache Software Foundation, Aug 2025<br />
<a href="https://avro.apache.org/docs/1.11.1/getting-started-java/" class="external-link" rel="nofollow">https://avro.apache.org/docs/1.11.1/getting-started-java/</a></p></td>
</tr>
</tbody>
</table>

</div>

One of the interesting things about Avro is that it not only requires a schema during data serialization, but also during data deserialization. Because the schema is provided at decoding time, metadata such as the field names don’t have to be explicitly encoded in the data. This makes the binary encoding of Avro data very compact.

# Schema

Avro Schemas are defined using JSON. The advantage of having a schema is that it clearly specifies the structure, the type and the meaning (through documentation) of the data. 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b6b3d54e-8475-424c-ac5a-06f0ffc9e683" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
 "namespace": "example.avro",
 "type": "record",
 "name": "User",
 "fields": [
  {"name": "name", "type": "string"},
  {"name": "favorite_number",  "type": ["int", "null"]},
  {"name": "favorite_color", "type": ["string", "null"]}
  ]
}
```

</div>

</div>

An important aspect of data management is schema evolution. After the initial schema is defined, applications may need to evolve it over time. When this happens, it’s critical for the downstream consumers to be able to handle data encoded with both the old and the new schema seamlessly.

There are three common patterns of schema evolution:

- backward compatibility – means that data encoded with an older schema can be read with a newer schema.

- forward compatibility – means that data encoded with a newer schema can be read with an older schema.

- full compatibility – means schemas are backward and forward compatible.

> Assume in version 1 of the schema, our Employee record did not have an age factor. But now we want to add an age field with a default value of -1. So, let’s suppose we have a consumer using version 1 with no age and a producer using version 2 of the schema with age. Now, by using version 2 of the Employee schema the Producer, creates an Employee record sets age field to 42, then sends it to topic new-Employees. Afterwards, using version 1 the consumer consumes records from new-Employees of the Employee schema. Hence, the age field gets removed during deserialization just because the consumer is using version 1 of the schema.

# Schema Registry

The Schema Registry provides a versioned history of all schemas. The registry introduces operational efficiency by providing reusable schema and enabling data providers and consumers to evolve at different speed.

</div>

</div>

</div>

<div class="columnLayout two-left-sidebar">

<div class="cell aside" data-type="aside">

<div class="innerCell">


![[2479941083-RhineApi_SchemaRegistry.png]]



</div>

</div>

<div class="cell normal" data-type="normal">

<div class="innerCell">

*The figure shows the Schema Registry with three managed schemas named A, B and C.*

*Four different versions exists for schema A. B is still at its initial state and for schema C exists tree versions.*

</div>

</div>

</div>

<div class="columnLayout fixed-width">

<div class="cell normal" data-type="normal">

<div class="innerCell">

# Streaming vs Bulk load

The Rhine API is designed to receive events or notifications of events as they occur, as a (near) real-time stream of data. But the API allows to upload a complete data set at any time also.

If the value of the data decreases over time, processing data streams is a must. Here is a use cases that illustrates the value of streaming data processing:

> Use customer behavior to suggest additional products or services the user might be interested in during the same session

## Bulk ingestion

Bulk ingestion is used to send all data from source to AI Platform.

Every entity is sent as single message, but that message is logically bundled to a batch. Every batch has a unique identifier (`meta.batchId`) and all messages that belong to that batch must have that field set.

A batch is a sequence of messages. Every message is consecutively numbered (`meta.batchSequence`). A batch with a gap in the numbering is invalid and all messages that belong to that batch are ignored.

In case of a failure, the sender can rollback to a previous sequence number and resend data. Previously sent messages with reused sequence numbers are discarded by the AI Platform.

A batch must be confirmed with a final [Notification](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2513123174/klara.sns.rhine.Notification+OLD) after the last data record.

# Ingest data for testing

The Rhine API allows ingesting of data for testing purposes even for PROD environment. See `testCase` field in [meta](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2495686113/common.lake.Meta) structure.

# Related articles

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2485748224/How+to+create+a+Lake+table+in+Hive" id="2485748224">How to create a Lake table in Hive</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479930566/Trigger+Bulk+Load+for+a+Topic" id="2479930566">Trigger Bulk Load for a Topic</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2486599793/How+to+update+the+Schema+of+a+Lake+table+in+Hive" id="2486599793">How to update the Schema of a Lake table in Hive</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2479932812/How+to+get+specific+messages+from+Kafka" id="2479932812">How to get specific messages from Kafka</a>

  </div>

- <div>

  <span class="icon aui-icon content-type-page" title="Page">Page:</span>

  </div>

  <div class="details">

  <a href="https://axonivy.atlassian.net/wiki/spaces/AI/pages/2472018575/Install+Rhine+Service" id="2472018575">Install Rhine Service</a>

  </div>

  

</div>

</div>

</div>

</div>
