---
ai_hash: 882712ab9bb95e0a
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 3
entities: []
relevance: 0.75
source: https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47726985300/Load+test
space: FUT
status: reference
tags:
- confluence
- infra
- space/fut
title: Load test
topic: infra
type: source
updated: 2024-03-25
---

# Load test

> [!info] Imported from Confluence
> Space **FUT** · updated 2024-03-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/FUT/pages/47726985300/Load+test)
> Relevance 0.75 · topic `infra`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="85feb225-0fab-4737-a26f-8694152b2168" macro-name="toc">

</div>

## 1. How to setup OneAPI load test environments

#### 1.1. Setup performance environment

a\) Create a branch of **luz-kubernetes** with configurations/resources same as PROD (<a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling</a>).

b\) Deploy this branch to performance environment.

c\) Execute the script `luz_kubernetes\env-performance-tools\delete-workloads-for-arrow.sh` to delete other workloads that are not used by OneAPI.

d\) Checkout <a href="https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services</a>, sync it with master branch (solve conflicts if any). Build new image of `luz-public-api-adapter` using this branch and then deploy it to performance environment.

=\> Make sure all remaining workloads are up and running healthy.

------------------------------------------------------------------------

Notes: To skip calling to 3rd party modules (like SMS provider, Print centers,…etc), we have to do below steps:

- Implement mock APIs in luz-thirdparty-mock module. Ex: <a href="https://bitbucket.org/axonivy-prod/luz_thirdparty_mock/commits/3d51aa060a979172b4bd20baf5511f11d8b82851" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_thirdparty_mock/commits/3d51aa060a979172b4bd20baf5511f11d8b82851</a>

- Build your code in luz-thirdparty-mock module and deploy it to performance environment

- Change URI of the real 3rd-party in OneAPI modules to this luz-thirdparty-mock module. Ex: <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/commits/842140e8cd68f4eeb3d67b9af6352280a788176f" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/commits/842140e8cd68f4eeb3d67b9af6352280a788176f</a>

------------------------------------------------------------------------

Ref: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47342256384/How+to+setup+OneAPI+on+Performance+env#STEPS" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47342256384/How+to+setup+OneAPI+on+Performance+env#STEPS</a>

#### 1.2. Setup local machine

a\) Port-forward necessary services (`luz-public-api-adapter`, `api-forwarder`):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c6edde79-3214-4d3f-b01b-656d8b13b352" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
gcloud container clusters get-credentials klara-performance --zone europe-west6-a --project klara-performance 
kubectl port-forward service/luz-public-api-adapter 8135:8080 -n performance
kubectl port-forward service/api-forwarder -n performance 8080:8080
```

</div>

</div>

b\) Start `luz-public-api-adapter-load-test` container on local machine. This container will provide APIs to support prepare API to do load test and send request to `luz-public-api-adapter` on GCP.  
Using latest code of <a href="https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_public_api_adapter/branch/load-test-services</a>, sync it with master branch (solve conflicts if any), build it and start the container using docker-compose:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="19ba3316-4812-4ed9-8787-cf4b690aad6a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker-compose -f docker-compose-load-test.yml up --build luz-public-api-adapter-load-test
```

</div>

</div>

#### 1.3. Start the test using Locust

a\) Set up Locust tool:

- Pull code of master branch locust at <a href="https://bitbucket.org/axonivy-prod/locust-load-test/src/master/" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/locust-load-test/src/master/</a>

- Edit `Dockerfile` of Locust to copy the necessary script (or create a new script). Ex: `COPY scripts/luz-eletter/SendDeliveryNdocsToNrecipients.py /home/locust/locustfile.py`

- Build and run this `Dockerfile`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d5d39e5f-5ba6-4ecd-b529-65a65a3740bd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker build -t locust_load_test .
docker run -p 8089:8089 locust_load_test
```

</div>

</div>

b\) Access to Locust at this URL: <a href="http://localhost:8089/#" class="external-link" rel="nofollow">http://localhost:8089</a>

c\) Configure test case. Note that `http://host.docker.internal:8085` is the endpoint of local `luz-public-api-adapter-load-test` container.


