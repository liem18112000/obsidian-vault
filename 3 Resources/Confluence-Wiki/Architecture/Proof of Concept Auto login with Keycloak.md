---
ai_hash: 1f2ee3e85d73b1ac
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 16
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/46958576246/Proof+of+Concept+Auto+login+with+Keycloak
space: TP2020
status: reference
tags:
- confluence
- architecture
- space/tp2020
title: '[Proof of Concept] Auto login with Keycloak'
topic: architecture
type: source
updated: 2021-09-27
---

# [Proof of Concept] Auto login with Keycloak

> [!info] Imported from Confluence
> Space **TP2020** · updated 2021-09-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/46958576246/Proof+of+Concept+Auto+login+with+Keycloak)
> Relevance 0.711 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="df09f9f4-bda8-41ae-a18a-ba76935c8194" macro-name="toc">

</div>

There is a case that, when opening also a Klara URL in the myLife app (myLife and Klara have the same authentication/authorization system), a new mobile browser tab is open, and even when user has logged in in the app, he/she still need to login again in the browser with his/her username and password.

We expected that, user will be automatically logged in when opening the Klara URL from the app without entering username and password again.

In this article, we are going to implement an auto login flow in keycloak, using the action token and the username validation from keycloak direct grant authentication flow. The flow only works when the user session is still valid (user is logged in in the app).

**You can find the sample project and play with it here:**

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="5805ee21-fbaf-420e-91a9-ac15b1e1d252" macro-name="view-file"><a href="../_attachments/46958576246-auto-login-poc.zip" class="confluence-embedded-file" data-nice-type="Zip Archive" data-file-src="/wiki/download/attachments/46958576246/auto-login-poc.zip?version=1&amp;modificationDate=1632232176147&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/zip" data-has-thumbnail="true">

![[46958576246-auto-login-poc.zip]]

</a></span>

## I. Configuration<span id="id-[ProofofConcept]AutologinwithKeycloak-configuration" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="706b8cd7ca83c276786623a4d4e242f3" macro-name="anchor"><span id="configuration" class="confluence-anchor-link"> </span></span>

Firstly we need to create a new authentication flow in Keycloak, let’s name it “auto login”.

In the authentication tab, click new


![[46958576246-image-20210921-133756.png]]



In the alias, type “auto login”, click create


![[46958576246-image-20210921-133902.png]]



Add two executions by the “Add execution” button

\- Cookie (Alternative): with this execution, if the user has logged in, the flow will use already stored user cookie and process the authentication flow, skip the below execution

\- Username Validation (Required) : If the cookie is not set, process the authentication flow with the username


![[46958576246-image-20210921-134453.png]]



We don’t have Password validation since we don’t have and don’t know user password during this flow. But we can enhance this by also validate the access token of the user who want to created the auto login flow

## II. Implementation

### 1. Auto login action token, action token handler and action token factory

#### a. Auto login action token

Firstly we need to create a new type of action token with type “auto-login”, it’s also the value of the `typ` in the encoded token. Here is how the token looks like


![[46958576246-image-20210922-012731.png]]



<div id="expander-630360625" class="expand-container conf-macro output-block" hasbody="true" macro-id="e6653de3-b4c2-4026-bd0f-0ace5e01077a" macro-name="expand">

<div id="expander-control-630360625" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AutoLoginActionToken.java </span>

</div>

<div id="expander-content-630360625" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8c5a765f-3b3d-464e-91da-627e2fc00773" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package ch.klara.keycloak.actiontoken.token;
import org.keycloak.authentication.actiontoken.DefaultActionToken;
public class AutoLoginActionToken extends DefaultActionToken {
  private static final long serialVersionUID = -6910022908963614460L;
  public static final String TOKEN_TYPE = "auto-login";
  
  public AutoLoginActionToken(String userId, int absoluteExpirationInSecs,
            String authenticationSessionId) {
        super(userId, TOKEN_TYPE, absoluteExpirationInSecs, null, authenticationSessionId);
  }
  
