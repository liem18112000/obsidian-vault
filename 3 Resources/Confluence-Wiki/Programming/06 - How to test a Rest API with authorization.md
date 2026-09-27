---
ai_hash: f7e554bbc2dfc000
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 2
depth: 3
entities: []
relevance: 0.886
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134411647/06+-+How+to+test+a+Rest+API+with+authorization
space: GRAVITY
status: reference
tags:
- confluence
- programming
- space/gravity
title: 06 - How to test a Rest API with authorization
topic: programming
type: source
updated: 2022-06-24
---

# 06 - How to test a Rest API with authorization

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134411647/06+-+How+to+test+a+Rest+API+with+authorization)
> Relevance 0.886 · topic `programming`

### 1. Introduce

This topic will be cover how can you authorize the credential of a request in the unit test, how can you get a credential from Keyloak in the unit test.

### 2. Example

API need to testing

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="54a21321-8f1f-4959-9d54-8a9f91901f2f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package com.axonfintech.fi.creditrating.service.api;

public class CreditRatingRestApi implements CreditRatingsApi {

    private static final Logger LOG = LoggerFactory.getLogger(CreditRatingRestApi.class);
    private ICreditRatingService creditRatingService;

    /**
     * Use this constructor for inject instances
     *
     * @param creditRatingService the credit rating check service
     */
    @Inject
    public CreditRatingRestApi(ICreditRatingService creditRatingService) {
        this.creditRatingService = creditRatingService;
    }

    @Override
    @Authenticated
    public Response creditRating(@NotNull @Size(max = 64) String tenantId, @Valid @NotNull Party party) {
        try {
            InlineResponse201 inlineResponse = creditRatingService.getInlineResponse201(tenantId, party);
            LOG.info("TenantId: {},  POST credit rating execute SUCCESSFUL. Log for credit rating check service. {}", tenantId, inlineResponse.toString());
            return ResponseBuilder.buildResponseCreatedOK(inlineResponse);
        } catch (CreditRatingServiceException e) {
            LOG.error("TenantId: {}, POST credit rating execute FAILED with an error: {}. Log for credit rating check service.", tenantId, e);
            return ResponseBuilder.buildResponseException(e);
        }
    }
}
```

</div>

</div>

#### 2.1 Mock parameter inject inside constructor in Quarkus framework

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="85f730be-4eb4-40d5-bff3-4aa68ba8e71c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 @Inject
    public CreditRatingRestApi(ICreditRatingService creditRatingService) {
        this.creditRatingService = creditRatingService;
    }
```

</div>

</div>

We have to follow::

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b1a6b3f1-eb10-4992-a57e-b6abdf048cc5" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private @io.quarkus.test.junit.mockito.InjectMock ICreditRatingService creditRaingServiceMock;
```

</div>

</div>

#### 2.2 Mock Authorization

– First of all: we have to files below into src/test/resources

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="13476d6e-836c-4820-99e6-488c9f715d7d" macro-name="view-file"><a href="../_attachments/47134411647-test-users.properties" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47134411647/test-users.properties?version=1&amp;modificationDate=1656070593784&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47134411647-test-users.properties]]

</a></span><span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="2eeb830e-fe1e-494e-9a52-22fb456ed534" macro-name="view-file"><a href="../_attachments/47134411647-test-roles.properties" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47134411647/test-roles.properties?version=1&amp;modificationDate=1656070593696&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/octet-stream" data-has-thumbnail="true">

![[47134411647-test-roles.properties]]

</a></span>

– Second: you need to add the configuration of the Keyloak support test to application.properties under src/main/resources folder.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="edfc3fd1-27cf-417c-abff-b8b7548db962" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
%test.quarkus.security.users.file.enabled=true
%test.quarkus.security.users.file.users=test-users.properties
%test.quarkus.security.users.file.roles=test-roles.properties
%test.quarkus.security.users.file.realm-name=MyRealm
%test.quarkus.security.users.file.plain-text=true

#OIDC
%test.quarkus.oidc.enabled=false
%test.fintech.security.auth-server-url=notdefine
%test.fintech.security.client-id=fintech
%test.fintech.security.token-issuer=notdefine
%test.quarkus.http.test-port=0
```

