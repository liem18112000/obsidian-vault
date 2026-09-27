---
ai_hash: 990d74b14bf1e4ab
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Security Risk - Luz-jwt Permission By Pass (2026-02-11)'
status: seedling
tags:
- security
- multi-tenancy
- authorization
- jwt
- fail-open
- luz-jwt
- gotcha
title: A null-guarded tenant check fails open, so a renamed path parameter disables
  isolation
type: lesson
---

# A null-guarded tenant check fails open, so a renamed path parameter disables isolation

The `luz_jwt` request filter enforced tenant isolation like this:

```java
String pathTenantId = requestContext.getUriInfo().getPathParameters().getFirst("tenant-id");
if (pathTenantId != null && token.getClaim("tenantId") != null
        && !pathTenantId.equals(token.getClaim("tenantId"))) {
    requestContext.abortWith(Response.status(Status.UNAUTHORIZED).build());
}
```

Read the condition as an attacker would: **the check only runs when both values are present.** So the guard is skipped entirely whenever

- the endpoint's path has no `{tenant-id}` segment at all, or
- the path parameter is spelled differently (`tenantId`, `tenant_id`) and `getFirst("tenant-id")` returns `null`, or
- the token simply carries no `tenantId` claim.

Every one of those is a **silent pass**, and the last two are *typos*. A single renamed path parameter turns a cross-tenant guard off with no compile error, no test failure, and no log line.

The shape of the bug is the lesson: **an authorization check written as "reject if mismatch" fails open; it must be written as "reject unless proven match".**

```java
if (pathTenantId == null || tokenTenantId == null || !pathTenantId.equals(tokenTenantId)) {
    abort(FORBIDDEN);
}
```

And the structural fix behind the code fix: a deny-by-default filter that requires every route to *declare* its tenant binding, so a route that forgets is rejected rather than exempted. Absence of evidence must not be evidence of permission.

Same filter, second defect: [[Returning 401 for a permission failure causes infinite login loops]].

## Related

- [[Returning 401 for a permission failure causes infinite login loops]]

## Related

- [[Returning 401 for a permission failure causes infinite login loops]]

%% ai-graph-start %%

**Related notes:**
- [[Returning 401 for a permission failure causes infinite login loops]]
- [[Security Risk - Luz-jwt Permission By Pass]]
- [[Security Risk - Luz-jwt Permission By Pass]]
- [[Tenant token issue]]
- [[Luz 403 Not allowed means the token has no tenant claim, not a missing permission]]

%% ai-graph-end %%