  @SuppressWarnings("unused")
  private AutoLoginActionToken() {
  }
}
```

</div>

</div>

</div>

</div>

#### b. Auto login action token handler

To handle our new action token, we need to implement a handler for it. There is two important methods in this handler

1\. `handleToken` : will receive our token when performing action, it will get username from the context and pass that user name to process the `auto login flow` which was created in step <a href="#configuration" rel="nofollow">I Configuraion</a>

2\. `startFreshAuthenticationSession`: Creates a fresh authentication session according to the information from the token. The default implementation creates a new authentication session that requests termination after required actions. It also handles the redirection after perform action.

<div id="expander-1218462918" class="expand-container conf-macro output-block" hasbody="true" macro-id="8fddfa51-9558-4642-a811-202409b34f94" macro-name="expand">

<div id="expander-control-1218462918" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AutoLoginActionTokenHandler.java</span>

</div>

<div id="expander-content-1218462918" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3c416a5a-c951-49c5-9c9c-88b2adcb8321" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class AutoLoginActionTokenHandlerextends AbstractActionTokenHander<AutoLoginActionToken> {

private static final String CLIENT_ID = "client_id";
private static final String REDIRECT_URL = "redirect_url";
private static final String AUTO_LOGIN_FLOW_ALIAS = "auto login";

public AutoLoginActionTokenHandler(KeycloakSession session) {
    super(AutoLoginActionToken.TOKEN_TYPE, AutoLoginActionToken.class, "not_allowed", EventType.LOGIN, "invalidCodeMessage");
}

@Override
public Response handleToken(AutoLoginActionToken token,
        ActionTokenContext<AutoLoginActionToken> tokenContext) {
    
    putUserNameToFormParamToProcessUserNameValidationFlow(tokenContext);
    
    AuthenticationFlowModel autoLoginFlow = getAutoLoginFlow(tokenContext);
    
    return tokenContext.processFlow(false, LoginActionsService.AUTHENTICATE_PATH,
            autoLoginFlow, null, new AuthenticationProcessor());
}

@Override
public AuthenticationSessionModel startFreshAuthenticationSession(AutoLoginActionToken token,
        ActionTokenContext<AutoLoginActionToken> tokenContext) {
    
    UriInfo uriInfo = tokenContext.getUriInfo();
    
    AuthenticationSessionModel authenticationSession = 
            tokenContext.createAuthenticationSessionForClient(getClientIdQueryParam(uriInfo));
    
    Optional.ofNullable(getRedirectUrlQueryParam(uriInfo))
            .ifPresent(authenticationSession::setRedirectUri);
    
    return authenticationSession;
}

@Override
public String getAuthenticationSessionIdFromToken(AutoLoginActionToken token,
        ActionTokenContext<AutoLoginActionToken> tokenContext,
        AuthenticationSessionModel authenticationSessionModel) {
    return "";
}

@Override
public Predicate<? super AutoLoginActionToken>[] getVerifiers(
        ActionTokenContext<AutoLoginActionToken> tokenContext) {
    return super.getVerifiers(tokenContext);
}

@Override
public boolean canUseTokenRepeatedly(AutoLoginActionToken token,
        ActionTokenContext<AutoLoginActionToken> tokenContext) {
    return false;
}

private AuthenticationFlowModel getAutoLoginFlow(ActionTokenContext<AutoLoginActionToken> tokenContext) {
    return tokenContext.getRealm().getFlowByAlias(AUTO_LOGIN_FLOW_ALIAS);
}

private String getRedirectUrlQueryParam(UriInfo uriInfo) {
    return uriInfo.getQueryParameters().getFirst(REDIRECT_URL);
}

private String getClientIdQueryParam(UriInfo uriInfo) {
    return uriInfo.getQueryParameters().getFirst(CLIENT_ID);
}

private void putUserNameToFormParamToProcessUserNameValidationFlow(ActionTokenContext<AutoLoginActionToken> tokenContext) {
    String username = tokenContext.getAuthenticationSession()
            .getAuthenticatedUser()
            .getUsername();
    tokenContext.getAuthenticationSession()
    .getAuthenticatedUser()
    .getUsername();
    
    tokenContext.getRequest()
        .getDecodedFormParameters()
        .put(AuthenticationManager.FORM_USERNAME, Arrays.asList(username));
}
```

