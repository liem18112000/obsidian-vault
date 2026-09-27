---
title: "Mutually exclusive API parameters should be rejected, not resolved by precedence"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Research Design architecture concept for Generic Interface File (HACKA)"
tags: [api-design, rest, validation, jax-rs, confluence-distilled]
---

# Mutually exclusive API parameters should be rejected, not resolved by precedence

When an endpoint accepts two inputs that identify the same thing two different ways, the contract is **exclusive or**, not "send whichever you like". Make that explicit and reject the both-present case rather than silently picking a winner.

The example: a generic-interface-file endpoint takes either a `salaryRunId` **or** a list of `payslipIds` — never both.

```
GET /{company-tenant-id}/companies/{company-id}/accounting-interfaces/generic-interface-document
```

```java
public Response generateGenericInterfaceFile(@BeanParam GenericInterfaceFileRequest request) { … }

public class GenericInterfaceFileRequest {
    @HeaderParam("Accept-Language") @DefaultValue("DE")
    private String language;

    @Parameter(description = "Tenant id", required = true)
    @PathParam("company-tenant-id")
    private String tenantId;
    // salaryRunId XOR payslipIds — validated, not assumed
}
```

**Why rejecting beats precedence.** The tempting shortcut is a documented precedence rule ("if both are supplied, `salaryRunId` wins"). That converts a caller bug into a *silent* wrong answer: the client believes it filtered to three payslips, gets the whole salary run, and nothing in the response says so. A `400` costs the caller one failed request and tells them exactly what they got wrong. Precedence rules also tend to be discovered by reading the implementation, not the docs.

**Two supporting habits from the same endpoint:**

- **`@BeanParam` to group parameters.** Instead of a method signature with seven `@PathParam`/`@QueryParam`/`@HeaderParam` arguments, bind them into one request object. The signature stays readable, the validation lives with the data, and the XOR check becomes a method on the request rather than the first ten lines of the resource method.
- **Default the locale rather than requiring it.** `@HeaderParam("Accept-Language") @DefaultValue("DE")` uses the standard HTTP mechanism and keeps the parameter optional — no bespoke `?lang=` query param, no null checks downstream.

> [!tip] Generalise it
> Any time a request object has two optional fields where exactly one must be present, that is a sum type being smuggled through a product type. The language may not let you express it directly, but validation at the boundary restores the guarantee — and it belongs at the boundary, because everything downstream then gets to assume it.

Source: [[Research Design architecture concept for the service to generate the Generic Interface File]] (HACKA, Confluence).
