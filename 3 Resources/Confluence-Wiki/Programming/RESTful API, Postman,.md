---
title: "RESTful API, Postman, ..."
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47079916390/RESTful+API+Postman+...
space: "TS"
topic: programming
relevance: 0.854
depth: 3
updated: 2022-03-30
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# RESTful API, Postman, ...

> [!info] Imported from Confluence
> Space **TS** · updated 2022-03-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47079916390/RESTful+API+Postman+...)
> Relevance 0.854 · topic `programming`

<a href="http://localhost:8180/luz_store/api/customers?company-uri=%2Fluz_compensation%2Fapi%2F44dcd4d0-5c1a-4f3c-9b09-07abd454a1f0%2Fcompanies%2F1" class="external-link" rel="nofollow">http://localhost:8180/luz_store/api/customers?company-uri=%2Fluz_compensation%2Fapi%2F44dcd4d0-5c1a-4f3c-9b09-07abd454a1f0%2Fcompanies%2F1</a>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c96da5c3-d62d-4c7d-a7d4-636c7d89a3e1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Path("/customers")
@Tag(name = "customers")
@RequestScoped
@Transactional
@Produces(MediaType.APPLICATION_JSON)
@AccessibleWithoutTenant
@SecurityRequirement(name = "bearerAuth")
public class CustomerResource {
    
    @Inject
    private CustomerService customerService;
    
    @GET
    @Operation(summary = "Get customers")
    @APIResponses(value = {
            @APIResponse(responseCode = "200", description = Constants.MSG_OK,
                    content = @Content(mediaType = MediaType.APPLICATION_JSON,
                    schema = @Schema(type = SchemaType.ARRAY, implementation = Customer.class))) })
    public Response finds(
            @Parameter(description = "company uri", required = true) @QueryParam("company-uri") String companyUri) {
        return Response.ok(Arrays.asList(customerService.findCustomerByCompanyUri(companyUri))).build();
    }
}
```

</div>

</div>

http://localhost:8080/luzfin_finance/api/**00a04daf-f2b3-41d5-8c12-2d1b4c48a36a**/companies/1/customers/**getUseCreditCard/luzfin_finance/api/00a04daf-f2b3-41d5-8c12-2d1b4c48a36a/companies/1/customers/712**

http://localhost:8082/luzfin_finance/api/642247b0-7e78-4a92-9f2a-74b727684732/companies/1/customers/customer-uri?company-uri=642247b0-7e78-4a92-9f2a-74b727684732/companies/2
