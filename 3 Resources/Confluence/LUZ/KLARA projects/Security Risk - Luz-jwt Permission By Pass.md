---
title: "Security Risk: Luz-jwt Permission By Pass"
created: 2026-02-11
updated: 2026-02-11
type: source
status: reference
source: "Confluence · LUZ - LUZ"
url: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49140039714/Security+Risk+Luz-jwt+Permission+By+Pass
confluence_id: "49140039714"
confluence_path: "LUZ Home > KLARA projects"
tags: [confluence, security]
---

# Security Risk: Luz-jwt Permission By Pass

*Confluence source · LUZ Home › KLARA projects · [view original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49140039714/Security+Risk+Luz-jwt+Permission+By+Pass) · updated 2026-02-11*

### Code Overview

Code link: [https://bitbucket.org/axonivy-prod/luz_jwt/src/master/src/main/java/ch/klara/luz/jwt/filter/TokenAuthenticationRequestFilter.java](https://bitbucket.org/axonivy-prod/luz_jwt/src/master/src/main/java/ch/klara/luz/jwt/filter/TokenAuthenticationRequestFilter.java)

```
private void checkPemissionIn(JsonWebToken token, ContainerRequestContext requestContext) {
    String pathTenantId = (String)requestContext.getUriInfo().getPathParameters().getFirst("tenant-id");
    if (pathTenantId != null && token.getClaim("tenantId") != null && !pathTenantId.equals(token.getClaim("tenantId"))) {
        requestContext.abortWith(Response.status(Status.UNAUTHORIZED).build());
    }
}
```

------------------------------------------------------------------------

### Null Path Parameter Can Bypass - Severe

**Scenario:**

- The variable "pathTenantId" could be null due to the name of parameter is mismatch.

- There are many case can cause the variable "pathTenantId" null such as the key of tenant is mismatch (tenant id name key is different from “tenant-id“)

**Risk:**

- Any endpoint not using `{tenant-id}` in path will bypass this check entirely.

### Inequivalent HTTP Error Code - Medium

**Scenario:**

- Currently, mismatch tenant will return HTTP 401 which indicates that a request was not successful because it lacks valid authentication credentials for the requested resource.

- However, it should be HTTP 403 which mean request failure is tied to application logic, such as insufficient permissions to a resource or action.

**Risk:**

- Infinite login loops — Clients interpret 401 as "token expired, re-authenticate." The user gets trapped refreshing tokens or re-logging in endlessly, because the real problem is permission, not identity.

- Token refresh storms — If you have an interceptor that auto-refreshes on 401, it will retry repeatedly, hammering your auth server.

- Confusing UX — User thinks their session expired when actually they just don't have access.