</div>

</div>

}

</div>

</div>

#### c. Auto login action token handler factory

To know which handler will be used for our new token, we need to implement a factory, override the method `getId` and return our token type (auto login)

  
To let Keycloak scan and know our factory we will create a file in the folder `src/main/resource/META-INF/services/org.keycloak.authentication.actiontoken.ActionTokenHandlerFactory` in the project. It’s content is the path of our custom `AutoLoginActionTokenHandlerFactory ` factory path like this:


![[46958576246-image-20210922-014152.png]]



<div id="expander-1494027150" class="expand-container conf-macro output-block" hasbody="true" macro-id="674f205c-c727-4427-9868-01c360c2046c" macro-name="expand">

<div id="expander-control-1494027150" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AutoLoginActionTokenHandlerFactory.java</span>

</div>

<div id="expander-content-1494027150" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="87850307-0200-49dc-8d43-9dae662ee923" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class AutoLoginActionTokenHandlerFactory implements ActionTokenHandlerFactory<AutoLoginActionToken>{
  @Override
  public ActionTokenHandler<AutoLoginActionToken> create(KeycloakSession session) {
        return new AutoLoginActionTokenHandler(session);
  }
  
  @Override
  public void init(Scope config) {
        
  }
  
  @Override
  public void postInit(KeycloakSessionFactory factory) {
        
  }
  
  @Override
  public void close() {
        
  }
  
  @Override
  public String getId() {
        return AutoLoginActionToken.TOKEN_TYPE;
  }
}
```

</div>

</div>

</div>

</div>

### 2. Auto login resource <span id="id-[ProofofConcept]AutologinwithKeycloak-autoLoginResource" class="confluence-anchor-link conf-macro output-inline" hasbody="false" macro-id="b02ee81248848a74bc89f8db94c2778b" macro-name="anchor"><span id="autoLoginResource" class="confluence-anchor-link"> </span></span>

#### a. Auto login resource

We create new rest extension for the purpose that to create our auto login URL or auto login token

To create the auto login URL, the resource will consume two mandatory parameters client_id, redirect_url which are pass to the resource via request form, and the valid access token in the request header.

The URL can only be create if the client_id is valid, the redirect_url is not empty and the access token is still valid

<div id="expander-1434653591" class="expand-container conf-macro output-block" hasbody="true" macro-id="29028480-0ee6-4e19-bfa5-0e0d43368223" macro-name="expand">

<div id="expander-control-1434653591" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AutoLoginResource.java</span>

</div>

<div id="expander-content-1434653591" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="21328df1-d786-4971-b435-ec2bce110317" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class AutoLoginResource {
  private static final String CLIENT_ID = "client_id";
  private static final String REDIRECT_URL = "redirect_url";
  private final KeycloakSession session;
  
  public AutoLoginResource(KeycloakSession session) {
        this.session = session;
  }
  
  @POST
  @Path("token")
  @NoCache
  @Produces(MediaType.APPLICATION_JSON)
  public Response getAutoLoginToken(@FormParam(CLIENT_ID) String clientId) {
        KeycloakContext context = session.getContext();
        UserInfo userInfo = getUserInfo(context);
        if(clientId==null) {
            return Response.status(Status.BAD_REQUEST)
                    .entity("Client id can not be null")
                    .build();
        }
        
        UserModel user = KeycloakModelUtils.findUserByNameOrEmail(session, context.getRealm(), userInfo.getPreferredUsername());
        
        String autoLoginToken = createAutoLoginToken(context, user, getAuthenticationSession(context, clientId));
        return Response.ok(autoLoginToken).build();
  }
  
  @POST
  @Path("url")
  @NoCache
  @Produces(MediaType.APPLICATION_JSON)
  public Response getAutoLoginUrl(@FormParam(CLIENT_ID) String clientId
            ,@FormParam(REDIRECT_URL) String redirectUrl) {
        
        KeycloakContext context = session.getContext();
        UserInfo userInfo = getUserInfo(context);
        
        if(clientId==null) {
            return Response.status(Status.BAD_REQUEST)
                    .entity("Client id can not be null")
                    .build();
        }
        
        if(redirectUrl==null) {
            return Response.status(Status.BAD_REQUEST)
                    .entity("redirect Url can not be null")
                    .build();
        }
        
        UserModel user = KeycloakModelUtils
                .findUserByNameOrEmail(session, context.getRealm(), userInfo.getPreferredUsername());
        
        AuthenticationSessionModel authSession = getAuthenticationSession(context, clientId);
        
        String autoLoginToken = createAutoLoginToken(context, user, authSession );
        
        UriBuilder builder = Urls.actionTokenBuilder(context.getUri().getBaseUri(), autoLoginToken,
              authSession.getClient().getClientId(), authSession.getTabId());
        
        if(redirectUrl != null) {
            builder.queryParam(REDIRECT_URL, redirectUrl);
        }
        
        String autoLoginUrl = builder.build(context.getRealm().getName()).toString();
        return Response.ok(autoLoginUrl).build();
  }
  
  /**
  * This method is being used for two purposes: <br/>
  * 1. To get the user info by the authorized access token <br/>
  * that was included in the header<br/>
  * 2. To validate if the provided access token valid, throw exception otherwise
  * @param context
  * @return UserInfo
  */
  private UserInfo getUserInfo(KeycloakContext context) {
        OIDCLoginProtocolService oidcLoginProtocolService = new OIDCLoginProtocolService(context.getRealm(), null);
        UserInfoEndpoint userInfoEndpoint = (UserInfoEndpoint)oidcLoginProtocolService.issueUserInfo();
        ObjectMapper objectMapper = new ObjectMapper();
        return objectMapper.convertValue(userInfoEndpoint.issueUserInfoGet(context.getRequestHeaders()).getEntity(), UserInfo.class);
  }
  
  private String createAutoLoginToken(KeycloakContext context, UserModel user, AuthenticationSessionModel authSession) {
        int validityInSecs = context.getRealm().getActionTokenGeneratedByUserLifespan();
      int absoluteExpirationInSecs = Time.currentTime() + validityInSecs;
      
      String authSessionEncodedId = AuthenticationSessionCompoundId.fromAuthSession(authSession).getEncodedId();
      String token = new AutoLoginActionToken(
            user.getId(),
              absoluteExpirationInSecs,
              authSessionEncodedId
            ).serialize(
              session,
              context.getRealm(),
              context.getUri()
            );
        return token;
  }
  
  private AuthenticationSessionModel getAuthenticationSession(KeycloakContext context, String clientId) {
        AuthenticationSessionManager authenticationSessionManager = new AuthenticationSessionManager(session);
        RootAuthenticationSessionModel rootAuthenticationSession = authenticationSessionManager.createAuthenticationSession(context.getRealm(), false);
        
        AuthenticationSessionModel authSession = rootAuthenticationSession.createAuthenticationSession(context.getRealm().getClientByClientId(clientId));
        return authSession;
  }
}
```

