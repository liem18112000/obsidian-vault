---
ai_hash: 6f8895b52cdba977
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 2.86
entities: []
relevance: 0.846
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47498232106/Postgres+Architecture+Blueprint+V2023
space: LUZ
status: reference
tags:
- confluence
- architecture
- space/luz
title: Postgres Architecture Blueprint V2023
topic: architecture
type: source
updated: 2023-10-15
---

# Postgres Architecture Blueprint V2023

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-10-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47498232106/Postgres+Architecture+Blueprint+V2023)
> Relevance 0.846 · topic `architecture`

This document describes the system architecture to store relational data in the KLARA backends in Postgres. This architecture represents an evolution that was developed during 2023 by Team CloudNinja and is expected to be rolled out by the end of 2023 to production. Therefore, the version designation V2023 is used.

The following diagram shows the logical data architecture used for relational data in KLARA.


![[47498232106-Logical Data View.png]]



In the diagram you can see that we use different logical databases for public data and also for individual tenant data. This means that the tenant-specific data is stored in separate logical databases for every single tenant. The term "logical database" is used because, although this is how the data is viewed by the user, it does not necessarily correspond to the physical architecture of the databases.

For the sake of this example, we’re only covering the 3 modules shown in the diagram. But the techniques discussed on this page can be easily adapted to other modules too.

The following 2 diagrams now show the software components involved - starting from a backend module to the database. The 1st diagram shows it when using the one-db-per-tenant approach and the 2nd with the one-schema-per-tenant approach:


![[47498232106-Software Components.png]]



In this variant, the new approach stores all data belonging to a tenant or public data in one single database. Module specific data is separated by schemas. Several of those databases are grouped and managed in one database cluster.

The following assumptions have been made for the diagram:

- `luz-keyvaluestore` has been completely migrated to the new approach.

- `luz-person` has been partially migrated to the new approach:

  - `public` data has been migrated to the new approach

  - `tenant-1` data is still stored according to the old approach

  - `tenant-2` data is stored according to the new approach

  - new tenants (`tenant-n`) are created according to the new approach

- `luz-compensation` has not yet been touched and therefore stores everything according to the old approach.


![[47498232106-Software Components Schema-based.png]]



In the 2nd variant, all data belonging to a module is stored in a single database. Tenant-specific data is separated by schemas. Several of such database may be managed in one single database cluster.

The following assumptions have been made for the diagram:

- `luz-keyvaluestore` has been migrated to a new database cluster. All tenants are stored in one database since we assume that the amount of data is low.

- `luz-person` has been partially migrated to new clusters:

  - `public` data has been migrated to `cluster2`

  - Data belonging to tenants with an odd number (e.g. `tenant-1`) is stored in `cluster2`

  - Data belonging to tenants with an even number (e.g. `tenant-2`) is stored in `cluster3`

- `luz-compensation` has not yet been touched.

# Questions

- Regarding the two variants above, what would be the resource consumptions regarding \# of connections and possibly also amount of memory (based on the number of connections)? Lets do the math with the following setting: 100 backend modules with data, 2 pods for each backend module, 3 nodes of PgBouncer and 100’000 tenants.

<a href="https://axonivy.atlassian.net/wiki/people/557058:e4d192ef-5588-458c-80d6-426683fb7f17?ref=confluence" class="confluence-userlink user-mention" data-account-id="557058:e4d192ef-5588-458c-80d6-426683fb7f17" target="_blank" data-base-url="https://axonivy.atlassian.net/wiki">Ravi Soni</a> : Can you please provide some rough estimates for the previous question?

Idle PostgreSQL connection use minimum 1.5 MB memory (If other memory relates setting are not tweaked). See Reference blog from Amazone.

100 Module x 2 Pods = 200 modules

200 Module x 20 Wildfly DataSource pool connection (Max) = 4000 Connection

Wildfly to each PGBouncer 4000 Connection

### PostgreSQL Cluster memory requirement.

#### Option 1 (Min 2), 100,000 Tenant Database

