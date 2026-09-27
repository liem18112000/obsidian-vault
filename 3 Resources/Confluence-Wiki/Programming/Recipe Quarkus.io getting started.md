---
ai_hash: a3133c1f08977053
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 2.92
entities: []
relevance: 0.798
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46941995722/Recipe+Quarkus.io+getting+started
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: 'Recipe: Quarkus.io getting started'
topic: programming
type: source
updated: 2022-02-21
---

# Recipe: Quarkus.io getting started

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-02-21 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/46941995722/Recipe+Quarkus.io+getting+started)
> Relevance 0.798 · topic `programming`

<span class="status-macro aui-lozenge aui-lozenge-visual-refresh aui-lozenge-error conf-macro output-inline" hasbody="false" macro-id="33187a6e-984a-40af-bb1c-78e24ddc638b" macro-name="status">WORK IN PROGRESS</span>

  

This recipe shows how to use the Quarkus.io fullstack framework to build a web application.

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="4d33b73a-51fb-4bc3-a3e9-0277a45919aa" macro-name="toc">

</div>

# Problem

Get to know how quarkus and quarkus qute work. 

# Solution

## Quarkus Sample

This sample project has been created to learn quarkus with qute. 

GIT Repo with a sample implementation: <a href="https://bitbucket.org/axonivy-prod/quarkus_and_qute_sample/src/master/" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/quarkus_and_qute_sample/src/master/</a>

### Bootstrapping REST resources

#### Hello Resource

This REST Resource is going to return the Hello message based on the name as input parameter. Also it calls the Echo REST endpoint just to make a roundtrip to another REST resource.   
To initialize the Hello REST resource this bootstrap bash script can be excecuted: 

GIT: <a href="https://bitbucket.org/axonivy-prod/quarkus_and_qute_sample/src/master/bootstrap-hello.sh" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/quarkus_and_qute_sample/src/master/bootstrap-hello.sh</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="41c06246-5dbc-4c98-bbf4-ecffe8ab3f0b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#!/usr/bin/env bash

mvn io.quarkus.platform:quarkus-maven-plugin:2.2.1.Final:create \
    -DprojectGroupId=ch.klara \
    -DprojectArtifactId=hello \
    -DprojectVersion=1.0-SNAPSHOT \
    -DclassName="ch.klara.hello.HelloResource" \
    -Dpath="/api/hello" \
    -Dextensions="resteasy, resteasy-jsonb"
```

</div>

</div>

#### Echo Resource

This REST Resource simply returns the echo message back to the caller without any modifications.

To initialize the Hello REST resource this bootstrap bash script can be excecuted: 

GIT: <a href="https://bitbucket.org/axonivy-prod/quarkus_and_qute_sample/src/master/bootstrap-echo.sh" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/quarkus_and_qute_sample/src/master/bootstrap-echo.sh</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="79f20fbb-14ee-4ae9-9031-5e3124b6d236" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#!/usr/bin/env bash

mvn io.quarkus.platform:quarkus-maven-plugin:2.2.1.Final:create \
    -DprojectGroupId=ch.klara \
    -DprojectArtifactId=echo \
    -DprojectVersion=1.0-SNAPSHOT \
    -DclassName="ch.klara.echo.EchoResource" \
    -Dpath="/api/echo" \
    -Dextensions="resteasy, resteasy-jsonb"
```

</div>

</div>

### Bootstrapping Quarkus Qute application

This qute-sample application implements few Page simple UI samples to display the data from Qute template instances.

To initialize th<span class="legacy-color-text-default">e qute-sample application</span> this bootstrap bash script can be excecuted: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5d15a141-aa4b-4fa5-ad59-73561e83e98d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#!/usr/bin/env bash

mvn io.quarkus.platform:quarkus-maven-plugin:2.2.1.Final:create \
    -DprojectGroupId=ch.klara \
    -DprojectArtifactId=qute-sample \
    -DprojectVersion=1.0-SNAPSHOT \
    -Dextensions="quarkus-resteasy-qute"
```

</div>

</div>

## Docker and Kubernetes with qute-sample project

### Prerequisites

- Install Graalvm for native image (mac os with brew cask: <a href="https://github.com/graalvm/homebrew-tap" class="external-link" rel="nofollow">https://github.com/graalvm/homebrew-tap</a>), extend PATH variable and set the JAVA_HOME or/and GRAALVM_HOME path

Steps

1.  Run application on DEV mode: 

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="af845f4f-5da0-4b12-b67e-b6b4611a5b24" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    ./mvnw compile quarkus:dev
    ```

    </div>

    </div>

2.  Native build with graalvm (native executable)

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a5c935d0-279c-4016-a579-733d7cb68812" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # a. Build jar
    ./mvnw clean install

    # b. Build native image with graalvm
    ./mvnw clean package -Pnative -DskipTests=true
    ```

    </div>

    </div>

3.  Run application

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4a1b0fe-3b64-4b1e-8443-4b613e7b6b5c" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # a. run application (jar)
    java -jar target/quarkus-app/quarkus-run.jar 

    # b. run application (nativ image)
    ./target/qute-sample-1.0-SNAPSHOT-runner
    ```

    </div>

    </div>

    The start time of the application is very fast ~0.024s.   
      

4.  Containerize (run Docker Desktop for this step)

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="77ab732f-9bfc-41d5-b1dc-fc45de355f1f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    ./mvnw quarkus:add-extension -Dextensions="container-image-docker"

    # a. java jar
    ./mvnw clean package -Dquarkus.native.container-build=true -Dquarkus.native.container-runtime=docker -Dquarkus.container-image.build=true -DskipTests=true

    # b. native image
    ./mvnw clean package -Pnative -Dquarkus.native.container-build=true -Dquarkus.native.container-runtime=docker -Dquarkus.container-image.build=true -DskipTests=true
    ```

    </div>

    </div>

    **Note:** Check trouble shoot section if the is an OutOfMemory issue (Code 137).   
    After the image has been created the following command can be excuted to find the image on local Docker registry. Sample:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b06c552f-e21c-4755-80e1-30a289000376" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # Show Docker images on your local Docker registry 
    docker images 

    # Ouput: 
    # REPOSITORY                                 TAG             IMAGE ID       CREATED        SIZE
    # <username>/qute-sample                     1.0-SNAPSHOT    ceb6425af5c1   2 days ago     146MB
    ```

    </div>

    </div>

5.  Run application Docker Image locally

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d27d7209-4395-4568-9f64-2700ae1f613d" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
     docker run --name qute-sample-app -d -p 8080:8080 <username>/qute-sample:1.0-SNAPSHOT
    ```

    </div>

    </div>

    **-d**: Run container in background and print container ID  
    **--name**: application name  
    **--p**: Publish a container's port(s) to the host  
      

6.  Tag Quarkus Docker image and push it to an Image Repository

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="25692b6a-cc12-4d2e-bc63-2034f4c4d163" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # https://docs.docker.com/engine/reference/commandline/push/
    # docker push [OPTIONS] NAME[:TAG] 

    docker push gcr.io/qute-sample-app/qute-sample:v1.0
    ```

    </div>

    </div>

    Note: If the username is not the same as on your local Docker Registry change it with the following command: docker tag \<username\>/qute-sample:1.0-SNAPSHOT \<new_username\>/qute-sample:1.0-SNAPSHOT 

