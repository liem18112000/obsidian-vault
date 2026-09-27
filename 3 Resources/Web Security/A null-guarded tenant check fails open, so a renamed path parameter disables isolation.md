---
title: "A null-guarded tenant check fails open, so a renamed path parameter disables isolation"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Security Risk - Luz-jwt Permission By Pass (2026-02-11)"
tags: [security, multi-tenancy, authorization, jwt, fail-open, luz-jwt, gotcha]
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
