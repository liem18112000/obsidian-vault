---
ai_hash: 36ad073ac7be823b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-05
entities: []
source: session 2026-08-05 luz_docs_import public-keys 500
status: seedling
tags:
- luz-docs-import
- config
- jwt
- luzsec
- gotcha
title: JWT_SERVICE_HOST_PORT must have no trailing slash (system-properties concatenates
  /luzsec/api/)
type: gotcha
---

# JWT_SERVICE_HOST_PORT must have no trailing slash (system-properties concatenates /luzsec/api/)

In **luz_docs_import**, the luzsec public-key base URI is assembled in `deployment/wildfly/config/system-properties.xml`:

```xml
<property name="com_axonivy_luzsec_RestResourceBaseURI" value="${env.JWT_SERVICE_HOST_PORT}/luzsec/api/"/>
```

Because it concatenates `${JWT_SERVICE_HOST_PORT}` directly with `/luzsec/api/`, the env var **must not** end with a slash. A trailing slash yields a double slash: `http://host:8080` + `/` + `/luzsec/api/` → `http://host:8080//luzsec/api/public-keys/active`.

## When it bit
`docker-compose.yml` had `JWT_SERVICE_HOST_PORT: 'http://host.docker.internal:8080/'` (trailing slash) producing `//luzsec` in the request-filter log. The `Dockerfile` value `http://jwt-service:8080` is already correct (no slash). Fix: drop the trailing slash in docker-compose.

## Contrast with luz_docs
The sibling **luz_docs** does NOT concatenate onto `JWT_SERVICE_HOST_PORT`; it uses dedicated full-path config keys — `JWT_SERVICE_KEY/mp-rest/url=.../luzsec/api/` (with a `JwtServiceClient` `@Path("/public-keys/active")`) and `MP_JWT_VERIFY_PUBLIC_KEY_LOCATION=.../luzsec/api/public-keys/key`. So luz_docs keeping a trailing slash on `JWT_SERVICE_HOST_PORT` is harmless there — the two services build the same URL by different means.

Related: [[Luz local run: host.docker.internal:8080 must be the dev api-forwarder, not another cluster on 8080]]

## Related

- [[Luz local run: host.docker.internal:8080 must be the dev api-forwarder]]
- [[not another cluster on 8080]]

%% ai-graph-start %%

**Related notes:**
- [[Luz local run host.docker.internal8080 must be the dev api-forwarder, not another cluster on 8080]]
- [[Truncating a JWT breaks signature verification and surfaces as 500 not 401]]
- [[Run luz_docs_statistic locally with docker-compose]]
- [[jwt-service token path synchronously calls luztenant security-classes]]
- [[jwt-service token endpoints and replicas (Luz prod)]]

%% ai-graph-end %%