---
ai_hash: e54729066d06c88a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 51
depth: 2.3
entities: []
relevance: 0.731
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46999536449/Recipe+Best+practices+implementing+scalable+distributed+applications+on+cloud
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: 'Recipe: Best practices implementing scalable distributed applications on cloud'
topic: architecture
type: source
updated: 2025-07-29
---

# Recipe: Best practices implementing scalable distributed applications on cloud

> [!info] Imported from Confluence
> Space **LUZ** · updated 2025-07-29 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46999536449/Recipe+Best+practices+implementing+scalable+distributed+applications+on+cloud)
> Relevance 0.731 · topic `architecture`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh conf-macro output-inline" hasbody="false" macro-id="758af5b2-faed-4fe8-97b4-4c7a9c7dcc96" macro-name="status">WORK IN PROGRESS</span>

This receipt describes some best practices to implement scalable and distributed applications based on architectural styles and patterns which are applicable to cloud platforms and environments e.g. GCP, Azure, AWS etc.. Hence it is possible to scale your applications depends on the business requirements and needs.

The following types of scalings are available:

**Vertical scaling** refers to increasing the hardware infrastructure, such as increasing the CPU processing power, or increasing the amount of physical memory available to the application. Clearly, there are limits to such an approach (eg. define memory and CPU assignments).

**Horizontal scaling** refers to the possibility of dynamically increasing or decreasing the number of instances of an application, depending on the system needs (eg. increase or decreasing number Pods)

Since there are many architectural styles and patterns available we will focus on the most important styles and patterns which can be applied to KLARA applications running on cloud platform.

- N-Tier architecture (out of scope)

- Web API (Microservices)

- CQRS (Command Query Responsibility Segregation) (out of scope)

- Event Driven architecture (Pub / Sub)

- Big data architecture (out of scope)

- Big compute architecture (out of scope)

- (Cron-)Job / Batch

- Serverless

- etc.

## Changes applied to applications running on cloud

As cloud is changing the way applications are designed. Instead of monoliths, applications are decomposed into smaller, decentralized services. These services communicate through **APIs** or by using **asynchronous messaging or eventing**. Applications **scale horizontally**, adding new instances as demand requires.

These trends bring new challenges. Application state is **distributed**. Operations are done in **parallel and asynchronously**. The system as a whole must be **resilient** when failures occur.

### Comparisons between traditional on-premises and cloud architecture

<div>

|  |  |
|----|----|
| **Traditional on-premises** | **Modern cloud** |
| Monolithic, centralized | Decomposed, de-centralized |
| Design für predictable scalability | Design for elastic scale |
| Relational database | Polyglot persistence (mix of storage technologies) |
| Strong consistency | Eventual consistency |
| Serial and synchronized processing | Parallel and asynchronous processing |
| Design to avoid failures (MTBF - Mean Time Between Failures) | Design for failure (MTTR - Mean Time To Repair) |
| Occasional big updates | Frequent small updates |
| Manual management | Automated self-managment |
| Snowflakes servers (a server that requires special configuration beyond that covered by automated deployment scripts) | Immutable infrastructure (servers (or VMs) that are never modified after deployment) |

</div>

## Architectural styles and patterns (decision matrix)

In this section we are focusing on each architectural styles and patterns mentioned above. Those styles and patterns can be combined together depends on the business needs/reqirements and to build scalable applications.


![[46999536449-Top level data transfer.png]]



(do be deleted??)

