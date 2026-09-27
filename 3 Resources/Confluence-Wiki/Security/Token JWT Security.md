---
title: "Token JWT Security"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436029691/Token+JWT+Security
space: "LUZ"
topic: security
relevance: 0.773
depth: 2.77
updated: 2021-01-13
attachments: 5
tags:
  - confluence
  - security
  - space/luz
---

# Token JWT Security

> [!info] Imported from Confluence
> Space **LUZ** · updated 2021-01-13 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20436029691/Token+JWT+Security)
> Relevance 0.773 · topic `security`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="7408dd18-ae8d-4b9d-b623-a9432fd3bd9a" macro-name="toc">

</div>

# Introduction

Securing the different rest resources is provided by a new API based on JWT tokens and Tenant management. Basically this can be summarized as follows:


![[20436029691-Summarized Token and Tenant Flow.png]]



  


![[20436029691-Klara web login flow.png]]



  

# The Token Service

The token service application is responsible for delivering a token for a user. The source code is available on Bitbucket at <a href="https://bitbucket.org/axonivy-prod/jwt_service" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/jwt_service</a> .

Two kind of JWT Tokens can be obtained: a generic token and a "full" token.

- The basic token holds basic user information like the subject (Ivy username) and can be used for calling resources annotated with @AccessibleWithoutTenant that do not need tenant specific information.

- The "full" token holds tenant specific information, like the user person-tenant-id and the company-tenant-id. You need such a token for calling resources working with tenant specific information (company, insurances, ...).

You can find further information about JWT there: <a href="https://jwt.io/" class="external-link" rel="nofollow">https://jwt.io/</a>

Basically the token is produced in JSON format, is encoded (Base64) and signed using a private key. The Token Service delivers the public key (base64 encoded) that allows verifying the token and getting all the information hold by it.

The Token Service token REST resource is secured by Basic Auth. You need to provide an Ivy username and its password. 

Example of a successful token service response:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eebd9c66-b66d-4cd1-b5c2-39cfd7c4826a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
  "token": "eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJ0ZXN0IiwiaXNzIjoiY29tLmF4b25pdnkiLCJleHAiOjE4NjI2MjY1Mjk3LCJpYXQiOjE4NjI2MjYzNDk3fQ.j7cukSWR0uuLmZZkqX1mpmnE1Bzbht3AjihG5fI4ucATI0jToUa27T5lFbqQd7RsCYB7ugO1ynszVeK0wYxrjR_MM1sjYJg-9Bll8M12nEE4z4KWbPqhcY1LKIiixdlWHW4cLQC78jxZh71287SoxtjckbufJbHpW1NPrmn_xeA",
  "publicKey": "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC1RhcrZKDNjjq+hmVll/X3RI+qx7fWbx0GRrzSOl5YhTYDZ9rFor3UyS+7wQv5aRPcEqu/5uLJyj+mxA96z+XpqwzLaWdGEWATIysqzwWbD3NwTVcziIMPt9+tuZ7mBMO5JRhDBaqIadwJPz/WgN0/8o7FPqz4XQR/L6vOUC7DewIDAQAB"
}
```

</div>

</div>

The Token service resource endpoint are: 

- Generic Token: **POST** <a href="http://localhost:8080/luzsec/api/tokens" class="external-link" rel="nofollow">http://[host]:[port]/luzsec/api/tokens</a> and Basic Auth with Ivy username and password 
- Full token: **POST** <a href="http://localhost:8080/luzsec/api/d31c34bf-7c19-4919-bb7e-80c8c980483c/tokens" class="external-link" rel="nofollow">http://localhost:8080/luzsec/api/{company-tenant-id}/tokens</a> and Basic Auth with Ivy username and password 

There is also a REST resource for getting the public key separately: **GET** <a href="http://localhost:8080/luzsec/api/tokens" class="external-link" rel="nofollow">http://[host]:[port]/luzsec/api/public-keys</a>/active (no Basic Auth)

The Tenant id can be get from the tenant resource with a basic token. The tenant resource is described bellow. 

# The Tenant Service

### GET tenant

The actual tenant Endpoint is: **GET** <a href="http://localhost:8080/luzadmin/api/clients/test/tenants" class="external-link" rel="nofollow">http://</a><a href="http://localhost:8080/luzsec/api/tokens" class="external-link" rel="nofollow">[host]:[port]</a><a href="http://localhost:8080/luzadmin/api/clients/test/tenants" class="external-link" rel="nofollow">/luztenant/api/{ivy-username}/tenants</a> with Header Authorization Bearer generic JWT token

It produces application/json and returns the list of the tenants corresponding to the given @PathParam Ivy username.

Example of a response:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4d9ffebe-b702-4258-b7fd-8be3b5dc2c78" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
[
  {
    "type": "person-tenant",
    "id": 62,
    "tenantId": "9513ffd5-8c6c-43fe-ba2b-814d741951b1",
    "roles": [
      "tenant-creator"
    ],
    "username": "test"
  },
  {
    "type": "company-tenant",
    "id": 63,
    "tenantId": "d31c34bf-7c19-4919-bb7e-80c8c980483c",
    "roles": [
      "tenant-creator"
    ],
    "username": "test"
  }
]
```