100000 Tenant x 2 Min Connection x 1.5 MB memory per Connection = 300 GB memory.

100000 Tenant can host on 3 or 5 PostgreSQL Cluster to reduce load per PostgreSQL Cluster.

3 PostgreSQL Cluster Setup

300000 Connection / 3 PostgreSQL Cluster = 100000 Connection (100G Memory) for 33333 tenant per cluster

**Note: GCP AlloyDB can allow max connection 240,000**, so not possible to have a database server with 100,000 tenants for 3 connection per tenant databases, PostgreSQL Cluster must be split into smaller size.

#### Option 2 (Min 0)

A Lazy initialization of connection when first SQL query execution request is received, PGBouncer create connection pool on the fly.

PGBouncer Configuration

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e80a978b-cd4b-4826-99e6-c069230a70da" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[databases]
# Lookup Tenant database on the fly for first connection and init pool creation
* = host=postgres-service auth_user=postgres

[pgbouncer]
min_pool_size = 0
server_idle_timeout=30
server_lifetime=120

# # Lookup Tenant database password on PostgreSQL cluster for authantication on first request.
auth_query = SELECT usename, passwd FROM pg_shadow WHERE usename=$1
```

</div>

</div>

This would close all idle connection for 120 Seconds. Closing connection would reduce memory requirements.

if some tenant is not active, no connection will be created, this would help to reduce memory requirements.

First connection establishment would be bit alow, kind of 500ms latency, as PGBouncer would be establishing the connections for pool creation.

Note:

AlloyDB limits an instance's maximum concurrent connections to 1,000, unless you set its `max_connections` flag to a higher value. You can <a href="https://cloud.google.com/alloydb/docs/instance-configure-database-flags" class="external-link" rel="nofollow">adjust the value of this flag</a> to as high as 240,000.

Reference blogs

<a href="https://aws.amazon.com/blogs/database/resources-consumed-by-idle-postgresql-connections/" class="external-link" data-card-appearance="inline" rel="nofollow">https://aws.amazon.com/blogs/database/resources-consumed-by-idle-postgresql-connections/</a>

<a href="https://aws.amazon.com/blogs/database/performance-impact-of-idle-postgresql-connections/" class="external-link" data-card-appearance="inline" rel="nofollow">https://aws.amazon.com/blogs/database/performance-impact-of-idle-postgresql-connections/</a>

- How are the connections and connection pools \#1 handled?

- How are the connections configured in the backend modules?

WildFly DataSource need to be configured to connect to PGBouncer host only. see below example.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="43627559-517d-4404-9a94-85574450a49e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<datasource jndi-name="java:jboss/datasources/luz-ds1" pool-name="LuzPostgresDS-1">
    <connection-url>jdbc:postgresql://localhost:6432/</connection-url>
    <driver>postgresql</driver>
    <pool>
        <allow-multiple-users>true</allow-multiple-users>
    </pool>
    <validation>
        <valid-connection-checker class-name="org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker"/>
        <check-valid-connection-sql>SELECT 1 FROM DUAL;</check-valid-connection-sql>
        <validate-on-match>true</validate-on-match>
        <background-validation>false</background-validation>
        <stale-connection-checker class-name="org.jboss.jca.adapters.jdbc.extensions.postgres.StaleConnectionChecker"/>
        <exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter"/>
    </validation>
</datasource>
```

</div>

</div>

The Key point in this configuration is as follow.

A Connection URL only points to PGBouncer only, No Database name added into connection URL. i.e `<connection-url>jdbc:postgresql://localhost:6432/</connection-url>`

This allows to JDBC Driver to connect to any database on this cluster on the fly using database Username/password. i. e. `Connection connection = ds.getConnection(username, password);`

PostgreSQL find Database name same as Username.

A Wildfly DataSource is configured all multiple user connection mode as follow.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5362dc48-b841-47e3-8a3e-4e7e11c8a23a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<pool>
    <allow-multiple-users>true</allow-multiple-users>