</div>

</div>

</div>

</div>

#### b. Auto login resource provider

The provider here to let Keycloak know which resource will be use for the context. In this context its `AutoLoginResource `above

<div id="expander-499980137" class="expand-container conf-macro output-block" hasbody="true" macro-id="3d8d8c09-18dc-43d5-8acc-f916de910d90" macro-name="expand">

<div id="expander-control-499980137" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AutoLoginResourceProvider.java</span>

</div>

<div id="expander-content-499980137" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b50ac94e-2fe3-492e-81ca-064ed5006873" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class AutoLoginResourceProvider implements RealmResourceProvider{
  private KeycloakSession session;
  
  public AutoLoginResourceProvider(KeycloakSession session) {
      this.session = session;
  }
  
  @Override
  public void close() {
        
  }
  
  @Override
  public Object getResource() {
        return new AutoLoginResource(session);
  }
}
```

</div>

</div>

</div>

</div>

#### c. Auto login resource provider factory

\- Like normal, keycloak need to scan and know our handler for any customized stuff, we need a factory for our resource provider.

\- The method `getId` return `"autologin"`, that means, the resource path will be:

**{{keycloakBaseUrl}}/auth/realm/{{realmNames}}/**`autologin`

\- The method `create` simply return our resource provider `AutoLoginResourceProvider`

<div id="expander-568489907" class="expand-container conf-macro output-block" hasbody="true" macro-id="f171955a-6c9c-452c-b8fa-742392aa3531" macro-name="expand">

<div id="expander-control-568489907" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">AutoLoginResourceProviderFactory.java</span>

</div>

<div id="expander-content-568489907" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="45892569-3376-4cc2-a5fd-8f0a7869cc45" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class AutoLoginResourceProviderFactory implements RealmResourceProviderFactory{
  public static final String ID = "autologin";
  
  @Override
  public RealmResourceProvider create(KeycloakSession session) {
        return new AutoLoginResourceProvider(session);
  }
  
  @Override
  public void init(Scope config) {
        
  }
  
  @Override
  public void postInit(KeycloakSessionFactory factory) {
        
  }
  
  @Override
  public void close() {
        
  }
  
  @Override
  public String getId() {
        return ID;
  }
}
```