<div>

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
<col style="width: 11%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Architecture styles and patterns</strong></p></th>
<th><p><strong>Web API (sync)</strong></p></th>
<th><p><strong>Web API</strong></p>
<p><strong>reactive/streams (async)</strong></p></th>
<th colspan="2"><p><strong>Event driven</strong></p></th>
<th><p><strong>(Cron)Job / Batch</strong></p></th>
<th><p><strong>Serverless (FaaS)</strong></p></th>
<th colspan="2"><p><strong>Concurrency (Java)</strong></p></th>
</tr>
&#10;<tr>
<td><p><strong>Service characteristics / factors</strong></p></td>
<td><p><strong>MQ</strong><br />
<strong>(DB Table)</strong></p></td>
<td><p><strong>MQ</strong><br />
<strong>(Pub/Sub)</strong></p></td>
<td><p><strong>Executor Service</strong></p></td>
<td><p><strong>Executor Service</strong><br />
<strong>(Scheduled)</strong></p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>High data volume</p></td>
<td></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Data integrity</p></td>
<td><p>not guaranteed, the more services and distributed</p></td>
<td><p>not guaranteed, the more services and distributed</p></td>
<td><p>not guaranteed, depends on load</p></td>
<td><p>not guaranteed, depends on load</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Real time (high velocity)</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Eventual consistency (async)</p></td>
<td></td>
<td></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(&lt;100 ms)</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Decoupled (async)</p></td>
<td></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>High workload (throughput)</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(only within a pod)</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(only within a pod)</p></td>
</tr>
<tr>
<td><p>Traffic steady</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Traffic unpredictable growth (peaks)</p></td>
<td></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Scalability</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(only within a pod)</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(only within a pod)</p></td>
</tr>
<tr>
<td><p>High availability</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Resiliency (Fault tolerant)</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(microservice)</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(microservice)</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p>Reliability</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(microservice)</p></td>
<td><p>

![[46999536449-check.png]]

</p>
<p>(microservice)</p></td>
<td></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td><p>

![[46999536449-check.png]]

</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="9"><p>Costs (<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="14939c8b-e433-4460-bc12-70401320bc10" data-macro-name="status">HIGH</span> / <span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="3faea4f2-7f2c-4769-ac0c-b9563d4ed6e9" data-macro-name="status">LOW</span> )</p></td>
</tr>
<tr>
<td><p>Infrastructure</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="9e16e0fc-2bf3-4ccd-8b67-b1e0002c3bff" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="db777779-31eb-4fb8-b21c-2fa84e06726f" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="988bd2db-8b09-4086-b051-579c8a6e30f1" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="c689f749-53ec-45ed-89e0-8dbaf47a73d1" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="93fe77c1-4698-42f4-af2f-563ed6ebbe44" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="9d70a70d-0899-4bf5-9457-4a33d97e25ac" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="84f52f83-8edc-4df8-9fe0-2c019bcd2efc" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="1cd84f10-27ca-46e4-a175-f50c0cf55c1d" data-macro-name="status">LOW</span></p></td>
</tr>
<tr>
<td><p>Development</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="ab66cc56-4bcf-4ba8-9a91-f0d353e92a8e" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="36bbf1ca-d631-42c9-8621-f5576cfc3189" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="3ec506b4-d39f-4d2d-a03b-fd10f0814983" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="9c45eb0c-d031-47fd-b52e-20d7d02e2b1b" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="7b647800-22b9-42c1-88a7-2dff94f02438" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="841b94c9-d9fb-4b7d-a599-7431e1ad14b4" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="0f5f9acd-fec5-4598-9533-9ebda30ac076" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="33d5d6f7-d027-4726-91b6-96ffe4464607" data-macro-name="status">HIGH</span></p></td>
</tr>
<tr>
<td><p>Maintenance</p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="b2c20a82-4025-4ba8-b1c2-80b29c8087ec" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="d95afca3-bb3d-4e9d-ab30-ca8c5947379e" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="829939df-ff98-4d4d-b95b-a59f0c7b4829" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="ff2d7494-5529-47ec-9fa2-83d31fd061b2" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" data-hasbody="false" data-macro-id="e7079c3b-949f-4f28-8941-9871594a679e" data-macro-name="status">HIGH</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="225d3ae9-7b79-4314-a3d1-4594952229ce" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="6a4cb3bc-b5d6-4334-823e-c8492d1bdae5" data-macro-name="status">LOW</span></p></td>
<td><p><span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-success conf-macro output-inline" data-hasbody="false" data-macro-id="5f48677e-68d6-4229-9111-090318792851" data-macro-name="status">LOW</span></p></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>

### Web API (synchronous)

The client calls a service and waits for a response. The response returns immediately (t \< 100ms). This architectural style is well know there no additional explanations are needed.


![[46999536449-Synchronous.png]]



#### Pros and cons


![[46999536449-check.png]]

 Instant reply, fast (\< 100ms)  

![[46999536449-check.png]]

 Consistent  

![[46999536449-check.png]]

 Testing  

![[46999536449-check.png]]

 Fault tolerant  

![[46999536449-check.png]]

 High scalability


![[46999536449-error.png]]

 Coupling Loss of service (Service down)  

![[46999536449-error.png]]

 Loss of data (Service down)  