</div>

</div>

### POST tenant (for creating a new tenant)

If the GET Tenant resource does not find any tenant for the given ivy username, this means that the user is login for the very first time. So the tenant has to be created. For that the POST tenant service has to be called.

The endpoint is: POST <a href="http://localhost:8080/luzadmin/api/tenants" class="external-link" rel="nofollow">http://</a><a href="http://localhost:8080/luzsec/api/tokens" class="external-link" rel="nofollow">[host]:[port]</a><a href="http://localhost:8080/luzadmin/api/tenants" class="external-link" rel="nofollow">/luztenant/api</a><a href="http://localhost:8080/luzadmin/api/clients/test/tenants" class="external-link" rel="nofollow">/{ivy-username}</a><a href="http://localhost:8080/luzadmin/api/tenants" class="external-link" rel="nofollow">/tenants</a> with Header Authorization Bearer generic JWT token.

We don't detail what happens here. Please read the \[MULTITENANCY SECTION\]

# How to use the JWT Token based security in your REST resources

Dependency to set:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="08c8d8ac-9c5d-4e82-973e-6b01b06e409d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
        <dependency>
            <groupId>com.axonivy.sec</groupId>
            <artifactId>luzsec_service</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
```

</div>

</div>

There are 3 levels of security:

- No security with the @PermitAll annotation at class or method level
- Security with basic token where the tenant information is not needed. With the basic token you cannot call tenant specific resources where the tenant id is set as Path Parameter. Here you have to annotate your class or method with @AccessibleWithoutTenant
- Tenant secure resources: if not annotation is present, you must provide the full jwt as Authorization Bearer header.

Example:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c70e9742-4aa9-4999-859e-e2ed2d460ad0" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl hide-border-bottom">

<span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> </span><span class="expand-control-text">Expand source</span></span><span class="collapse-spinner-wrapper"> </span>

</div>

<div class="codeContent panelContent pdl hide-toolbar">

``` syntaxhighlighter-pre
package com.axonivy.sec.rest.hello;
import com.axonivy.sec.CurrentSession;
import com.axonivy.sec.authorization.AccessibleWithoutTenant;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import com.axonivy.sec.rest.RestResourceConstants;
import com.axonivy.sec.token.Token;
import javax.inject.Inject;
import javax.annotation.security.PermitAll;

@Path("hello")
@Produces({MediaType.APPLICATION_JSON})
public class HelloWorldResource {
    
    @Inject
    @CurrentSession
    Token token;
    
    /**
     * This REST method can be accessed without any security.
     */
    @GET
    @Path("withname/{name}")
    @PermitAll
    public HelloResponse sayHelloToGivenName(@PathParam("name") String name) {
        return makeResponse(name);
    }
    
    /**
     * This REST method needs a generic token. It can be of course called with a full token.
     * You call this resource by setting your request Authorization Header with "Bearer jwt" (Bearer eyJhbGciOiJSUzU....)
     */
    @GET
    @Path("user")
    @AccessibleWithoutTenant
    public HelloResponse sayHelloToUser() {
        return makeResponse(token.getSubject());
    }
    