</pool>
```

</div>

</div>

- Do we need to update the connection configuration of modules not yet migrated?

- How can the connection pooler determine where certain tenant data is available?

# Installation/Configuration

## Tenant DB configuration

TBD: How is Redis installed? What data do we need in the tenant db configuration, i.e. Redis?

### Radis based Tenant lookup configurations.

Radis Server used in consideration of Google Cloud Memorystreo service. Ref: <a href="https://cloud.google.com/memorystore?hl=en" class="external-link" data-card-appearance="inline" rel="nofollow">https://cloud.google.com/memorystore?hl=en</a>

Google Cloud Memorystreo provide Hosted Redis Cluster server to host a most reliable service.

Radis Operator can be also used for Kubernetes deployment. Ref: <a href="https://operatorhub.io/operator/redis-operator" class="external-link" data-card-appearance="inline" rel="nofollow">https://operatorhub.io/operator/redis-operator</a>

Redis Cache store two different set of configurations,

A Configuration of Postgres Connection detail to use for Flyway Migration script.

Flyway Migration process use Direct connection to the database using a JDBC Connection.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1a04bfa3-9534-4108-a2fb-7faff92f4d81" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Flyway directly connect to the database and schema to migraiton.
flyway.setDataSource(dsConfig.getUrl(),dsConfig.getUserName(),dsConfig.getPassword());
flyway.setSchemas(dsConfig.getSchema());
```

</div>

</div>

Database Tenant Configuration, Database tenant use Postgres User to connect to PostgreSQL Cluster to create a Tenant User and database with schema with name of Module.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="481f3c96-8d26-4c1d-99b5-d2f0f0f728f2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Database tenant PostgreSql cluster Configuration
set luz_keyvaluestore-ds1 10.110.56.149=6432=postgres=postgres=postgres
```

</div>

</div>

Schema Tenant Configuration. Schema tenant configuration connect to module database and a schema to using module specific database user.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8748e67b-740d-41a7-a688-dc2166e9beaf" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Schema Tenant PostgreSql Cluster Configuration
set luz_keyvaluestore 10.110.56.149=6432=luzkeyvaluestore=luzkeyvaluestore=luzkeyvaluestore
```

</div>

</div>

A Tenant configuration to lookup which DataSource need to use to connect to Tenant database.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f0d4c537-fd08-4497-935e-87d4f72e3dc6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
SET tenant1 tenant1db=luzkeyvaluestore=DATABASE=luz_keyvaluestore-ds1
SET tenant12 tenant127db=luzkeyvaluestore=DATABASE=luz_keyvaluestore-ds1
SET tenant23 tenant23db=luzkeyvaluestore=DATABASE=luz_keyvaluestore-ds1
```

</div>

</div>

In this configuration tenant1 is on tenant1db on luzkeuvauestore schema in on a PostgreSQL cluster DataSource luz_keyvalyestore-ds1,

A Tenant configuration to lookup which DataSource need to use to connect to Tenant database.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="dc8ffaef-47ad-4c35-bccb-1d24ffbc6db8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
set tenant9 luzkeyvaluestore=tenant9=SCHEMA=luz_keyvaluestore
```

</div>

</div>

In this configuration tenant9 is on luzkeuvauestore database to s_tenant9 schema in on a PostgreSQL cluster DataSource luz_keyvalyestore.

### MicroProfile Config Tenant lookup configurations.

A MicroProfile Config file (microprofile-config.properties) can be also used to keep some common configuration.

Example of microprofile-config.properties

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c14e49c1-383a-490b-acdd-d522b95e01f5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
cloudninja.strategy.default=schema
cloudninja.strategy.schema.name=luzkeyvaluestore
cloudninja.strategy.schema.ds=luz_keyvaluestore
cloudninja.strategy.schema.ds.url="10.110.56.149=6432=luzkeyvaluestore=luzkeyvaluestore=luzkeyvaluestore"