![[46999536449-error.png]]

 Blocking worker threads  

![[46999536449-error.png]]

 Increase complexity the more services

#### Field of usage

- Web API for UI

- Resource APIs

#### Sample implementation

### Web API (asynchronous, reactive)

The client calls a service and waits for a response. The response will be returned at once, in chunks of items or as a stream (t \< 100ms). For this implementation a specific reactive library (e.g. Eclipse vert.x, Spring WebFlux etc.) for client and server need. Keep in mind that implementing reactive APIs will bring some challenges frontend and backend side. Therefore use this pattern only if really needed.  
The reactive approaches can also be used for database. In this case the data will returned as stream.  
For single response there is only one HTTP response available.  
To return a list of items the responses look as follow:


![[46999536449-Web API reactive.png]]



#### Pros and cons


![[46999536449-check.png]]

 Instant reply, fast (\< 100ms)  

![[46999536449-check.png]]

 Consistent  

![[46999536449-check.png]]

 Testing  

![[46999536449-check.png]]

 Non blocking I/O threads  

![[46999536449-check.png]]

 Fault tolerant  

![[46999536449-check.png]]

 High scalability


![[46999536449-error.png]]

 Coupling  

![[46999536449-error.png]]

 Loss of service (Service down)  

![[46999536449-error.png]]

 Loss of data (Service down)  

![[46999536449-error.png]]

 More implementation effort Increase complexity the more services

#### Field of usage

- Web API for UI with better user experiences

- Resource APIs, if clients can handle it

#### Sample implementation

### Event driven

Implementing the event driven architectural style and pattern the messages of a queue can either be pulled by subscribers or the messages can be pushed to subscribers. The acknowledgements will be sent back to the Pub/Sub system and the message will be removed from the queue. The communication can be done from one publisher to many subscriber (one-to-many, fan-out), from many publisher to one subscriber (many-to-one, fan-in) or from many publisher to many subscriber (many-to-many).


![[46999536449-PCP Pub Sub.png]]



There are two approaches to implement the event driven architectural style and pattern:

- Implementing a queue by your own (challenging)

- Using existing cloud technologies e.g. GCP Pub/Sub , Kafka etc.

Let’s focus on existing GCP Pub / Sub.

#### Pros and cons


![[46999536449-check.png]]

 Improved flexibility and maintainability (Separation of Concerns)  

![[46999536449-check.png]]

 High scalability (create additional instances to handle the queue with high load)  

![[46999536449-check.png]]

 High availability (queuing requests)  

![[46999536449-check.png]]

 Good reliability and robust (decoupled)  

![[46999536449-check.png]]

 Fault tolerant  

![[46999536449-check.png]]

 Good performance (async steps, no waits, but no real time)


![[46999536449-error.png]]

 Eventual consistency enough?  

![[46999536449-error.png]]

 No real time  

![[46999536449-error.png]]

 Testing  

![[46999536449-error.png]]

 Transaction based mechanism because of decoupled and independent modules/components

#### Field of usage

- Processings which are time consuming (eventual consistency t \< 100ms (GCP documentation))

- Processings with unpredictable growth

- Multiple subsystems must process the same events

- Processing high volume and high velocity of data

- Processing huge amount of messages (e.g. multiple tenants)

- Prioritizing processings

#### Sample implementation

### (Cron-)Job / Batch

Jobs or batches are used to process a huge amount of data controlled by time or interval. The processings can be done during edge times or at weekends.


![[46999536449-Job Batch.png]]



#### Pros and cons


![[46999536449-check.png]]

 Can be run during evenings or weekends  

![[46999536449-check.png]]

 Using resources (CPU and memory) while system is less under load


![[46999536449-error.png]]

 Long running processes  

![[46999536449-error.png]]

 Doesn’t scale  

![[46999536449-error.png]]

 Requires admin access or service account access

#### Field of usage

- Admin processing (e.g. Housekeeping)

- DB Migrations

- Processing high volumes (can be repetitive)

- Scripts (manual executions)

- Non transactional processings

#### Sample implementation

### FaaS

FaaS is a scalable pay-as-you-go functions as a service to run the code (logic) with no server management. Features:

- No server to provision, manage or update

- Automatically scale base on the load

- Integrated monitoring, logging and debugging capability

- Built-in security at role per function level based on the principle of least privilege

- Key networking capabilities for hybrid and multi-cloud scenarios