</div>

</div>

In case if you want to debug in as uni test, may you need to add the configuration below to application.properties. Because the timeout of an API test is so short, the application will stop right the way after you have caught breakpoint.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2b206a5c-1858-405e-9f7a-322b1784a13f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
%test.quarkus.http.test-timeout=300000s
```

</div>

</div>

– Third: require dependency support import test-user and test-role file into application.properties file

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="18d43b7c-a940-4e41-b08a-f22c133f81b1" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// support security with .properties file
    implementation 'io.quarkus:quarkus-elytron-security-properties-file'
```

</div>

</div>

– Four: add `AccountTest ` class in to test package

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ff00257e-18aa-45cd-8c13-2e1984784f2f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre

package com.axonfintech.fi.creditrating.service;

public enum AccountTest {
    ANONYMOUS("anonymous"),
    BUSINESS_CUSTOMER("businessCustomer"),
    BUSINESS_USER("businessUser"),
    BUSINESS_ADMIN("businessAdmin"),
    APPLICATION_ADMIN("applicationAdmin"),
    UN_PERMISSION_USER("unPermissionUser");

    private String userName;
    private String password = "1234";

    /**
     * Instantiates a new account test.
     *
     * @param userName the user name
     * @param password the password
     */
    AccountTest(String userName, String password) {
        this.userName = userName;
        this.password = password;
    }

    /**
     * Instantiates a new account test.
     *
     * @param userName the user name
     */
    AccountTest(String userName) {
        this.userName = userName;
    }

    /**
     * Gets the user name.
     *
     * @return the user name
     */
    public String getUserName() {
        return userName;
    }

    /**
     * Sets the user name.
     *
     * @param userName the new user name
     */
    public void setUserName(String userName) {
        this.userName = userName;
    }

    /**
     * Gets the password.
     *
     * @return the password
     */
    public String getPassword() {
        return password;
    }

    /**
     * Sets the password.
     *
     * @param password the new password
     */
    public void setPassword(String password) {
        this.password = password;
    }
}
```

</div>

</div>

– Finally, this is our class, we use basic authentication `.auth().basic(AccountTest.ANONYMOUS.getUserName(), AccountTest.ANONYMOUS.getPassword())`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e7bb6e82-281f-4205-8a58-a36837f4f39a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@QuarkusTest
@TestHTTPEndpoint(CreditRatingRestApi.class)
public class CreditRatingRestApiTest {

    private static final String TENANT_ID = "fintech";
    private static final String TENANT_ID_HEADER_NAME = "tenantId";

    private @InjectMock ICreditRatingService creditRaingServiceMock;

    /**
     * test for case request successfully
     *
     * @throws CreditRatingServiceException if can not handle request
     */
    @Test
    public void creditRating_whenCreditRatingCheckExisting_shouldReturn201() throws CreditRatingServiceException {
        Party party = DataMock.buildParty();
        InlineResponse201 reponse = new InlineResponse201();
        reponse.setCreditScore(new CreditScore());
        reponse.setParty(party);

        Mockito.when(creditRaingServiceMock.getInlineResponse201(TENANT_ID, party))
            .thenReturn(reponse);

        given()
            .contentType(ContentType.JSON)
            .headers(TENANT_ID_HEADER_NAME, TENANT_ID)
            .auth().basic(AccountTest.ANONYMOUS.getUserName(), AccountTest.ANONYMOUS.getPassword())
            .body(party)
        .when()
            .post()
        .then()
            .statusCode(Status.CREATED.getStatusCode())
            .body("creditScore", Matchers.is(CoreMatchers.notNullValue()))
            .body("party", Matchers.is(CoreMatchers.notNullValue()));
            
    }
  }
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[How to authenticate with Keycloak (SSO authentication) in adapter]]
- [[RESTful API, Postman,]]
- [[Tenant token issue]]
- [[Microprofile OpenAPI config]]
- [[Test Keycloak - Public API]]

%% ai-graph-end %%