cloudninja.strategy.database.ds=luz_keyvaluestore-ds1
cloudninja.strategy.database.ds.url="10.110.56.149=6432=postgres=postgres=postgres"
```

</div>

</div>

We can assume the default strategy is “Schema” and a MicroProfile configuration can help to lookup a database configuration to use in Flyway migration script execution.

If we can add a new property in `LuzTenant` for define a strategy and some identifier of PostgreSQL cluster. it would help to lookup of database configuration without any external tenant strategy lookup services.

## Connection pooler

TBD: How is the connection pooler installed and configured?

PGBouncer can be installed using a Kubernetes deployment.

Example of Kubernetes ConfigMap

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="539a415c-93a2-4b20-8058-fe2b79741033" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: ConfigMap
metadata:
  name: pgbouncer-ini-config
  namespace: cloudninja
data:
  pgbouncer.ini: |
    [databases]
    person = host=postgresql-person dbname=person
    personcommon = host=postgresql-person-common dbname=personcommon
    * = host=postgres-service auth_user=postgres

    [pgbouncer]
    pool_mode = session
    listen_port = 6432
    listen_addr = 0.0.0.0
    auth_type = md5
    auth_file = /etc/pgbouncer/users.txt
    admin_users = pgbouncer
    auth_query = SELECT usename, passwd FROM pg_shadow WHERE usename=$1
    ignore_startup_parameters = extra_float_digits
    max_client_conn = 5000
    default_pool_size = 10
    server_login_retry = 2

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: pgbouncer-user-config
  namespace: cloudninja
data:
  user.txt: |
    "postgres" "md53175bce1d3201d16594cebf9d7eb3f9d"
    "pgbouncer" "md5be5544d3807b54dd0637f2439ecb03b9"
    "person" "md5b1a7c49bedc60364748486c1495aaecd"
    "personcommon" "md5c5d14be7fb7b533588a3a41f8b12e401"
```

</div>

</div>

Example of Kubernetes deployment

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b6f2a08c-ecd7-4475-ba42-41535838dd92" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pgbouncer
  namespace: cloudninja
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pgbouncer
  template:
    metadata:
      labels:
        app: pgbouncer
    spec:
      containers:
        - name: pgbouncer
          image: luz-pgbouncer:1.20.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 6432
          volumeMounts:
            - name: pgbouncer-ini-config
              mountPath: /etc/pgbouncer/pgbouncer.ini
              subPath: pgbouncer.ini
              readOnly: true
            - name: pgbouncer-user-config
              mountPath: /etc/pgbouncer/users.txt
              subPath: user.txt
          env:
            - name: PGBouncer_VERBOSE
              value: "0"
      volumes:
        - name: pgbouncer-ini-config
          configMap:
            name: pgbouncer-ini-config
        - name: pgbouncer-user-config
          configMap:
            name: pgbouncer-user-config
```

</div>

</div>

Example of Kubernetes Service

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="86da844b-4a38-4f8e-90fd-55b6a69bc2d3" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
apiVersion: v1
kind: Service
metadata:
  name: pgbouncer-service
  namespace: cloudninja
spec:
  selector:
    app: pgbouncer
  ports:
    - port: 6432
      targetPort: 6432
```

</div>

</div>

## Database clusters

TBD: Re-configuration of existing one and new ones.

## Backend modules

TBD: Assuming we’re creating a new module, what needs to be considered?

# Migration

## Backend modules

TBD: How are the backend modules migrated? What code changes are needed? What configuration changes are needed?

## Databases

TBD: How are the existing databases migrated, so that they are ready for the new approach?

## Tenant

TBD: How is a tenant migrated, i.e. how is the tenant data migrated for a database from the old to the new approach?

%% ai-graph-start %%

**Related notes:**
- [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]]
- [[System Architecture & Overview - Training Guide]]
- [[Benchmark of luz-database (performance env)]]
- [[Luz performance env cluster topology]]
- [[Data Migration Report Postgres → MongoDB]]

%% ai-graph-end %%