</div>

</div>

</div>

</div>

And don’t forget to create the file to let Keycloak scan and know our factory: `src/main/resource/META-INF/org.keycloak.services.resource.RealmResourceProviderFactory`, the file content is our `AutoLoginResourceProviderFactory` path


![[46958576246-image-20210922-014641.png]]



## III. Deployment

To deploy the code, we just run maven command: `mvn clean install`, in the target folder of the project, we will have the .jar file, we will copy that file to the providers folder in Keycloak (create the folder if it does not exist).


![[46958576246-image-20210921-135432.png]]



Restart the Keycloak and check our provider in the Keycloak server info.


![[46958576246-image-20210921-135330.png]]

![[46958576246-image-20210922-022352.png]]



## IV. Security

\- User can generate the auto login URL and use it only when he/she has the valid access token (logged in), otherwise exception will be throw trying to generate the URL

\- The auto login URL can only be used one time, it will be expired once user has successfully logged in by that URL.

\- The URL itself also have expiration time  (the action token expiration time) which can be configured in Keycloak.

## V. Usage

Call to auto login resource which was defined in step <a href="#autoLoginResource" rel="nofollow">2. Auto login resource</a> using POST method, the URL format is : **{{keycloakBaseUrl}}/auth/realm/{{realmNames}}/**`autologin/url`

Firstly we need to pass the access token in the authorization


![[46958576246-image-20210922-031323.png]]



Secondly we need client_id and redirect_url in the request form


![[46958576246-image-20210922-031458.png]]



Trigger the call, the response will be the full auto login URL, if the is no exception (token is valid, client_id is not empty, redirect url is not empty)


![[46958576246-image-20210922-031645.png]]



Enter that URL to the browser to process the auto login flow without entering username and password

%% ai-graph-start %%

**Related notes:**
- [[Keycloak action tokens bridge an app session into a browser login]]
- [[Auto login in myLife and ePost private web clients]]
- [[Understanding Keycloak Authorization Code flow]]
- [[Copy Proof of Concept Passwordless account login with Keycloak]]
- [[Proof of Concept Passwordless account login with Keycloak]]

%% ai-graph-end %%