7.  Start application pod at GCP 

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="31fed54b-7ce3-4dd9-8d4a-e1adf7e474a4" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    # 1. adding quarkus extension, if not added yet (optional)
    ./mvnw quarkus:add-extension -Dextensions="quarkus-kubernetes" 

    # 2. Run packaging command step 4 to generate the kubernetes.yml or kubernetes.json file on target folder

    # 3. Deploy the application on Kubernetes
    kubectl apply -f target/kubernetes/kubernetes.yml
    ```

    </div>

    </div>

    **Note:** The generation of the kubernetes.yml or kubernetes.json file can be customizes with application.properties (see quarkus cheat sheet or <a href="https://quarkus.io/guides/deploying-to-kubernetes" class="external-link" rel="nofollow">https://quarkus.io/guides/deploying-to-kubernetes</a>)

## Reactive and Asynchronous APIs

Reference: <a href="https://quarkus.io/guides/getting-started-reactive" class="external-link" rel="nofollow">https://quarkus.io/guides/getting-started-reactive</a>

The reactive model uses Non-blocking I/O threads directly instead of worker threads. It saves memory and CPU as there is no need to create worker threads to handle the requests. Also I/O thread can handle multiple concurrent requests. 

Threads: 

- Quarkus Main Thread (imperative/reactive: main thread)
- executor-thread-xxx (imperative: worker thread)
- vert.x-worker-thread-xxx (reactive: worker thread)
- vert.x-eventloop-thread-xxx (reative: DB, IO-Thread)

Info:

- RESTEasy Reactive by default handles each HTTP request on an IO thread (otherwise known as an event-loop thread)
- Reactive is good at blocking IO operations (Resource access (DB, file, etc.), Service calls)
- RESTEasy Reactive can work with blocking or nonblocking endpoints
- Using @Blocking 30% less throughput than reactive
- Using @Blocking 50% higher throughput than RESTEasy classic
- Using RESTEasy classic with Uni or Mutiny (quarkus-resteasy-mutiny)

Recommendations:

- Use reactive with blocking (@blocking) or non blocking (no annotations) endpoints instead of RESTEasy classic
- Use Reactive Routes to gain better performance and througput

Features:

- Response time is smaller (not Thread context switches)
- Reduce memory consumption
- Concurrency is no longer limited by the number of threads
- (uses hardware resources efficiently)
- (maximum throughput)

Resources:

- <a href="https://quarkus.io/blog/resteasy-reactive-faq/" class="external-link" rel="nofollow">https://quarkus.io/blog/resteasy-reactive-faq/</a>
- <a href="https://quarkus.io/blog/io-thread-benchmark/" class="external-link" rel="nofollow">https://quarkus.io/blog/io-thread-benchmark/</a>

### Quarkus Extensions enabling reactive

Reactive Architecture: <a href="https://quarkus.io/guides/quarkus-reactive-architecture" class="external-link" rel="nofollow">https://quarkus.io/guides/quarkus-reactive-architecture</a>

HTTP

- **RESTEasy Reactive** (`io.quarkus:quarkus-resteasy-reactive`): an implementation of JAX-RS tailored for the Quarkus architecture. It follows a reactive-first approach but allows imperative code using the `@Blocking` annotation.

- **Reactive Routes** (`io.quarkus:quarkus-reactive-routes`): a declarative way to register HTTP routes directly on the Vert.x router used by Quarkus to route HTTP requests to methods.

- **Reactive Rest Client** (io.quarkus:`quarkus-rest-client-reactive`): allows consuming HTTP endpoints. Under the hood, it uses the non-blocking I/O features from Quarkus.

- **Qute** (`io.quarkus:`<span class="extension-id" title="io.quarkus:quarkus-resteasy-reactive-qute">`quarkus-resteasy-reactive-qute`</span>): the Qute template engine exposes a reactive API to render templates in a non-blocking manner.

For additional extension e.g. Data, Even-Driven, Network Protocol and Utilities, Engine please refer to <a href="https://quarkus.io/guides/quarkus-reactive-architecture" class="external-link" rel="nofollow">https://quarkus.io/guides/quarkus-reactive-architecture</a> (Section "Quarkus Extensions enabling Reactive").

### Thread configs

- quarkus.vertx.event-loops-pool-size=2 (number of non blocking I/O threads)
- quarkus.vertx.worker-pool-size=2 (tbd)
- quarkus.thread-pool.max-threads=2 (number of worker threads)

  

### Todo Sample

Bootstrap

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="592e5d56-9f45-4702-98ca-9b90d3509244" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
#!/usr/bin/env bash

mvn io.quarkus.platform:quarkus-maven-plugin:2.2.3.Final:create \
    -DprojectGroupId=com.sample \
    -DprojectArtifactId=todo \
    -DclassName="org.acme.vertx.TodoResource" \
    -Dpath="/todo" \
    -Dextensions="resteasy,reactive-pg-client,resteasy-mutiny"
```

</div>

</div>

Using PostgresSQL Docker Image: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3699e61b-bf7b-4cff-a9f4-60625fb235f1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run --ulimit memlock=-1:-1 -it --rm=true --memory-swappiness=0 --name quarkus_test -e POSTGRES_USER=quarkus_test -e POSTGRES_PASSWORD=quarkus_test -e POSTGRES_DB=quarkus_test -p 5432:5432 postgres:10.5
```

</div>

</div>

Steps

1.  Inject Postgres client the code: 

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bac88b01-b861-422d-b873-aea36c017463" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    @Path("/todo")
    public class TodoResource {
        ...
        @Inject
        io.vertx.mutiny.pgclient.PgPool client;
        ...
    }
    ```

    </div>

    </div>

2.  Using Uni (one item) and Multi (muliple items) asynchrous types in code (REST resources and DB queries):

    REST Resources:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="14269d3f-b6bc-450a-90fb-64f5d5876e5f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    @Path("/todo")
    @Produces(MediaType.APPLICATION_JSON)
    @Consumes(MediaType.APPLICATION_JSON)
    public class TodoResource {    
        ...

        @GET
        public Multi<Todo> get() {
            return Todo.findAll(client);
        }
        ...

        @GET
        @Path("{id}")
        public Uni<Response> getSingle(@PathParam("id") Long id) {
            return Todo.findById(client, id)
                    .onItem().transform(todo -> todo != null ? Response.ok(todo) : Response.status(Response.Status.NOT_FOUND))
                    .onItem().transform(Response.ResponseBuilder::build);
        }
        ...
    }
    ```

    </div>

    </div>

    SQL queries:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1ec02cf1-0d2c-4fe3-a3cb-88d85d6c3055" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    public class Todo {
        ...
        public static Multi<Todo> findAll(PgPool client) {
            return client.query("SELECT id, title, description FROM todo ORDER BY id ASC").execute()
                    .onItem().transformToMulti(set -> Multi.createFrom().iterable(set))
                    .onItem().transform(Todo::from);
        }
        ...

        public static Uni<Todo> findById(PgPool client, Long id) {
            return client.preparedQuery("SELECT id, title, description FROM todo WHERE id = $1").execute(Tuple.of(id))
                    .onItem().transform(RowSet::iterator)
                    .onItem().transform(iterator -> iterator.hasNext() ? from(iterator.next()) : null);
        }
        ...
    }
    ```

    </div>

    </div>

Further topics would be Streaming REST Endpoint with Server-Sent Event support: <a href="https://quarkus.io/guides/resteasy-reactive" class="external-link" rel="nofollow">https://quarkus.io/guides/resteasy-reactive</a> (section: Server-Sent Event (SSE) support). 

  

#### Streaming

Reference: <a href="https://quarkus.io/guides/getting-started-reactive" class="external-link" rel="nofollow">https://quarkus.io/guides/getting-started-reactive</a>

Reactive developers may wonder why we can’t return a stream of fruits directly. It **tends to be a bad idea when dealing with a database**. Relational **databases do not handle streaming well**. It’s a problem of protocols not designed for this use case. So, to stream rows from the database, you need to **keep a connection** (and sometimes a transaction) open until all the rows are consumed. If you have slow consumers, you break the **golden rule of databases: don’t hold connections for too long.** Indeed, the number of connections is rather low, and having consumers keeping them for too long will dramatically reduce the concurrency of your application. So, when possible, use a `Uni<List<T>>` and load the content. If you have a large set of results, **implement pagination**.

Using stream with REST client: <a href="https://quarkus.io/guides/resteasy-reactive#streaming-support" class="external-link" rel="nofollow">https://quarkus.io/guides/resteasy-reactive#streaming-support</a>

Reactive SQL client:<a href="https://quarkus.io/guides/reactive-sql-clients" class="external-link" rel="nofollow"> https://quarkus.io/guides/reactive-sql-clients</a> (no streaming)

  

## Quarkus Fault Tolerance

Reference: <a href="https://quarkus.io/guides/smallrye-fault-tolerance" class="external-link" rel="nofollow">https://quarkus.io/guides/smallrye-fault-tolerance</a> 

- Resiliency - Retries: Every time the call to the same endpoint fails the platform will automatically retry the call based on the maxRetries configuration on @Retry annotation. 
- Resiliency - Timeouts: If a call duration exceeds the configured time on @Timeout annotation (in milliseconds) a TimoutException will be thrown.
- Resiliency - Fallbacks: A fallback method will be called in case of exception instead of throwing the exception.
- Resiliency - Circuit Breaker: The circuit breaker limits the number of failures in the system, when part of the system becomes temorarily unstable. In this case the circuit breaker will open and block all further invocations of that method for a given time. Sample: requestVolumeThreshold=4, CircuitBreaker.failureRation is by default 0.5 and CircuitBreaker.delay is by default 5 seconds. That means that a circuit breaker will open wehn 2 of the last 4 invocations failes and it will stay open for 5 seconds.