    /**
     * This REST method needs a full token. The token contains all the tenant info.
     * You call this resource by setting your request Authorization Header with "Bearer jwt" (Bearer eyJhbGciOiJSUzU....)
     */
    @GET
    @Path("tenant")
    public HelloResponse sayHelloToTenants() {
        return makeResponse("person-tenant " + token.getPersonTenant().getTenantId() + " and company-tenant " + token.getCompanyTenant().getTenantId());
    }
    
    /**
     * This REST method needs a full token AND the company tenant contained in the token must be the same as the one specified in the url. 
     * You call this resource by setting your request Authorization Header with "Bearer jwt" (Bearer eyJhbGciOiJSUzU....)
     */
    @GET
    @Path("company/{company-tenant-id}")
    public HelloResponse sayHelloToCompanyTenant() {
        return makeResponse("company-tenant " + token.getCompanyTenant().getTenantId());
    }
    
    /**
     * This REST method needs a full token AND the person tenant contained in the token must be the same as the one specified in the url. 
     * You call this resource by setting your request Authorization Header with "Bearer jwt" (Bearer eyJhbGciOiJSUzU....)
     */
    @GET
    @Path("person/{person-tenant-id}")
    public HelloResponse sayHelloToPersonTenant() {
        return makeResponse("person-tenant " + token.getPersonTenant().getTenantId());
    }
    
    
    private HelloResponse makeResponse(String name) {
        HelloResponse resp = new HelloResponse();
        resp.setIssued(new SimpleDateFormat("dd.MM.yyyy HH:mm:ss").format(new Date(System.currentTimeMillis())));
        resp.setMessage("Hello " + name);
        resp.setInfo(token);
        return resp;
    }
}
```

</div>

</div>

Note in the above example that you can inject the token Object deserialized from the jwt token with:

    @Inject
    @CurrentSession
    Token token;

# Key store

Until the jwt_service version 0.0.18 the keys were stored in memory. Each time the jwt_service was deployed or the server was restarted, this KeyPair was generated again. This was fine for the normal AccessToken, as they were generated at login time. Now the Refresh-tokens must be valid for a long time and the system must be able to validate them even if they were generated before some server-restarts or new deployments. So we had to change the way the KeyPair is kept on the system.

Now the KeyPair is stored into 2 files: <a href="http://jwt_public_key.pk" class="external-link" rel="nofollow">jwt_public_key.pk</a> and <a href="http://jwt_private_key.pk" class="external-link" rel="nofollow">jwt_private_key.pk</a>. The place where they are stored is defined by a new System property:

  
**System property name for setting the directory path where the keys are stored**

<div>

|                                           |
|-------------------------------------------|
| `com_axonivy_luz_jwt_keys_directory_path` |

</div>

  

The User running the Wildfly JVM must be able to create this directory structure if it does not exists, and must be able to create, read and write files there.

If you don't provide any RSA public/private key files, the system will generate them automatically only once.

If you provide these key, they should be respectively named <a href="http://jwt_public_key.pk" class="external-link" rel="nofollow">jwt_public_key.pk</a> and <a href="http://jwt_private_key.pk" class="external-link" rel="nofollow">jwt_private_key.pk</a>, they should be RSA 2048 bits long.

# What should we do when we want to add more user role

### Situation:

We would like to add new user role. With user who is belong to this role just can access to some APIs

### Problem:

Our implementation for security:


![[20436029691-wrong tooltip.png]]



<span class="legacy-color-text-default">It means there is nothing related to user role for security. </span>

<span class="legacy-color-text-default"><span class="legacy-color-text-default">If we have an user which is assigned to a tenant, this user can access to all KLARA functionalities of this tenant without caring about which role he/she is belong.</span></span>

### What can we do(to be discussed):

We have to make all our APIs to be protected by user roles also.

**How:**

<span class="legacy-color-text-default">There is an annotation <span class="legacy-color-text-default">@TokenRolesAllowed</span> which we can specify which user roles are allowed for which APIs.  
</span>

Now we have to add this annotation for APIs which need to be protected by user roles.

Because there are a lots of APIs until now, so it will take time to do and test. And we also need support from other teams too.