![[47726985300-image-20230426-160417.png]]



c\) Hit `Start swarming` button to start the test.

#### 1.4. Start the test using Postman

Use this Postman collections:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="b30dea13-bb1a-4b68-a3cd-75402e1bbd4e" macro-name="view-file"><a href="../_attachments/47726985300-Load_test_end_to_end_latest.postman_collection.json" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47726985300/Load_test_end_to_end_latest.postman_collection.json?version=1&amp;modificationDate=1711083175769&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/json" data-has-thumbnail="true">

![[47726985300-Load_test_end_to_end_latest.postman_collection.json]]

</a></span>

→ Original source from this page: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47398814200/One+API+end+to+end+testing.#Postman-Collection:~:text=use%20this%20postman%20collection%3A" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47398814200/One+API+end+to+end+testing.#Postman-Collection:~:text=use%20this%20postman%20collection%3A</a>

#### 1.5. Clean up after test

- Clean up all remaining messages in OneAPI’s topics/subscriptions if any:

See this page [Clean up data of performance after test](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47385674233/Clean+up+data+of+performance+after+test) for how to clean up

#### =\> Potential problems if setup steps

a\) Test data of all load test case are now pre-prepared only once time and kept in performance database. Those data are reused in many test cases. They are not cleaned up.

b\) Use the branch <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling" class="external-link" data-card-appearance="inline" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling</a> to keep configuration same as PROD. It is easy to be outdated. We need to sync it up with master branch manually.

## 2. Test cases

#### 2.1. Load test of individual modules

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Modules under load test</strong></p></th>
<th><p><strong>Test case</strong></p></th>
<th><p><strong>Number of concurrent user request</strong></p></th>
<th><p><strong>Test results</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td rowspan="3"><p>luz-docs-view-controller-batch: 1 pod</p>
<p>luz-docs-batch: 1 pod</p></td>
<td><p>Store document (100kb) in sender folder</p></td>
<td><p>8/20/50</p></td>
<td rowspan="3"><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47360312101/Load+test+luz-docs-view-controller+and+related+modules">Load test luz-docs-view-controller and related modules</a></p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47360312101/Load+test+luz-docs-view-controller+and+related+modules#Extra-testcases" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47360312101/Load+test+luz-docs-view-controller+and+related+modules#Extra-testcases</a></p></td>
</tr>
<tr>
<td>2</td>
<td><p>Get only document metadata in sender folder</p></td>
<td><p>6/20/50</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Download document (100kb) from sender folder and send it to recipient</p></td>
<td><p>3/20/50</p></td>
</tr>
<tr>
<td>4</td>
<td><p>luz-message-broker: 1 pod</p></td>
<td><p>Publish messages into a topic</p></td>
<td><p>1/5/25/50/100/1000/5000</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47346876696/Load+test+luz-message-broker">Load test luz-message-broker</a></p></td>
</tr>
<tr>
<td>5</td>
<td><p>luz-cache: 1 pod</p></td>
<td><p>Get a value from cache</p>
<p>Put a value into cache</p></td>
<td><p>1/5/10/25/100/1000</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47348547754/Load+test+luz-cache">Load test luz-cache</a></p></td>
</tr>
<tr>
<td>6</td>
<td><p>luz-doc-output-mgmt: 1 pod</p></td>
<td><p>Register Print &amp; Send with XML</p></td>
<td><p>20/35</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47351694568/Load+test+luz-doc-output-mgmt">Load test luz-doc-output-mgmt</a></p></td>
</tr>
<tr>
<td>7</td>
<td><p>luz-sms: 1 pod</p></td>
<td><p>Send SMS to recipients</p></td>
<td><p>1/20/30/60/100</p></td>
<td><p>Simulate 3rd party SMS system using <strong>luz_thirdparty_mock</strong>, it delays <strong>500ms</strong> for all requests and then return dummy responses.</p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47352119446/Luz-sms+the+same+configuration+as+Prod">Luz-sms (the same configuration as Prod)</a></p></td>
</tr>
<tr>
<td>8</td>
<td rowspan="2"><p>luz-email: 1 pod</p></td>
<td><p>Send E-mail to recipients by simulator</p></td>
<td><p>25</p></td>
<td><p>Simulate 3rd party E-mail system using <strong>luz_thirdparty_mock</strong>, it delays <strong>500ms</strong> for all requests and then return dummy response</p>
<p>Result: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47351857737/Load+test+luz-email#Test-result" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47351857737/Load+test+luz-email#Test-result</a></p></td>
</tr>
<tr>
<td>9</td>
<td><p>Send E-mail to recipients by real E-mail server</p></td>
<td><p>20/30/40/50</p></td>
<td><p>Result: <a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47351857737/Load+test+luz-email#More-testcases" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47351857737/Load+test+luz-email#More-testcases</a></p></td>
</tr>
<tr>
<td>10</td>
<td rowspan="4"><p>luz-ebill-networkpartner: 1 pod</p></td>
<td><p>Convert a KLARA receipt PDF (118KB) using Aspose</p></td>
<td><p>1/5/25/50/200</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47358935641/Load+test+render+pdf-a3b+by+aspose">Load test render pdf-a3b by aspose</a></p></td>
</tr>
<tr>
<td>11</td>
<td><p>Convert a KLARA receipt PDF (~118KB - 945KB) using PDFBox</p></td>
<td><p>1/2/5/25/200</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47358968053/Load+test+render+pdf-a3b+by+PDF-BOX">Load test render pdf-a3b by PDF-BOX</a></p></td>
</tr>
<tr>
<td>12</td>
<td><p>Convert a KLARA invoice PDF (~ 200Kb ) using Aspose failed then using PDFBox successful</p></td>
<td><p>3/10</p></td>
<td rowspan="2"><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47364833384/Load+test+-+luz-ebill-networkpartner">Load test - luz-ebill-networkpartner</a></p></td>
</tr>
<tr>
<td>13</td>
<td><p>Convert a small sample PDF using Aspose successful</p></td>
<td><p>3</p></td>
</tr>
<tr>
<td>14</td>
<td><p>luz-sps-outline: 1 pod</p></td>
<td><p>Send Print &amp; Send requests to Avaloq</p></td>
<td><p>3/5/10</p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47385608546/SPS+Outline+Loadtest">SPS Outline Loadtest</a></p></td>
</tr>
</tbody>
</table>