### Sample code

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3361eee3-8503-4f40-a7d8-f858b65feb77" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom">

<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
    @POST
    @Produces(MediaType.APPLICATION_JSON)
    @Consumes(MediaType.TEXT_PLAIN)
    @Retry(maxRetries = 4) // adding resiliency: retries
    @Timeout(100) // adding resiliency: timeout
    @Fallback(fallbackMethod = "fallbackHello") // adding reciliency: fallback
    @Path("/hello")
    public Hello sayHelloWithSpecificName(String name) throws InterruptedException {
        String echoMessage = echoAdapter.echo(name);

        Hello helloMessage = new Hello();
        String message = "Hello " + echoMessage;
        helloMessage.setMessage(message);
        logger.info("hello message: " + helloMessage);

        return helloMessage;
    }

    // fallback method
    public Hello fallbackHello(String name){
        Hello fallbackHelloMessage = new Hello();
        fallbackHelloMessage.setMessage("This is the fallback message.");
        return fallbackHelloMessage;
    }

    @POST
    @Produces(MediaType.APPLICATION_JSON)
    @Consumes(MediaType.TEXT_PLAIN)
    @CircuitBreaker(requestVolumeThreshold = 4) adding reciliency: circuit breaker
    @Path("/hello/circuit")
    public Hello sayHelloSimulateCircuitBreaker(String name){
        logger.info("not circuit yet.");
        maybeFail(name.length());
        Hello helloSuccess = new Hello();
        helloSuccess.setMessage(name);
        return helloSuccess;
    }

    private void maybeFail(int length) {
        if (length % 4 == 0) { // alternate 2 successful and 2 failing invocations
            throw new RuntimeException("Service failed.");
        }
    }
```

</div>

</div>

  

## Qute Templating Engine

Reference: <a href="https://quarkus.io/guides/qute" class="external-link" rel="nofollow">https://quarkus.io/guides/qute</a>

### Workflow

<div class="paragraph">

  

</div>

<div class="olist arabic">

1.  Create template contents (`hello.html`): 

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bdb424f3-f275-4fb9-b2c7-f5ea590b2bf9" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    <html>
      <p>Hello {name}! 
    </html>
    ```

    </div>

    </div>

2.  Parse template definition (`io.quarkus.qute.Template`): 

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9a2ca70e-9998-48f4-bb77-aad6b88c50be" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    Template helloTemplate = engine.parse(helloHtmlContent);
    ```

    </div>

    </div>

3.  Create template instance (io.quarkus.qute.TemplateInstance), set the data and render output

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="770b38c8-318f-44e2-b55f-828b5d02a717" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // Renders <html><p>Hello Jim!</p></html>
    helloTemplate.data("name", "Jim").render(); 
    ```

    </div>

    </div>

</div>

### Additional features

#### Type-safe templates

Implementation that check if the template is available by some conventions.

#### Template Parameter Declarations

Parameters defined in Templates. Qute is going to validate all expressions that references those parameter during compilation.

#### Template parameter declaration inside the template itself (optional)

Qute is going to validate all expressions that references this parameter. 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ba221d6e-77c0-47fe-a7cb-571c577c1972" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom">

<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
{@org.acme.Item item} <!-- here -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>{item.name}</title> 
</head>
<body>
    <h1>{item.name}</h1>
    <div>Price: {item.price}</div>
</body>
</html>
```

</div>

</div>

#### Template Extension

Sometimes, you’re not in control of the classes that you want to use in your template, and you cannot add methods to them. Template extension methods allows you to declare new method for those classes that will be available from your templates just as if they belonged to the target class. E.g. decoration UI (Datetime format, decimal format, translations, etc.)

#### Template Inheritance

Template inheritance makes it possible to reuse template layouts.

Reference: <a href="https://quarkus.io/guides/qute-reference" class="external-link" rel="nofollow">https://quarkus.io/guides/qute-reference</a>

**base.html**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="66a7f78e-d978-4d84-b288-8c91e2280f64" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<html>
<head>
    <meta charset="UTF-8">
    <title>{#insert title}Default Title{/}</title> <!-- If no title will be inserted the default title will be displayed -->
    {#include shared/style.html}{/include}
</head>

<body>
{#include shared/sidebar.html}{/include}

<div id="main">
    <button class="openbtn" onclick="openNav()">☰ Open Sidebar</button>

    {#insert}No body!{/} <!-- Insert body -->

</div>
</body>
</html>
```

</div>

</div>