![[46999536449-FaaS event-driven.png]]



#### Pros and cons


![[46999536449-check.png]]

 Dedicated infrastructure (cloud)  

![[46999536449-check.png]]

 High scalability  

![[46999536449-check.png]]

 High availability  

![[46999536449-check.png]]

 No server management


![[46999536449-check.png]]

 

![[46999536449-error.png]]

 Pay per execution (Pricy?)  

![[46999536449-error.png]]

 Startup / warmup

#### Field of usage

- Complex calculations (high CPU Load or high memory consumption)

- Serverless webhooks (e.g. Slack integration → post message to Slack, notifications, alerts etc.)

- Real-time data processing

- AI and Big Query

#### Sample implementation

### Concurrency - Executor Service

Please check the following link [Concurrency and parallelism in Java](https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/09/25/46962115395/Concurrency+and+parallelism+in+Java) (section: Executor Service and Thread Pool) for further explanation.

Try to avoid the usage of this architectural pattern in could environments since it could cause problems regarding horizontal scaling. In case of service unavailability (e.g. service crash/down) all the blocking request and the data will be lost.

Parallel execution should not be done inside the JVM but distributed to other pods

1.  avoid hot spot which can’t be scaled (see luz-comp, salary run)

2.  there is no starvation of other requests going into Pod A

3.  so that a horizontal scaling is possible.

## Use cases

1\. Salary run at luz_compensation (high CPU load)  
**Problem statement**: Service calls are heterogenous (duration, cpu consumption, memory usage vary extremely). So if the salary run service is executed (internally multiple threads are spawn) it uses up 14 cpu of 16 - in some cases for quite some time. This will cause a cpu starvation for the other service calls.


![[46999536449-luz_compensation_cpuload_memoryleak.png]]



  
**Solution**: The goal is that the service calls are homogenous, so that each thread receives the same amount of CPU and possibly uses the worker threads (=execution of the call) for a “short time”. Further the request should not block the available worker threads (default approx 100). So that leads to the first conclusion, that the salary run has to be separated. Focusing on the salary run we realize that it can take from 1 minute to 100 minutes. Since synchronous calls lasting 100min do not make sense, this call has to be executed asynchronously. A possible solution would be to queue up the requests and process them by a predefined number of pods.

2\. Document zip upload with thumbnails and metadata generation at luz_docs (long running processing, response timeouts, pod upscaling)  
**Problem statement**: Consists of 3 main tasks: a) storing the document to GCS and generate metadata, b) create thumbnail enrichment c) run AI enrichment. c) takes 10s or even longer if the requests are queued up. b) and c) each can be run asynchronously


![[46999536449-luz-docs_workflow.png]]



**Upload document .zip or file (createDocument API call)**  
1. Validate document metadata (throws exception in case of validation errors)  
2. Extract documents from multipart  
3. Check if document is temporary  
4. Get document file name  
5. Save reference file docs to temp folder (/luz_docs/reference-files/) with UUID  
6. Virus scan reference file docs in temp folder (Virus scan enabled)  
7. Store json metadata to mongoDB (retrieve document id)  
8. Upload files encrypted document to GCS (Google Cloud Storage) with tenant id and document id  
9. Trigger document created event and delete reference file (async, Enricher)  
10. (Optional) Create audit log (async)  
11. Return Json object of the metadata and links of the documents  

**Solution**: There is not only one solution for this use case. It make sense to have a view on each components or modules. The following steps has been identified in detail:

- luz-docs implements (1-8)

- Extract enricher to separat pode

- Create a message queue with 2 subscriptions:

  - Subscription 2 for document enricher (pull, because of bottleneck)

  - Subscription 2 for audit log (pull/push)


![[46999536449-luz-docs possible solution.png]]



Points to be discusses:

- Streaming document to antivirus scanning REST API possible, instead of persist them to temp folder?

- Scalability of antivirus scanning, json store and GCS doc upload?

- JSON metadata has to be return back to caller synchronously? The idea is to update the JSON metadata which is located in jsonstore (mongodb) increasingly when the information are available from antivirus and gcs.

3\. Sending huge amount of (news-)letters to multiple ePost receivers (~100000 users) with different documents takes time (high latencies or even response timeouts).  
**Problem statement**: This issue can be separated in 2 main task: i) Sending of 100k letters which ii) involves execution of 100k x point 2. (see above).  
**Solution**: …