</div>

=\> Those test cases were created to find the best performance of 1 single module or 1 single API. There is no test for following modules:

- luz-tenant-dir + luz-address-normalizer

- luz-baumer

- luz-sms

#### 2.2. Load test of specific steps

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
<th></th>
<th><p><strong>Step</strong></p></th>
<th><p><strong>Test case</strong></p></th>
<th><p><strong>Test results</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Store in sender folder</p>

![[47726985300-image-20240320-104017.png]]

</td>
<td><ul>
<li><p>Submit a delivery with 10k documents</p></li>
<li><p><strong>30</strong> threads concurrently send request to luz-docs to store into sender folder</p></li>
<li><p>Modules are configured same as PROD</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47552561450/One+API+store+in+sender+step+test+result.">One API store in sender step test result.</a></p></td>
</tr>
<tr>
<td>2</td>
<td><p>Process document and prepare recipient sendings</p>

![[47726985300-image-20240320-104038.png]]

</td>
<td><ul>
<li><p>Modules are configured same as PROD</p></li>
<li><p>Find values for following configuration for document-sending queue consumers:</p>
<ul>
<li><p>Number of threads to pull messages: <strong>6</strong></p></li>
<li><p>Number of threads to handle messages: <strong>50</strong></p></li>
<li><p><code>MaxOutstandingElementCount</code>: <strong>180</strong></p></li>
<li><p>Ack deadline: <strong>300</strong></p></li>
</ul></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47359787092/Load+test+document+sending+dispatcher">Load test document sending dispatcher</a></p></td>
</tr>
<tr>
<td>3</td>
<td><p>Send to recipients</p>

![[47726985300-image-20240320-110713.png]]