**login.html** (which reuse base.html template):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f438f9e5-51f5-4b21-a8ca-589197660fb7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{#include shared/base.html} <!-- reuse base.html -->
{#title}Login{/title} <!-- replace titel in base.html -->


<div class="col-md-6 col-sm-12">
    <div class="login-form">
        <form id="loginForm" action="/admin/login" method="post">
            <div class="form-group">
                <label>User Name</label>
                <input name="user" type="text" class="form-control" placeholder="User Name">
            </div>
            <div class="form-group">
                <label>Password</label>
                <input name="password" type="password" class="form-control" placeholder="Password">
            </div>
            <div class="form-group">
                <button type="submit" class="btn btn-primary">Login</button>
            </div>
        </form>
    </div>
</div>

{/include}
```

</div>

</div>

  

<span style="font-size: 1.142em;">Adding Bootstrap </span>

<div class="sectionbody">

<div class="sect2">

To use Bootstrap add the following WebJars depdendency to pom.xml and include the stylesheet to the HTML page: 

pom.xml: 

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c2882be2-23af-4051-8311-bc9fcd380b3e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<!-- https://mvnrepository.com/artifact/org.webjars/bootstrap -->    
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>bootstrap</artifactId>
    <version>5.1.0</version>
</dependency>
```

</div>

</div>

\*.html: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e9219a31-1b0d-492d-90ab-d69ab04e7801" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<!DOCTYPE html>
<html>
<head>
...
    <link rel="stylesheet" href="/webjars/bootstrap/5.1.0/css/bootstrap.min.css">
...
</head>
<body>
</body>
```

</div>

</div>

#### Including HTML templates

It's possible to include another HTML template and override some parts of the template. 

#### Sample code structure


![[46941995722-quarkus_and_qute_sample_–_item_html__qute-sample_.png]]



#### qutepage.html

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="14e3a080-ed6d-4f52-9d2f-890a485a9ad6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<!DOCTYPE html>
<html>
{#include header.html}{/include}
<body>
    {#include sidebar.html}{/include}

    <div id="main">
        <button class="openbtn" onclick="openNav()">☰ Open Sidebar</button>
        <p>
            <h1>Hi <b>{name ?: "Qute"}</b></h1>
        </p>
        <p>Create your web page using Quarkus RESTEasy & Qute</p>

        {#include usertable.html}{/include}

    </div>
</body>
</html>
```

</div>

</div>

#### Static HTTP resources

The static HTTP resources has to be places in `META-INF/resources` and sub folders. Only Qute Templates (dynamic resources) can be stored in `META-INF/resources/templates`.

</div>

  

<div class="sectionbody">

Reference: <a href="https://quarkus.io/guides/http-reference" class="external-link" rel="nofollow">https://quarkus.io/guides/http-reference</a>

</div>

  

<div class="sectionbody">

#### Web Components with Lit

</div>

tbd: adding link

  

  

  

  

<div class="sectionbody">

### KeyCloak

This is a sample how to enable authentication to your web application using OpenID Connect. 

</div>

Reference: <a href="https://github.com/quarkusio/quarkus-quickstarts/tree/main/security-openid-connect-web-authentication-quickstart" class="external-link" rel="nofollow">https://github.com/quarkusio/quarkus-quickstarts/tree/main/security-openid-connect-web-authentication-quickstart</a>

Adding quarkus extension for OIDC: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7301467d-943e-479d-bf6e-169a7681a401" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
./mvnw quarkus:add-extension -Dextensions="quarkus-oidc"

./mvnw quarkus:add-extension -Dextensions="oidc,keycloak-authorization"

# for token propagation
./mvnw quarkus:add-extension -Dextensions="quarkus-oidc-token-propagation"
```

</div>

</div>

  

Adding quarkus keycloak sample config to the application.properties: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="41ab56be-298e-4165-9ab5-66bbe3131b6a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# quarkus authorizatin code flow config:
quarkus.oidc.auth-server-url=http://localhost:8180/auth/realms/quarkus
quarkus.oidc.client-id=frontend
quarkus.oidc.credentials.secret=fcfaa004-5fb3-4e65-a4dc-540bef1f9a8d
quarkus.oidc.application-type=web-app
quarkus.http.auth.permission.authenticated.paths=/*
quarkus.http.auth.permission.authenticated.policy=authenticated
```

</div>

</div>

  

Run Keycloak Docker Image locally: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="31bebaf6-82a2-4f5a-9db8-e92ac6a445af" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
docker run --name keycloak -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin -p 8180:8080 {keycloak-docker-image}
```

</div>

</div>

  

  

Configure token exchange on KeyCloak Server: 

<a href="https://www.keycloak.org/docs/latest/securing_apps/#_token-exchange" class="external-link" rel="nofollow">https://www.keycloak.org/docs/latest/securing_apps/#_token-exchange</a>

<a href="https://www.keycloak.org/docs/latest/server_installation/#profiles" class="external-link" rel="nofollow">https://www.keycloak.org/docs/latest/server_installation/#profiles</a>

  

# Testing

Reference: <a href="https://quarkus.io/guides/getting-started-testing" class="external-link" rel="nofollow">https://quarkus.io/guides/getting-started-testing</a>

- RESTassured: Is used to call/test REST endpoints

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cb29149e-8ec9-410e-8a3f-b3feabc542a8" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  @Test
  public void shouldReturnEchoMessage() {
      String message = "hi mock";
      given().contentType(ContentType.TEXT)
              .queryParam("message", message)
          .when().post("/testing/echo")
          .then()
              .statusCode(200)
              .body(is(message));
      }
  ```

  </div>

  </div>

## Mock

Please refer to the reference quide link above for

- CDI @alternative of @priority mechanism: This Method can be used to Mock local services or REST calls (remote)

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="38036c0e-9a97-403d-a63a-565cb7f29fb4" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  @Mock
  @ApplicationScoped 
  public class MockExternalService extends ExternalService {

      @Override
      public String service() {
          return "mock";
      }
  }
  ```

  </div>

  </div>

- QuarkusMock

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b696d9eb-ac15-4111-90cb-425704d5a0eb" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  @QuarkusTest
  public class MockTestCase {

      @Inject
      MockableBean1 mockableBean1;

      @BeforeAll
      public static void setup() {
          MockableBean1 mock = Mockito.mock(MockableBean1.class);
          Mockito.when(mock.greet("Stuart")).thenReturn("A mock for Stuart");
          QuarkusMock.installMockForType(mock, MockableBean1.class);  
      }

      @Test
      public void testBeforeAll() {
          Assertions.assertEquals("A mock for Stuart", mockableBean1.greet("Stuart"));
      }
  }
  ```

  </div>

  </div>

- @InjectMock and @InjectSpy annotations (further information at the documentation)  
    

## WireMock

Using HTTP Server for Mock/Stubs. 

Reference: <a href="https://quarkus.io/guides/rest-client" class="external-link" rel="nofollow">https://quarkus.io/guides/rest-client</a> (section wiremock).

### Implements QuarkusTestResourceLifecycleManager

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7e9e679c-d890-464c-9705-03dbad32e13f" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom">

<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
package com.sample.wiremock;

import com.github.tomakehurst.wiremock.WireMockServer;
import io.quarkus.test.common.QuarkusTestResourceLifecycleManager;
import java.util.HashMap;
import java.util.Map;
import static com.github.tomakehurst.wiremock.client.WireMock.*;

public class WireMockHelloService implements QuarkusTestResourceLifecycleManager { <-- implements life cycle manager

    private WireMockServer wireMockServer;

    @Override
    public Map<String, String> start() {

        // 1. Create instance and start Wiremock server
        wireMockServer = new WireMockServer();
        wireMockServer.start();

        // 2. Create stubs to specific calls
        stubFor(post(urlPathMatching("/api/hello"))
                .withQueryParam("name", matching("^[a-zA-Z0-9_.-]*"))
                .willReturn(aResponse()
                        .withHeader("Content-Type", "application/json")
                        .withBody(
                                "{" +
                                        "\"name\": \"Kurt\"" +
                                        "}"
                        )));

        // 3. Create stub for all other call to perform real calls
        stubFor(get(urlMatching(".*")).atPriority(10).willReturn(aResponse().proxiedFrom("https://restcountries.eu/rest")));

        // 4. Returns configuration that applies for tests and overwrite the REST enpoint URL with the WireMock URL
        Map<String, String> restUriMap = new HashMap<>();
        restUriMap.put("com.sample.proxy.HelloService/mp-rest/uri", wireMockServer.baseUrl());
        return restUriMap;
    }

    @Override
    public void stop() {
        if (null != wireMockServer) {
            wireMockServer.stop();
        }
    }
}
```

</div>

</div>

  

Using Annoation in test class: 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5d59cb17-e3b8-4a76-9a00-1cbb44c5adc0" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom">

<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
import static io.restassured.RestAssured.given;
import static org.hamcrest.CoreMatchers.is;

@QuarkusTest
@QuarkusTestResource(WireMockHelloService.class) // <-- Annotation
public class TestingResourceWireMockTest {

    @Test
    public void shouldReturnHelloMessage() {
        String name = "Kurt";

        Hello expectedBody = new Hello();
        expectedBody.setName("Kurt");

        given().contentType(ContentType.TEXT)
                .queryParam("name", name)
                .when().post("/testing/hello")
                .then()
                .statusCode(200)
                .body(is(new Gson().toJson(expectedBody)));
    }
}
```

</div>

</div>

  

  

## TestContainer

Testcontainers is a Java library that supports JUnit tests, providing lightweight, throwaway instances of common databases, Selenium web browsers, or anything else that can run in a Docker container.

Official page: <a href="https://www.testcontainers.org/" class="external-link" rel="nofollow">https://www.testcontainers.org/</a>

  

# TIPS

- Show all application endpoints: <a href="http://localhost:8080/@documentation" class="external-link" rel="nofollow">http://localhost:8080/@documentation</a>

  

# PROS and CONS


![[46941995722-add.png]]

 Development Mode: Hot deployment with background compilation (Hot Reload)


![[46941995722-add.png]]

 Fast startup (AOT Compilation)


![[46941995722-add.png]]

 High Performance with Native compilation in GraalVM 


![[46941995722-add.png]]

 Reactive


![[46941995722-add.png]]

 Using Java EE libraries


![[46941995722-add.png]]

 Eclipse Microprofile 3.2 compatible


![[46941995722-add.png]]

 Kubernetes and Cloud Native support

  


![[46941995722-forbidden.png]]

 Lack of documentation


![[46941995722-forbidden.png]]

 Small community

# File

<div>

<table style="width: 70.7006%;">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr>
<th>Name</th>
<th>File</th>
<th><div class="content-wrapper">
<p>Download Date / Link</p>
</div></th>
</tr>
&#10;<tr>
<td>Quarkus Cheat sheet</td>
<td><div class="content-wrapper">
<span class="confluence-embedded-file-wrapper conf-macro output-inline" data-hasbody="false" data-macro-id="6679e80f-a026-46fa-968d-eaaaf00e386c" data-macro-name="view-file"><a href="../_attachments/46941995722-quarkus-cheat-sheet.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/46941995722/quarkus-cheat-sheet.pdf?version=1&amp;modificationDate=1631193991671&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[46941995722-quarkus-cheat-sheet.pdf]]

</a></span>
</div></td>
<td><div class="content-wrapper">
09 Sep 2021 <a href="https://lordofthejars.github.io/quarkus-cheat-sheet/" class="external-link" rel="nofollow">https://lordofthejars.github.io/quarkus-cheat-sheet/</a>
</div></td>
</tr>
<tr>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

</div>

# Troubleshoot

<div>

<table style="width: 87.7445%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th>Nr</th>
<th>Title</th>
<th>Problem</th>
<th>Solution</th>
</tr>
&#10;<tr>
<td>1</td>
<td>Build Docker image with graalvm native image - OutOfMemory (Code 137)</td>
<td>Image generation failed. Exit code was 137 which indicates an out of memory error. Consider increasing the Xmx value for native image generation by setting the "quarkus.native.native-image-xmx" property</td>
<td><div class="content-wrapper">
<p>Increase Docker memory to ~5GB</p>

![[46941995722-Settings_und_Sicherheit___Datenschutz.png]]


</div></td>
</tr>
<tr>
<td>2</td>
<td>Nativ Image Build Error - SSLConnectionSocketFactory</td>
<td><div class="content-wrapper">
<p>Error message if run the Native Image Build command: </p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="f7caaa28-7732-4cd3-9ca7-38de6083b5ff" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: bash; gutter: false; theme: Confluence" data-theme="Confluence"><code>./mvnw clean package -Pnative -Dquarkus.native.container-build=true -Dquarkus.native.container-runtime=docker -Dquarkus.container-image.build=true -DskipTests=true</code></pre>
</div>
</div>
<p><strong>Error message: </strong></p>
<p>com.oracle.svm.core.util.UserError$UserException: Classes that should be initialized at run time got initialized during image building:<br />
org.apache.http.conn.ssl.SSLConnectionSocketFactory the class was requested to be initialized at run time (from feature io.quarkus.runner.AutoFeature.beforeAnalysis with 'SSLConnectionSocketFactory.class')<br />
...</p>
<p><strong>Stack trace: </strong></p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="eb81493b-76d5-46c2-a70c-6e41c1e56179" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom">
<strong></strong> <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code> at org.apache.http.conn.ssl.SSLConnectionSocketFactory.&lt;clinit&gt;(SSLConnectionSocketFactory.java:151)
        at org.jboss.resteasy.client.jaxrs.engines.ClientHttpEngineBuilder43.build(ClientHttpEngineBuilder43.java:170)
        at org.jboss.resteasy.client.jaxrs.internal.ResteasyClientBuilderImpl.build(ResteasyClientBuilderImpl.java:415)
        at io.quarkus.restclient.runtime.QuarkusRestClientBuilder.build(QuarkusRestClientBuilder.java:332)
        at io.quarkus.restclient.runtime.RestClientBase.create(RestClientBase.java:69)
        at com.sample.proxy.JwtTokenService_3af0d8a1d04574dd1d9f7b11918d4e696d57e12b_Synthetic_Bean.create(JwtTokenService_3af0d8a1d04574dd1d9f7b11918d4e696d57e12b_Synthetic_Bean.zig:189)
        at com.sample.proxy.JwtTokenService_3af0d8a1d04574dd1d9f7b11918d4e696d57e12b_Synthetic_Bean.get(JwtTokenService_3af0d8a1d04574dd1d9f7b11918d4e696d57e12b_Synthetic_Bean.zig:220)
        at com.sample.proxy.JwtTokenService_3af0d8a1d04574dd1d9f7b11918d4e696d57e12b_Synthetic_Bean.get(JwtTokenService_3af0d8a1d04574dd1d9f7b11918d4e696d57e12b_Synthetic_Bean.zig:243)
        at io.quarkus.arc.impl.CurrentInjectionPointProvider.get(CurrentInjectionPointProvider.java:52)
        at com.sample.filter.AuthTokenFilter_Bean.create(AuthTokenFilter_Bean.zig:346)
        at com.sample.filter.AuthTokenFilter_Bean.create(AuthTokenFilter_Bean.zig:429)
        at io.quarkus.arc.impl.AbstractSharedContext.createInstanceHandle(AbstractSharedContext.java:96)
        at io.quarkus.arc.impl.AbstractSharedContext$1.get(AbstractSharedContext.java:29)
        at io.quarkus.arc.impl.AbstractSharedContext$1.get(AbstractSharedContext.java:26)
        at io.quarkus.arc.impl.LazyValue.get(LazyValue.java:26)
        at io.quarkus.arc.impl.ComputingCache.computeIfAbsent(ComputingCache.java:69)
        at io.quarkus.arc.impl.AbstractSharedContext.get(AbstractSharedContext.java:26)
        at com.sample.filter.AuthTokenFilter_Bean.get(AuthTokenFilter_Bean.zig:461)
        at com.sample.filter.AuthTokenFilter_Bean.get(AuthTokenFilter_Bean.zig:477)
        at io.quarkus.arc.impl.ArcContainerImpl.beanInstanceHandle(ArcContainerImpl.java:434)
        at io.quarkus.arc.impl.ArcContainerImpl.beanInstanceHandle(ArcContainerImpl.java:447)
        at io.quarkus.arc.impl.ArcContainerImpl$1.get(ArcContainerImpl.java:270)
        at io.quarkus.arc.impl.ArcContainerImpl$1.get(ArcContainerImpl.java:267)
        at io.quarkus.resteasy.common.runtime.QuarkusConstructorInjector.construct(QuarkusConstructorInjector.java:39)
        at org.jboss.resteasy.core.providerfactory.ResteasyProviderFactoryImpl.injectedInstance(ResteasyProviderFactoryImpl.java:1399)
        at org.jboss.resteasy.core.interception.jaxrs.JaxrsInterceptorRegistryImpl$AbstractInterceptorFactory.createInterceptor(JaxrsInterceptorRegistryImpl.java:150)
        at org.jboss.resteasy.core.interception.jaxrs.JaxrsInterceptorRegistryImpl$OnDemandInterceptorFactory.initialize(JaxrsInterceptorRegistryImpl.java:168)
        at org.jboss.resteasy.core.interception.jaxrs.JaxrsInterceptorRegistryImpl$OnDemandInterceptorFactory.checkInitialize(JaxrsInterceptorRegistryImpl.java:183)
        at org.jboss.resteasy.core.interception.jaxrs.JaxrsInterceptorRegistryImpl$OnDemandInterceptorFactory.getInterceptor(JaxrsInterceptorRegistryImpl.java:193)
        at org.jboss.resteasy.core.interception.jaxrs.JaxrsInterceptorRegistryImpl$AbstractInterceptorFactory.postMatch(JaxrsInterceptorRegistryImpl.java:131)
        at org.jboss.resteasy.core.interception.jaxrs.JaxrsInterceptorRegistryImpl.postMatch(JaxrsInterceptorRegistryImpl.java:288)
        at org.jboss.resteasy.core.interception.jaxrs.ContainerRequestFilterRegistryImpl.postMatch(ContainerRequestFilterRegistryImpl.java:30)
        at org.jboss.resteasy.core.interception.jaxrs.ContainerRequestFilterRegistryImpl.postMatch(ContainerRequestFilterRegistryImpl.java:12)
        at org.jboss.resteasy.core.ResourceMethodInvoker.&lt;init&gt;(ResourceMethodInvoker.java:142)
        at org.jboss.resteasy.core.ResourceMethodRegistry.processMethod(ResourceMethodRegistry.java:381)
        at org.jboss.resteasy.core.ResourceMethodRegistry.register(ResourceMethodRegistry.java:308)
        at org.jboss.resteasy.core.ResourceMethodRegistry.addResourceFactory(ResourceMethodRegistry.java:259)
        at org.jboss.resteasy.core.ResourceMethodRegistry.addResourceFactory(ResourceMethodRegistry.java:227)
        at org.jboss.resteasy.core.ResourceMethodRegistry.addResourceFactory(ResourceMethodRegistry.java:208)
        at org.jboss.resteasy.core.ResourceMethodRegistry.addResourceFactory(ResourceMethodRegistry.java:192)
        at org.jboss.resteasy.core.ResourceMethodRegistry.addResourceFactory(ResourceMethodRegistry.java:175)
        at org.jboss.resteasy.core.ResourceMethodRegistry.addPerRequestResource(ResourceMethodRegistry.java:87)
        at org.jboss.resteasy.core.ResteasyDeploymentImpl.registerResources(ResteasyDeploymentImpl.java:518)
        at org.jboss.resteasy.core.ResteasyDeploymentImpl.registration(ResteasyDeploymentImpl.java:475)
        at org.jboss.resteasy.core.ResteasyDeploymentImpl.startInternal(ResteasyDeploymentImpl.java:164)
        at org.jboss.resteasy.core.ResteasyDeploymentImpl.start(ResteasyDeploymentImpl.java:121)
        at org.jboss.resteasy.plugins.server.servlet.ServletContainerDispatcher.init(ServletContainerDispatcher.java:144)
        at org.jboss.resteasy.plugins.server.servlet.FilterDispatcher.init(FilterDispatcher.java:47)
        at io.undertow.servlet.core.LifecyleInterceptorInvocation.proceed(LifecyleInterceptorInvocation.java:112)
        at io.undertow.servlet.core.ManagedFilter.createFilter(ManagedFilter.java:80)
        at io.undertow.servlet.core.DeploymentManagerImpl$2.call(DeploymentManagerImpl.java:591)
        at io.undertow.servlet.core.DeploymentManagerImpl$2.call(DeploymentManagerImpl.java:556)
        at io.undertow.servlet.core.ServletRequestContextThreadSetupAction$1.call(ServletRequestContextThreadSetupAction.java:42)
        at io.undertow.servlet.core.ContextClassLoaderSetupAction$1.call(ContextClassLoaderSetupAction.java:43)
        at io.quarkus.undertow.runtime.UndertowDeploymentRecorder$9$1.call(UndertowDeploymentRecorder.java:569)
        at io.undertow.servlet.core.DeploymentManagerImpl.start(DeploymentManagerImpl.java:598)
        at io.quarkus.undertow.runtime.UndertowDeploymentRecorder.bootServletContainer(UndertowDeploymentRecorder.java:520)
        at io.quarkus.deployment.steps.UndertowBuildStep$build-649634386.deploy_1(UndertowBuildStep$build-649634386.zig:2730)
        at io.quarkus.deployment.steps.UndertowBuildStep$build-649634386.deploy(UndertowBuildStep$build-649634386.zig:45)
        at io.quarkus.runner.ApplicationImpl.&lt;clinit&gt;(ApplicationImpl.zig:271)
&#10;
        at com.oracle.svm.core.util.UserError.abort(UserError.java:68)
        at com.oracle.svm.hosted.classinitialization.ConfigurableClassInitialization.checkDelayedInitialization(ConfigurableClassInitialization.java:555)
        at com.oracle.svm.hosted.classinitialization.ClassInitializationFeature.duringAnalysis(ClassInitializationFeature.java:169)
        at com.oracle.svm.hosted.NativeImageGenerator.lambda$runPointsToAnalysis$12(NativeImageGenerator.java:730)
        at com.oracle.svm.hosted.FeatureHandler.forEachFeature(FeatureHandler.java:71)
        at com.oracle.svm.hosted.NativeImageGenerator.runPointsToAnalysis(NativeImageGenerator.java:730)
        at com.oracle.svm.hosted.NativeImageGenerator.doRun(NativeImageGenerator.java:532)
        at com.oracle.svm.hosted.NativeImageGenerator.run(NativeImageGenerator.java:491)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner.buildImage(NativeImageGeneratorRunner.java:380)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner.build(NativeImageGeneratorRunner.java:543)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner.main(NativeImageGeneratorRunner.java:119)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner$JDK9Plus.main(NativeImageGeneratorRunner.java:573)
</code></pre>
</div>
</div>
</div></td>
<td><p>Solution: </p>
<p>Using Interceptor instead of filter which Injects the REST client with @RestClient annotation.</p>
<p><br />
</p>
<p>Issue tracked on Quarkus chat:</p>
<p><a href="https://quarkusio.zulipchat.com/#narrow/stream/187030-users/topic/Error.20building.20native.20image.20.20-.20SSLConnectionSocketFactory" class="external-link" rel="nofollow">https://quarkusio.zulipchat.com/#narrow/stream/187030-users/topic/Error.20building.20native.20image.20.20-.20SSLConnectionSocketFactory</a></p>
<p><br />
</p>
<p><br />
</p></td>
</tr>
<tr>
<td>3</td>
<td>Nativ Image Build Error - UnresolvedElementException</td>
<td><p>Error message: </p>
<p>Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.Integer.describeConstable(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.</p>
<p>Stack trace: </p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5a101e3a-3eab-4d04-b6b5-7ce63be2d68a" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl hide-border-bottom">
<strong></strong> <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>
</div>
<div class="codeContent panelContent pdl hide-toolbar">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code>Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.Integer.describeConstable(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.
Trace: 
        at parsing io.quarkus.qute.Integer_ValueResolver.resolve(Integer_ValueResolver.zig:151)
Call path from entry point to io.quarkus.qute.Integer_ValueResolver.resolve(EvalContext): 
        at io.quarkus.qute.Integer_ValueResolver.resolve(Integer_ValueResolver.zig:80)
        at io.quarkus.qute.EvaluatorImpl.resolve(EvaluatorImpl.java:175)
        at io.quarkus.qute.EvaluatorImpl.lambda$resolve$3(EvaluatorImpl.java:129)
        at io.quarkus.qute.EvaluatorImpl$$Lambda$1989/0x00000007c1eed840.apply(Unknown Source)
        at sun.security.ec.XECParameters$1.get(XECParameters.java:183)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.initializeLazyValue(SystemPropertiesSupport.java:216)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.getProperty(SystemPropertiesSupport.java:169)
        at com.oracle.svm.core.jdk.Target_java_lang_System.getProperty(JavaLangSubstitutions.java:290)
        at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_lang_System_2_0002egetProperty_00028Ljava_lang_String_2_00029Ljava_lang_String_2(generated:0)
Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.String.translateEscapes(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.
Trace: 
        at parsing io.quarkus.qute.String_ValueResolver.resolve(String_ValueResolver.zig:697)
Call path from entry point to io.quarkus.qute.String_ValueResolver.resolve(EvalContext): 
        at io.quarkus.qute.String_ValueResolver.resolve(String_ValueResolver.zig:80)
        at io.quarkus.qute.EvaluatorImpl.resolve(EvaluatorImpl.java:175)
        at io.quarkus.qute.EvaluatorImpl.lambda$resolve$3(EvaluatorImpl.java:129)
        at io.quarkus.qute.EvaluatorImpl$$Lambda$1989/0x00000007c1eed840.apply(Unknown Source)
        at sun.security.ec.XECParameters$1.get(XECParameters.java:183)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.initializeLazyValue(SystemPropertiesSupport.java:216)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.getProperty(SystemPropertiesSupport.java:169)
        at com.oracle.svm.core.jdk.Target_java_lang_System.getProperty(JavaLangSubstitutions.java:290)
        at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_lang_System_2_0002egetProperty_00028Ljava_lang_String_2_00029Ljava_lang_String_2(generated:0)
&#10;com.oracle.svm.core.util.UserError$UserException: Unsupported features in 2 methods
Detailed message:
Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.Integer.describeConstable(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.
Trace: 
        at parsing io.quarkus.qute.Integer_ValueResolver.resolve(Integer_ValueResolver.zig:151)
Call path from entry point to io.quarkus.qute.Integer_ValueResolver.resolve(EvalContext): 
        at io.quarkus.qute.Integer_ValueResolver.resolve(Integer_ValueResolver.zig:80)
        at io.quarkus.qute.EvaluatorImpl.resolve(EvaluatorImpl.java:175)
        at io.quarkus.qute.EvaluatorImpl.lambda$resolve$3(EvaluatorImpl.java:129)
        at io.quarkus.qute.EvaluatorImpl$$Lambda$1989/0x00000007c1eed840.apply(Unknown Source)
        at sun.security.ec.XECParameters$1.get(XECParameters.java:183)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.initializeLazyValue(SystemPropertiesSupport.java:216)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.getProperty(SystemPropertiesSupport.java:169)
        at com.oracle.svm.core.jdk.Target_java_lang_System.getProperty(JavaLangSubstitutions.java:290)
        at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_lang_System_2_0002egetProperty_00028Ljava_lang_String_2_00029Ljava_lang_String_2(generated:0)
Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.String.translateEscapes(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.
Trace: 
        at parsing io.quarkus.qute.String_ValueResolver.resolve(String_ValueResolver.zig:697)
Call path from entry point to io.quarkus.qute.String_ValueResolver.resolve(EvalContext): 
        at io.quarkus.qute.String_ValueResolver.resolve(String_ValueResolver.zig:80)
        at io.quarkus.qute.EvaluatorImpl.resolve(EvaluatorImpl.java:175)
        at io.quarkus.qute.EvaluatorImpl.lambda$resolve$3(EvaluatorImpl.java:129)
        at io.quarkus.qute.EvaluatorImpl$$Lambda$1989/0x00000007c1eed840.apply(Unknown Source)
        at sun.security.ec.XECParameters$1.get(XECParameters.java:183)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.initializeLazyValue(SystemPropertiesSupport.java:216)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.getProperty(SystemPropertiesSupport.java:169)
        at com.oracle.svm.core.jdk.Target_java_lang_System.getProperty(JavaLangSubstitutions.java:290)
        at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_lang_System_2_0002egetProperty_00028Ljava_lang_String_2_00029Ljava_lang_String_2(generated:0)
&#10;        at com.oracle.svm.core.util.UserError.abort(UserError.java:82)
        at com.oracle.svm.hosted.FallbackFeature.reportAsFallback(FallbackFeature.java:233)
        at com.oracle.svm.hosted.NativeImageGenerator.runPointsToAnalysis(NativeImageGenerator.java:764)
        at com.oracle.svm.hosted.NativeImageGenerator.doRun(NativeImageGenerator.java:532)
        at com.oracle.svm.hosted.NativeImageGenerator.run(NativeImageGenerator.java:491)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner.buildImage(NativeImageGeneratorRunner.java:380)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner.build(NativeImageGeneratorRunner.java:543)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner.main(NativeImageGeneratorRunner.java:119)
        at com.oracle.svm.hosted.NativeImageGeneratorRunner$JDK9Plus.main(NativeImageGeneratorRunner.java:573)
Caused by: com.oracle.graal.pointsto.constraints.UnsupportedFeatureException: Unsupported features in 2 methods
Detailed message:
Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.Integer.describeConstable(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.
Trace: 
        at parsing io.quarkus.qute.Integer_ValueResolver.resolve(Integer_ValueResolver.zig:151)
Call path from entry point to io.quarkus.qute.Integer_ValueResolver.resolve(EvalContext): 
        at io.quarkus.qute.Integer_ValueResolver.resolve(Integer_ValueResolver.zig:80)
        at io.quarkus.qute.EvaluatorImpl.resolve(EvaluatorImpl.java:175)
        at io.quarkus.qute.EvaluatorImpl.lambda$resolve$3(EvaluatorImpl.java:129)
        at io.quarkus.qute.EvaluatorImpl$$Lambda$1989/0x00000007c1eed840.apply(Unknown Source)
        at sun.security.ec.XECParameters$1.get(XECParameters.java:183)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.initializeLazyValue(SystemPropertiesSupport.java:216)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.getProperty(SystemPropertiesSupport.java:169)
        at com.oracle.svm.core.jdk.Target_java_lang_System.getProperty(JavaLangSubstitutions.java:290)
        at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_lang_System_2_0002egetProperty_00028Ljava_lang_String_2_00029Ljava_lang_String_2(generated:0)
Error: com.oracle.graal.pointsto.constraints.UnresolvedElementException: Discovered unresolved method during parsing: java.lang.String.translateEscapes(). To diagnose the issue you can use the --allow-incomplete-classpath option. The missing method is then reported at run time when it is accessed the first time.
Trace: 
        at parsing io.quarkus.qute.String_ValueResolver.resolve(String_ValueResolver.zig:697)
Call path from entry point to io.quarkus.qute.String_ValueResolver.resolve(EvalContext): 
        at io.quarkus.qute.String_ValueResolver.resolve(String_ValueResolver.zig:80)
        at io.quarkus.qute.EvaluatorImpl.resolve(EvaluatorImpl.java:175)
        at io.quarkus.qute.EvaluatorImpl.lambda$resolve$3(EvaluatorImpl.java:129)
        at io.quarkus.qute.EvaluatorImpl$$Lambda$1989/0x00000007c1eed840.apply(Unknown Source)
        at sun.security.ec.XECParameters$1.get(XECParameters.java:183)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.initializeLazyValue(SystemPropertiesSupport.java:216)
        at com.oracle.svm.core.jdk.SystemPropertiesSupport.getProperty(SystemPropertiesSupport.java:169)
        at com.oracle.svm.core.jdk.Target_java_lang_System.getProperty(JavaLangSubstitutions.java:290)
        at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_lang_System_2_0002egetProperty_00028Ljava_lang_String_2_00029Ljava_lang_String_2(generated:0)
&#10;        at com.oracle.graal.pointsto.constraints.UnsupportedFeatures.report(UnsupportedFeatures.java:129)
        at com.oracle.svm.hosted.NativeImageGenerator.runPointsToAnalysis(NativeImageGenerator.java:761)
        ... 6 more
</code></pre>
</div>
</div></td>
<td><p>Solution: </p>
<ul>
<li><p>Configure application.properties with the following entry (<code>--allow-incomplete-classpath</code>): </p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="471802dd-994d-4004-a5ea-f66b7041749b" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>quarkus.native.additional-build-args=--allow-incomplete-classpath</code></pre>
</div>
</div></li>
</ul>
<p>The missing method is then reported at run time when it is accessed the first time.</p></td>
</tr>
<tr>
<td>4</td>
<td>Reactive incompatible with undertow </td>
<td><p>Issue: </p>
<ul>
<li><a href="https://github.com/quarkusio/quarkus/issues/14997" class="external-link" rel="nofollow">https://github.com/quarkusio/quarkus/issues/14997</a> </li>
<li><a href="https://github.com/quarkusio/quarkus/pull/20886" class="external-link" rel="nofollow">https://github.com/quarkusio/quarkus/pull/20886</a></li>
</ul>
<p>Routes conflicts between reactive routes and undertow servlet routes </p>
<p>Additional Bug report: <a href="https://issueexplorer.com/issue/quarkusio/quarkus/20302" class="external-link" rel="nofollow">https://issueexplorer.com/issue/quarkusio/quarkus/20302</a> → This was fixed in <a href="https://github.com/quarkusio/quarkus/pull/20886" class="external-link" rel="nofollow">#20886</a> and should be available when Quarkus 2.5 is released</p>
<p>Quarkus path resolution: </p>
<p><a href="https://quarkus.io/blog/path-resolution-in-quarkus/" class="external-link" rel="nofollow">https://quarkus.io/blog/path-resolution-in-quarkus/</a></p></td>
<td><p>This issue might be fixed in Quarkus 2.5 (current version is 2.4.1.Final )</p></td>
</tr>
<tr>
<td>5</td>
<td>Runtime error for native image build . </td>
<td><div class="content-wrapper">
<p>POJOs default constructor not available issue: </p>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="e887ec04-6e41-4d4a-aab4-00ef262bd075" data-macro-name="code" style="border-width: 1px;">
<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
<strong>JsonbException</strong>
</div>
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>2021-11-15 11:13:09,978 ERROR [com.sam.int.InitializeSessionInterceptor] (executor-thread-0) initializeSessionWithPublicToken catched: %s: javax.json.bind.JsonbException: Cannot create instance of a class: class com.sample.dto.auth.TokenResponse, No default constructor found.
        at org.eclipse.yasson.internal.serializer.ObjectDeserializer.getInstance(ObjectDeserializer.java:101)
        at org.eclipse.yasson.internal.serializer.AbstractContainerDeserializer.deserialize(AbstractContainerDeserializer.java:65)
        at org.eclipse.yasson.internal.Unmarshaller.deserializeItem(Unmarshaller.java:62)
        at org.eclipse.yasson.internal.Unmarshaller.deserialize(Unmarshaller.java:51)
        at org.eclipse.yasson.internal.JsonBinding.deserialize(JsonBinding.java:59)
        at org.eclipse.yasson.internal.JsonBinding.fromJson(JsonBinding.java:66)
        at com.sample.session.SessionWrapper.getJwtPublicToken(SessionWrapper.java:169)
        at com.sample.session.SessionWrapper.buildAuthTokens(SessionWrapper.java:127)
        at com.sample.session.SessionWrapper.createAuthSessionWithPublicToken(SessionWrapper.java:118)
        at com.sample.session.SessionWrapper.initializeSessionWithPublicToken(SessionWrapper.java:100)
        at com.sample.session.SessionWrapper_ClientProxy.initializeSessionWithPublicToken(SessionWrapper_ClientProxy.zig:331)
        at com.sample.interceptor.InitializeSessionInterceptor.initializeAuthSessionOrRedirectToLogout(InitializeSessionInterceptor.java:28)
        at com.sample.interceptor.InitializeSessionInterceptor_Bean.intercept(InitializeSessionInterceptor_Bean.zig:327)
        at io.quarkus.arc.impl.InterceptorInvocation.invoke(InterceptorInvocation.java:41)
        at io.quarkus.arc.impl.AroundInvokeInvocationContext.proceed(AroundInvokeInvocationContext.java:50)
        at io.quarkus.security.runtime.interceptor.SecurityHandler.handle(SecurityHandler.java:24)
        at io.quarkus.security.runtime.interceptor.AuthenticatedInterceptor.intercept(AuthenticatedInterceptor.java:29)
        at io.quarkus.security.runtime.interceptor.AuthenticatedInterceptor_Bean.intercept(AuthenticatedInterceptor_Bean.zig:378)
        at io.quarkus.arc.impl.InterceptorInvocation.invoke(InterceptorInvocation.java:41)
        at io.quarkus.arc.impl.AroundInvokeInvocationContext.perform(AroundInvokeInvocationContext.java:41)
        at io.quarkus.arc.impl.InvocationContexts.performAroundInvoke(InvocationContexts.java:32)
        at com.sample.TenantResource_Subclass.getUserTenants(TenantResource_Subclass.zig:949)
        at java.lang.reflect.Method.invoke(Method.java:566)
        at org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:170)
        at org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:130)
        at org.jboss.resteasy.core.ResourceMethodInvoker.internalInvokeOnTarget(ResourceMethodInvoker.java:660)
        at org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTargetAfterFilter(ResourceMethodInvoker.java:524)
        at org.jboss.resteasy.core.ResourceMethodInvoker.lambda$invokeOnTarget$2(ResourceMethodInvoker.java:474)
        at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:364)
        at org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTarget(ResourceMethodInvoker.java:476)
        at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:434)
        at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:408)
        at org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:69)
        at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:492)
        at org.jboss.resteasy.core.SynchronousDispatcher.lambda$invoke$4(SynchronousDispatcher.java:261)
        at org.jboss.resteasy.core.SynchronousDispatcher.lambda$preprocess$0(SynchronousDispatcher.java:161)
        at org.jboss.resteasy.core.interception.jaxrs.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:364)
        at org.jboss.resteasy.core.SynchronousDispatcher.preprocess(SynchronousDispatcher.java:164)
        at org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:247)
        at org.jboss.resteasy.plugins.server.servlet.ServletContainerDispatcher.service(ServletContainerDispatcher.java:249)
        at io.quarkus.resteasy.runtime.ResteasyFilter$ResteasyResponseWrapper.service(ResteasyFilter.java:70)
        at io.quarkus.resteasy.runtime.ResteasyFilter$ResteasyResponseWrapper.sendError(ResteasyFilter.java:76)
        at io.undertow.servlet.handlers.DefaultServlet.doGet(DefaultServlet.java:172)
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:503)
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:590)
        at io.undertow.servlet.handlers.ServletHandler.handleRequest(ServletHandler.java:74)
        at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:129)
        at io.quarkus.resteasy.runtime.ResteasyFilter.doFilter(ResteasyFilter.java:31)
        at io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61)
        at io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131)
        at io.undertow.servlet.handlers.FilterHandler.handleRequest(FilterHandler.java:84)
        at io.undertow.servlet.handlers.security.ServletSecurityRoleHandler.handleRequest(ServletSecurityRoleHandler.java:63)
        at io.undertow.servlet.handlers.ServletChain$1.handleRequest(ServletChain.java:68)
        at io.undertow.servlet.handlers.ServletDispatchingHandler.handleRequest(ServletDispatchingHandler.java:36)
        at io.undertow.servlet.handlers.RedirectDirHandler.handleRequest(RedirectDirHandler.java:67)
        at io.undertow.servlet.handlers.security.SSLInformationAssociationHandler.handleRequest(SSLInformationAssociationHandler.java:133)
        at io.undertow.servlet.handlers.security.ServletAuthenticationCallHandler.handleRequest(ServletAuthenticationCallHandler.java:57)
        at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
        at io.undertow.security.handlers.AbstractConfidentialityHandler.handleRequest(AbstractConfidentialityHandler.java:46)
        at io.undertow.servlet.handlers.security.ServletConfidentialityConstraintHandler.handleRequest(ServletConfidentialityConstraintHandler.java:65)
        at io.undertow.security.handlers.AuthenticationMechanismsHandler.handleRequest(AuthenticationMechanismsHandler.java:60)
        at io.undertow.servlet.handlers.security.CachedAuthenticatedSessionHandler.handleRequest(CachedAuthenticatedSessionHandler.java:77)
        at io.undertow.security.handlers.NotificationReceiverHandler.handleRequest(NotificationReceiverHandler.java:50)
        at io.undertow.security.handlers.AbstractSecurityContextAssociationHandler.handleRequest(AbstractSecurityContextAssociationHandler.java:43)
        at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
        at io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
        at io.undertow.servlet.handlers.ServletInitialHandler.handleFirstRequest(ServletInitialHandler.java:247)
        at io.undertow.servlet.handlers.ServletInitialHandler.access$100(ServletInitialHandler.java:56)
        at io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:111)
        at io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:108)
        at io.undertow.servlet.core.ServletRequestContextThreadSetupAction$1.call(ServletRequestContextThreadSetupAction.java:48)
        at io.undertow.servlet.core.ContextClassLoaderSetupAction$1.call(ContextClassLoaderSetupAction.java:43)
        at io.quarkus.undertow.runtime.UndertowDeploymentRecorder$9$1.call(UndertowDeploymentRecorder.java:593)
        at io.undertow.servlet.handlers.ServletInitialHandler.dispatchRequest(ServletInitialHandler.java:227)
        at io.undertow.servlet.handlers.ServletInitialHandler.handleRequest(ServletInitialHandler.java:152)
        at io.quarkus.undertow.runtime.UndertowDeploymentRecorder$1.handleRequest(UndertowDeploymentRecorder.java:119)
        at io.undertow.server.Connectors.executeRootHandler(Connectors.java:290)
        at io.undertow.server.DefaultExchangeHandler.handle(DefaultExchangeHandler.java:18)
        at io.quarkus.undertow.runtime.UndertowDeploymentRecorder$5$1.run(UndertowDeploymentRecorder.java:415)
        at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
        at java.util.concurrent.FutureTask.run(FutureTask.java:264)
        at io.quarkus.vertx.core.runtime.VertxCoreRecorder$13.runWith(VertxCoreRecorder.java:543)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2449)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1478)
        at org.jboss.threads.DelegatingRunnable.run(DelegatingRunnable.java:29)
        at org.jboss.threads.ThreadLocalResettingRunnable.run(ThreadLocalResettingRunnable.java:29)
        at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
        at java.lang.Thread.run(Thread.java:829)
        at com.oracle.svm.core.thread.JavaThreads.threadStartRoutine(JavaThreads.java:567)
        at com.oracle.svm.core.posix.thread.PosixJavaThreads.pthreadStartRoutine(PosixJavaThreads.java:192)
