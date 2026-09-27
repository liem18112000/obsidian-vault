---
title: "Coding conventions for KLARA Java EE modules"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436752441/Coding+conventions+for+KLARA+Java+EE+modules
space: "LUZ"
topic: programming
relevance: 0.804
depth: 2.84
updated: 2016-12-19
attachments: 0
tags:
  - confluence
  - programming
  - space/luz
---

# Coding conventions for KLARA Java EE modules

> [!info] Imported from Confluence
> Space **LUZ** · updated 2016-12-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436752441/Coding+conventions+for+KLARA+Java+EE+modules)
> Relevance 0.804 · topic `programming`

<span class="status-macro aui-lozenge aui-lozenge-current conf-macro output-inline">IN-PROGRESS</span>

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2" macro-id="91ad0088-0fe7-4ffe-9d2a-26f0efb7bf8e" macro-name="toc">

</div>

# Conventions for Rest Resources Classes

## Use `@RequestScoped` Scope 

### JAX-RS Spec

According to <a href="http://download.oracle.com/otndocs/jcp/jaxrs-2_0-fr-eval-spec/index.html" class="external-link" rel="nofollow">JSR-399 JAX-RS 2.0 Spec</a>, §3.1.1 and §3.1.2, root-resource classes are bound to a request.

 

> By default a new resource class instance is created for each request to that resource.

> Providers and Application subclasses MUST be singletons or use application scope.

 

 

If a root-resource is deployed within a Environment supporting CDI or EJB, the JAX-RS implementation will also have to support them. See  §10.2.3 and  §10.2.4

### Jersey & RESTEasy

- Jersey by default maintains request scope for root-resource classes (unless specified differently) according to its <a href="https://jersey.java.net/documentation/latest/jaxrs-resources.html#d0e2643" class="external-link" rel="nofollow">official documentation</a>. Since Jersey (and the JAX-RS spec) does not depends on CDI, a custom annotation `@org.glassfish.jersey.process.internal.RequestScoped`<span class="Apple-tab-span">` `</span> is introduced. It is equivalent to CDI's `@RequestScoped`.

- RESTEasy does the same as described in <a href="https://docs.jboss.org/resteasy/docs/3.1.0.Final/userguide/html_single/#d4e2232" class="external-link" rel="nofollow">Default Scopes</a> :  
    

  > If a JAX-RS root resource does not define a scope explicitly, it is bound to the Request scope.  
  > If a JAX-RS Provider or <a href="http://javax.ws/" class="external-link" rel="nofollow">javax.ws</a>.rs.Application subclass does not define a scope explicitly, it is bound to the Application scope.

   RESTEasy also warns:  
    

  > **Warning**
  >
  > Since the scope of all beans that do not declare a scope is modified by resteasy-cdi, this affects session beans as well. **As a result, a conflict occurs if the scope of a stateless session bean or singleton is changed automatically as the spec prohibits these components to be @RequestScoped. Therefore, you need to explicitly define a scope when using stateless session beans or singletons.** This requirement is likely to be removed in future releases.

   

### Conclusion

It's a good practice to annotate all our Rest Resource Classes with `@RequestScoped` *explicitly* for clarification and avoiding confusion implied in integration between JAX-RS implementations and other parts of Java EE (e.g CDI, EJB, etc).

## Use of `@Transactional`

Here the definition of a transaction: <a href="http://docs.oracle.com/javaee/6/tutorial/doc/bncii.html" class="external-link" rel="nofollow">http://docs.oracle.com/javaee/6/tutorial/doc/bncii.html</a>

Theoretically there is no need for having our resources annotated with `@Transactional`. But you need it in the case the JAX-RS Resource class is part of a transaction, for example if it uses directly Services classes which in turn are implied in some transactions.

Example in pseudo Code:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3609d78f-17d4-4240-9f58-c35e7ba311c8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@RequestScoped
@Transactional
@Path("myPath")
public class myResource {
 
    //The services Classes are each @Transactional
    @Inject
    PersonService personService;
 
    @Inject
    CompanyService companyService;
 
    @POST
    public Response addStuff(Stuff stuffToAdd) {
        // personService involved in the first transaction part
        personService.add(stuffToAdd.getPerson());
 
        // companyService involved in the second transaction part
        companyService.add(stuffToAdd.getCompany());
 
        ....
    }
 
}
```

</div>

</div>

In this example, if the companyService fails, we need to be `@Transactional` if we wants that the person part is rolled-back automatically.

### Conclusion

It's a good practice to annotate all our Rest Resource Classes with `@Transactional`*.*

# EJB Session Beans