4\. Execute invoice run to generate billings with payment slips to KLARA customers

5\. Execute salary run to generate payroll for all employees

6\. Customer address sync with IRENE

7\. Batch-/Cron-Jobs which perform lookups to all tenants and processing things

------------------------------------------------------------------------

## Appendix

### A1: Flow charts


![[46999536449-Decision.png]]




![[46999536449-ggg.png]]



### A2: Google Cloud

#### **Deciding where to run your code on Google Cloud?**


![[46999536449-How_I_built_it__A_“Hello__World”_web_application_on_Google_Cloud_Platform___Goog.png]]



Reference. <a href="https://cloud.google.com/blog/products/gcp/time-to-hello-world-vms-vs-containers-vs-paas-vs-faas" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/blog/products/gcp/time-to-hello-world-vms-vs-containers-vs-paas-vs-faas</a>

#### GCP - Decision Tree


![[46999536449-Choosing_the_right_compute_option_in_GCP__a_decision_tree___Google_Cloud_Blog.pn]]



Reference: <a href="https://cloud.google.com/blog/products/compute/choosing-the-right-compute-option-in-gcp-a-decision-tree" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/blog/products/compute/choosing-the-right-compute-option-in-gcp-a-decision-tree</a>

### A3: Decision tree

**To be discussed!!**


![[46999536449-Field of usage - decision tree.png]]



## Open points

- UI experience: Response time limits (<a href="https://www.nngroup.com/articles/response-times-3-important-limits/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.nngroup.com/articles/response-times-3-important-limits/</a> )

- Decision tree: UI involved or not?

- Decision tree: Response result needed? Fire and forget?

- Decision tree: Data integrity and constistency. In case of server crashes, rebalancing, restart etc.

- Which part of the call can be made async?

- How to optimize memory e.g. large documents in memory etc.?

## TODO

- Finalize use cases

- Add use cases for each architecture variants

## Links

- [Concurrency and parallelism in Java](https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/09/25/46962115395/Concurrency+and+parallelism+in+Java) (Nam)

- [QUARKUS REACTIVE ARCHITECTURE](https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2021/10/01/46967849037/QUARKUS+REACTIVE+ARCHITECTURE) (Vu)

- <a href="https://www.reactivemanifesto.org/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.reactivemanifesto.org/</a>

- <a href="https://dzone.com/articles/asynchronous-communication-with-queues-and-microse" class="external-link" data-card-appearance="inline" rel="nofollow">https://dzone.com/articles/asynchronous-communication-with-queues-and-microse</a>

- Nielsen (User experience)

  - <a href="https://www.nngroup.com/articles/response-times-3-important-limits/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.nngroup.com/articles/response-times-3-important-limits/</a>

  - <a href="https://www.nngroup.com/articles/powers-of-10-time-scales-in-ux/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.nngroup.com/articles/powers-of-10-time-scales-in-ux/</a>

- <a href="https://www.nginx.com/blog/microservices-at-netflix-architectural-best-practices/" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.nginx.com/blog/microservices-at-netflix-architectural-best-practices/</a>

- <a href="https://pradnyapatil29.medium.com/cloud-application-architectural-styles-part-1-bbda76f8ad3f" class="external-link" data-card-appearance="inline" rel="nofollow">https://pradnyapatil29.medium.com/cloud-application-architectural-styles-part-1-bbda76f8ad3f</a>

- <a href="https://pradnyapatil29.medium.com/cloud-application-architectural-styles-part-2-b67469d524fa" class="external-link" data-card-appearance="inline" rel="nofollow">https://pradnyapatil29.medium.com/cloud-application-architectural-styles-part-2-b67469d524fa</a>

- <a href="https://www.linkedin.com/pulse/cloud-application-architectural-styles-part-3-web-queue-pradnya-patil" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.linkedin.com/pulse/cloud-application-architectural-styles-part-3-web-queue-pradnya-patil</a>

- Microsoft: <a href="https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/" class="external-link" data-card-appearance="inline" rel="nofollow">https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/</a>

%% ai-graph-start %%

**Related notes:**
- [[Database scaling case study]]
- [[Command Query Responsibility Segregation (CQRS)]]
- [[Performance pain points]]
- [[How to implement a service]]
- [[KLARA Documents Concept - Solution Design]]

%% ai-graph-end %%