&#10;</code></pre>
</div>
</div>
</div></td>
<td><p>RegisterForReflection Annotation needed for POJOs. Otherwise GraalVM will remove <span>classes/methods/fields</span> <span>that are not used directly.</span></p>
<p>Reference: <a href="https://quarkus.io/guides/writing-native-applications-tips#using-the-registerforreflection-annotation" class="external-link" rel="nofollow">https://quarkus.io/guides/writing-native-applications-tips#using-the-registerforreflection-annotation</a></p></td>
</tr>
</tbody>
</table>

</div>

# Discussion 

# More information 

- <a href="https://quarkus.io/" class="external-link" rel="nofollow">https://quarkus.io/</a>
- Qute Templating Engine: <a href="https://quarkus.io/guides/qute" class="external-link" rel="nofollow">https://quarkus.io/guides/qute</a>
- Qute Templating referende guide: <a href="https://quarkus.io/guides/qute-reference" class="external-link" rel="nofollow">https://quarkus.io/guides/qute-reference</a>
- Quarkus quickstarts samples: <a href="https://github.com/quarkusio/quarkus-quickstarts" class="external-link" rel="nofollow">https://github.com/quarkusio/quarkus-quickstarts</a>
- Docker CLI dreference: <a href="https://docs.docker.com/engine/reference/run/" class="external-link" rel="nofollow">https://docs.docker.com/engine/reference/run/</a>
- Quarkus Kubernetes deployment: <a href="https://quarkus.io/guides/deploying-to-kubernetes" class="external-link" rel="nofollow">https://quarkus.io/guides/deploying-to-kubernetes</a>
- Performance comparision between Quarkus native and jvm: <a href="https://quarkus.io/blog/runtime-performance/" class="external-link" rel="nofollow">https://quarkus.io/blog/runtime-performance/</a>
- Performacnce messure:<a href="https://quarkus.io/guides/performance-measure" class="external-link" rel="nofollow"> https://quarkus.io/guides/performance-measure</a>
- Quarkus.io GIT Repo: <a href="https://github.com/quarkusio" class="external-link" rel="nofollow">https://github.com/quarkusio</a>

%% ai-graph-start %%

**Related notes:**
- [[Quarkus]]
- [[Migrate to Quarkus (WIP)]]
- [[Recipe Deploy with Terraform]]
- [[2.31 Build & deploy agent review service to k8s (POC)]]
- [[06 - How to test a Rest API with authorization]]

%% ai-graph-end %%