</td>
<td><ul>
<li><p>Modules are configured same as PROD</p></li>
<li><p>Find values for following configuration for recipient-sending queue consumers:</p>
<ul>
<li><p>Number of threads to pull messages: <strong>6</strong></p></li>
<li><p>Number of threads to handle messages: <strong>50</strong></p></li>
<li><p><code>MaxOutstandingElementCount</code>: <strong>180</strong></p></li>
<li><p>Ack deadline: <strong>300</strong></p></li>
</ul></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47365816422/Load+test+recipient+sending+dispatcher">Load test recipient sending dispatcher</a></p></td>
</tr>
</tbody>
</table>

</div>

=\> Test case \#1 is OK

=\> Test cases \#2 and \#3 were created to find correct configurations of message consumers in luz-eletter-dispatcher, and luz-eletter-large-dispatcher. Those test cases should be kept to execute in the future with ideal configurations that have been found.

#### 2.2. Load test end to end

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p><strong>Channels</strong></p></th>
<th><p><strong>Test case</strong></p></th>
<th><p><strong>Setup configuration</strong></p></th>
<th><p><strong>Result</strong></p></th>
</tr>
&#10;<tr>
<td>1</td>
<td rowspan="7"><p>DIGITAL</p></td>
<td><p>Send 1 KLARA invoice to 10000 recipients in 1 delivery (10000 sendings in total)</p></td>
<td><ul>
<li><p><strong>1</strong> 'luz-eletter' pods</p></li>
<li><p><strong>4</strong> 'luz-eletter-dispatcher' pods (auto-scaled to 5 if any)</p></li>
<li><p><strong>5</strong> 'luz-docs-batch' pods</p></li>
<li><p><strong>5</strong> 'luz-docs-view-controller' pods</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%201%20doc%20to%2010000%20recipients" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%201%20doc%20to%2010000%20recipients</a></p></td>
</tr>
<tr>
<td>2</td>
<td><p>Send 10000 documents to 10000 recipients in 1 delivery (10000 sendings in total)</p></td>
<td><ul>
<li><p><strong>3</strong> 'luz-eletter' pods</p></li>
<li><p><strong>5</strong> 'luz-eletter-large-dispatcher' pods</p></li>
<li><p><strong>5</strong> 'luz-docs-batch' pods</p></li>
<li><p><strong>5</strong> 'luz-docs-view-controller' pods</p></li>
<li><p><strong>10</strong> 'luz-jsonstore' pods</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%2010000%20doc%20to%2010000%20recipients" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%2010000%20doc%20to%2010000%20recipients</a></p></td>
</tr>
<tr>
<td>3</td>
<td><p>Send 3 deliveries in parallel, each delivery sends 8000 documents to 8000 recipients. (3 * 8000 sendings in total)</p></td>
<td><ul>
<li><p><strong>2</strong> 'luz-eletter' pods</p></li>
<li><p><strong>5</strong> 'luz-eletter-large-dispatcher' pods</p></li>
<li><p><strong>5</strong> 'luz-docs-batch' pods (auto-scaled to 7 if any)</p></li>
<li><p><strong>1</strong> 'luz-public-api-adapter’ pod (branch load test) <strong></strong> with 9Gi memory</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Concurrent%20sending%20with%20large%20deliveries%3A%20DIGITAL%20%2D%203%20deliveries%20%2B%208000%20docs%20to%208000%20recipients" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Concurrent%20sending%20with%20large%20deliveries%3A%20DIGITAL%20%2D%203%20deliveries%20%2B%208000%20docs%20to%208000%20recipients</a></p></td>
</tr>
<tr>
<td>4</td>
<td><p>Send 4 marketing documents to 5000 recipients in 1 delivery (4 * 5000 sendings in total)</p></td>
<td rowspan="4"><ul>
<li><p>apply auto scale for luz-cache, luz-message-broker (max 3 replicas)</p></li>
<li><p>luz-docs-view-controller-batch adjust scale target based on memory</p></li>
<li><p>config as branch <a href="https://bitbucket.org/axonivy-prod/luz_kubernetes/branch/load-test-with-scaling" class="external-link" rel="nofollow"><strong>load-test-with-scaling</strong></a></p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=CPU%2022%25%20memory)-,DIGITAL,1%20delivery%20of%204%20docs%20to%205000%20recipients,-(1%20delivery%20to" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=CPU%2022%25%20memory)-,DIGITAL,1%20delivery%20of%204%20docs%20to%205000%20recipients,-(1%20delivery%20to</a></p></td>
</tr>
<tr>
<td>5</td>
<td><p>Send 10 deliveries in parallel from 10 <strong>different senders</strong>, each delivery sends 500 documents to 500 recipients (10 * 500 sendings in total)</p>
<ul>
<li><p>Document file size 1.2MB</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=10%20*%20(500%20docs%20to%20500%20recipients)%20from%2010%20different%20senders" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=10%20*%20(500%20docs%20to%20500%20recipients)%20from%2010%20different%20senders</a></p></td>
</tr>
<tr>
<td>6</td>
<td><p>Send 10 deliveries in parallel from 1 <strong>same sender</strong>, each delivery sends 500 documents to 500 recipients (10 * 500 sendings in total)</p>
<ul>
<li><p>Document file size 1.2MB</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=10%20*%20(500%20docs%20to%20500%20recipients)%20from%20same%20sender%20at%20the%20same%20time" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=10%20*%20(500%20docs%20to%20500%20recipients)%20from%20same%20sender%20at%20the%20same%20time</a></p></td>
</tr>
<tr>
<td>7</td>
<td><p>Send 28 deliveries submitted in total within 4 hours:</p>
<ul>
<li><p>16 delivery - each delivery send to 100 recipients (submit 1 delivery per 15 mins)</p></li>
<li><p>8 deliveries - each delivery send to 500 recipients (submit 1 delivery per 30 mins)</p></li>
<li><p>4 delivery- each delivery send to 5000 recipients (submit 1 delivery per hour)</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=100%20(March%202024)-,DIGITAL,(mix%20delivery),-One%20API%20end" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=100%20(March%202024)-,DIGITAL,(mix%20delivery),-One%20API%20end</a></p></td>
</tr>
<tr>
<td>8</td>
<td colspan="4"><p>=&gt; DIGITAL: Test cases are OK.</p></td>
</tr>
<tr>
<td>9</td>
<td rowspan="2"><p>EMAIL</p></td>
<td><p>Send email to 5000 recipients in 1 delivery</p></td>
<td><ul>
<li><p><strong>2</strong> 'luz-eletter-dispatcher' pods</p></li>
<li><p><strong>1</strong> 'luz-email' pod</p></li>
<li><p>Max number of concurrent request to e-mail server : <strong>15</strong></p></li>
<li><p>Using dev mail server</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=20231005%2D032950.png-,EMAIL,-Sending%20to%205000" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=20231005%2D032950.png-,EMAIL,-Sending%20to%205000</a></p></td>
</tr>
<tr>
<td>10</td>
<td><p>Send email to 5000 recipients in 1 delivery</p></td>
<td><ul>
<li><p><strong>2</strong> 'luz-eletter-dispatcher' pods</p></li>
<li><p><strong>1</strong> 'luz-email' pod</p></li>
<li><p>Max number of concurrent request to e-mail server : <strong>30</strong></p></li>
<li><p>Using dev mail server</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=503%20%2D%20service%20unavailable-,Sending%20to%205000%20recipients,-2%20%27luz%2Deletter" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=503%20%2D%20service%20unavailable-,Sending%20to%205000%20recipients,-2%20%27luz%2Deletter</a></p></td>
</tr>
<tr>
<td>11</td>
<td colspan="4"><p>=&gt; EMAIL: We should increase number of recipients in those test cases.</p></td>
</tr>
<tr>
<td>12</td>
<td rowspan="2"><p>EBILL</p></td>
<td><p>Send 100 documents to 100 recipients in 1 delivery (100 sendings in total) - <strong>single conversion using Aspose</strong></p></td>
<td rowspan="2"><ul>
<li><p>Real call to <strong>eBill (Six)</strong></p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%20100%20document%20to%20100%20recipients%20%2D%20single%20conversion%20using%20aspose" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending 100 document to 100 recipients - single conversion using aspose</a></p></td>
</tr>
<tr>
<td>13</td>
<td><p>Send 100 documents to 100 recipients in 1 delivery (100 sendings in total) - <strong>double conversion both using Aspose and PDFBox</strong></p></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%20100%20document%20to%20100%20recipients%20%2D%20double%20conversion%20both%20using%20aspose%20and%20pdfbox" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=Sending%20100%20document%20to%20100%20recipients%20%2D%20double%20conversion%20both%20using%20aspose%20and%20pdfbox</a></p></td>
</tr>
<tr>
<td>14</td>
<td colspan="4"><p>=&gt; EBILL: Test cases are OK, <strong>Six</strong> server recommend to test maximum 100 sendings at a time.</p></td>
</tr>
<tr>
<td>15</td>
<td rowspan="3"><p>PHYSICAL</p></td>
<td><p>Send 10000 documents to 10000 recipients in 1 delivery (10000 sendings in total)</p></td>
<td><ul>
<li><p>Real call to <strong>Baumer</strong></p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=PHYSICAL-,Sending%2010000%20document%20to%2010000%20recipients,-(DELIVERY%20528)" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=PHYSICAL-,Sending%2010000%20document%20to%2010000%20recipients,-(DELIVERY%20528)</a></p></td>
</tr>
<tr>
<td>16</td>
<td><p>Send 10000 documents to 10000 recipients in 1 delivery (10000 sendings in total)</p></td>
<td><ul>
<li><p>Real call to <strong>Avaloq</strong></p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=PHYSICAL-,Sending%2010000%20document%20to%2010000%20recipients,-20.10.2023" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=PHYSICAL-,Sending%2010000%20document%20to%2010000%20recipients,-20.10.2023</a></p></td>
</tr>
<tr>
<td>17</td>
<td><p>Send 10 deliveries in parallel from 10 <strong>different senders</strong>, each delivery sends 500 documents to 500 recipients (10 * 500 sendings in total)</p>
<ul>
<li><p>Document file size 1.2MB</p></li>
</ul></td>
<td><ul>
<li><p>Real call to <strong>Baumer</strong> &amp; <strong>Avaloq</strong></p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=PHYSICAL,from%20different%20senders" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=PHYSICAL,from%20different%20senders</a></p></td>
</tr>
<tr>
<td>18</td>
<td colspan="4"><p>=&gt; PHYSICAL test cases #15, #16 are OK. We should double number of total sendings in test case #17</p></td>
</tr>
<tr>
<td>19</td>
<td rowspan="4"><p>SMS</p></td>
<td><p>Send 10000 SMS messages with backup provider and many incorrect phone numbers</p></td>
<td><ul>
<li><p>Using SMS API (E-call provider as backup provider)</p></li>
<li><p>E-call provider rejects all the phone number</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%201st" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%201st</a></p></td>
</tr>
<tr>
<td>20</td>
<td><p>Send 10000 SMS messages with no backup provider</p></td>
<td><ul>
<li><p>Using SMS API only</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%202nd" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%202nd</a></p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%203rd" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%203rd</a></p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%204th" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Sending%20SMS%20simple%20short%20messages%204th</a></p>
<p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=10%27000-,Sending%20SMS%20channel%20via%20oneAPI,-Delivery%20Id%3A" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=10%27000-,Sending%20SMS%20channel%20via%20oneAPI,-Delivery%20Id%3A</a></p></td>
</tr>
<tr>
<td>21</td>
<td><p>Send 10000 SMS messages <strong></strong> via oneAPI using real SMS provider</p></td>
<td><ul>
<li><p>Use SMS API provider</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=10%27000-,Sending%20SMS%20channel%20via%20oneAPI,-Delivery%20Id%3A" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=10%27000-,Sending%20SMS%20channel%20via%20oneAPI,-Delivery%20Id%3A</a></p></td>
</tr>
<tr>
<td>22</td>
<td><p>Send 10000 SMS messages <strong></strong> via oneAPI when 3rd service is not stable</p></td>
<td><ul>
<li><p>Mocked the 3rd party service to test 3rd party is unavailable</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Mocked%20the%203rd%20party%20service" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47399600234/Testing+Protocol+-+10+000+docs+h#:~:text=Mocked%20the%203rd%20party%20service</a></p></td>
</tr>
<tr>
<td>23</td>
<td colspan="4"><p>=&gt; SMS: If possible, we need to modify test case #19. There should be NO incorrect phone number in delivery. Other test cases look OK.</p></td>
</tr>
<tr>
<td>24</td>
<td><ul>
<li><p>DIGITAL</p></li>
<li><p>SMS</p></li>
</ul></td>
<td><p>Send 100 documents to 100 recipients in 1 delivery (100 sendings in total) , fallback to SMS channel when DIGITAL channel failed.</p></td>
<td><ul>
<li><p><strong>5</strong> 'luz-eletter-dispatcher' pods</p></li>
<li><p><strong>5</strong> 'luz-eletter-large-dispatcher' pods</p></li>
<li><p>Use SMS API provider</p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=validate%20closed%20stream-,DIGITAL%20JUMP%20TO%20SMS,-Sending%20100%20docs" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=validate%20closed%20stream-,DIGITAL%20JUMP%20TO%20SMS,-Sending%20100%20docs</a></p></td>
</tr>
<tr>
<td>25</td>
<td colspan="4"><p>=&gt; Increase number of total sendings</p></td>
</tr>
<tr>
<td>26</td>
<td><ul>
<li><p>DIGITAL</p></li>
<li><p>PHYSICAL</p></li>
</ul></td>
<td><p>Send 100 documents to 100 recipients in 1 delivery (100 sendings in total) , fallback to PHYSICAL channel when DIGITAL channel failed.</p></td>
<td><ul>
<li><p><strong>1</strong> 'luz-eletter-dispatcher' pods</p>
<ul>
<li><p>Retry policy :</p>
<ul>
<li><p>max email retries: <strong>5</strong></p></li>
<li><p>email delay: <strong>120s</strong></p></li>
</ul></li>
</ul></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=DIGITAL%20JUMP%20TO%20PHYSICAL" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=DIGITAL%20JUMP%20TO%20PHYSICAL</a></p></td>
</tr>
<tr>
<td>27</td>
<td colspan="4"><p>=&gt; Increase number of total sendings</p></td>
</tr>
<tr>
<td>28</td>
<td><ul>
<li><p>EBILL</p></li>
<li><p>EMAIL</p></li>
</ul></td>
<td><p>Send 100 documents to 100 recipients in 1 delivery (100 sendings in total) , fallback to EMAIL channel when EBILL channel failed.</p></td>
<td><ul>
<li><p><strong>2</strong> 'luz-eletter-dispatcher' pods</p>
<ul>
<li><p>Retry policy :</p>
<ul>
<li><p>max email retries: <strong>5</strong></p></li>
<li><p>email delay: <strong>120s</strong></p></li>
</ul></li>
</ul></li>
<li><p><strong>1</strong> 'luz-email' pod</p>
<ul>
<li><p>Max number of concurrent request : <strong>20</strong></p></li>
</ul></li>
<li><p><strong>1</strong> 'luz-ebill-network-partner' pod</p></li>
<li><p>Using dev mail server</p></li>
<li><p>Real call to <strong>eBill (Six)</strong></p></li>
</ul></td>
<td><p><a href="https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=allRecipientsRequired%20%3D%20true%20flag-,EBILL%20JUMP%20TO%20EMAIL,-Sending%20100%20docs" data-card-appearance="inline" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47507767545/Performance+ONE+API+Load+Test#:~:text=allRecipientsRequired%20%3D%20true%20flag-,EBILL%20JUMP%20TO%20EMAIL,-Sending%20100%20docs</a></p></td>
</tr>
<tr>
<td>29</td>
<td colspan="4"><p>=&gt; Increase number of total sendings</p></td>
</tr>
</tbody>
</table>

</div>

=\> Need more e2e load test cases for:

- Identity matching scenarios

- Channels switching

%% ai-graph-start %%

**Related notes:**
- [[One API end to end testing]]
- [[One API load test with locust]]
- [[Port forward and Docker compose]]
- [[Infrastructure]]
- [[Port Forward to call GCP API in localhost]]

%% ai-graph-end %%