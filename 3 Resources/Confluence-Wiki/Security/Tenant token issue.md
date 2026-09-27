---
title: "Tenant token issue"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47192343098/Tenant+token+issue
space: "TS"
topic: security
relevance: 0.75
depth: 3
updated: 2022-10-04
attachments: 0
tags:
  - confluence
  - security
  - space/ts
---

# Tenant token issue

> [!info] Imported from Confluence
> Space **TS** · updated 2022-10-04 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47192343098/Tenant+token+issue)
> Relevance 0.75 · topic `security`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="64ccd33e-3776-45ad-9df9-14bc11d91020" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
select c.*
from companyinfo c
left join tenant t 
    on c.tenantid = t.tenantid 
where t."type" is null
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="23def46b-027f-4673-9917-1a5208f90bde" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// https://bitbucket.org/axonivy-prod/jwt_service/src/master/src/main/java/com/axonivy/sec/token/jwt/JWTGenerator.java#lines-154

@Override
public TokenGenerator addCompanyTenant(CompanyTenant t) {
    this.claimSetBuilder.claim(t.getType().getName(), t);
    this.claimSetBuilder.claim(TokenConstants.CLAIM_TENANT_ID, t.getTenantId());
    return this;
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="20d1d360-2693-475d-95ba-294bc82d2322" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
2022-10-04 13:35:31.522 ICT

luztenant-service
08:35:31,522 INFO [io.undertow.accesslog] (default task-30) 10.124.12.204 [04/Oct/2022:08:35:31 +0200] luz-uri=GET /luztenant/api/a61a6043-84f7-4067-9cce-9e42379a5743/company-info HTTP/1.1 status-code=200 bytes-sent=100 time-consuming=6
2022-10-04 13:35:31.565 ICT

jwt-service
08:35:31,564 ERROR [io.undertow.request] (default task-37) UT005023: Exception handling request to /luzsec/api/a61a6043-84f7-4067-9cce-9e42379a5743/access/tokens: org.jboss.resteasy.spi.UnhandledException: javax.ejb.EJBException: java.lang.NullPointerException
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ExceptionHandler.handleApplicationException(ExceptionHandler.java:82)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ExceptionHandler.handleException(ExceptionHandler.java:346)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.writeException(SynchronousDispatcher.java:193)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:457)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.lambda$invoke$4(SynchronousDispatcher.java:229)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.lambda$preprocess$0(SynchronousDispatcher.java:135)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.interception.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:358)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.preprocess(SynchronousDispatcher.java:138)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:215)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.plugins.server.servlet.ServletContainerDispatcher.service(ServletContainerDispatcher.java:245)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher.service(HttpServletDispatcher.java:61)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher.service(HttpServletDispatcher.java:56)
    at javax.servlet.api@2.0.0.Final//javax.servlet.http.HttpServlet.service(HttpServlet.java:590)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletHandler.handleRequest(ServletHandler.java:74)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:129)
    at io.opentracing.contrib.opentracing-jaxrs2//io.opentracing.contrib.jaxrs2.server.SpanFinishingFilter.doFilter(SpanFinishingFilter.java:52)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.core.ManagedFilter.doFilter(ManagedFilter.java:61)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.FilterHandler$FilterChainImpl.doFilter(FilterHandler.java:131)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.FilterHandler.handleRequest(FilterHandler.java:84)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.security.ServletSecurityRoleHandler.handleRequest(ServletSecurityRoleHandler.java:62)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletChain$1.handleRequest(ServletChain.java:68)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletDispatchingHandler.handleRequest(ServletDispatchingHandler.java:36)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.security.SecurityContextAssociationHandler.handleRequest(SecurityContextAssociationHandler.java:78)
    at io.undertow.core@2.2.2.Final//io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.RedirectDirHandler.handleRequest(RedirectDirHandler.java:68)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.security.SSLInformationAssociationHandler.handleRequest(SSLInformationAssociationHandler.java:132)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.security.ServletAuthenticationCallHandler.handleRequest(ServletAuthenticationCallHandler.java:57)
    at io.undertow.core@2.2.2.Final//io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.core@2.2.2.Final//io.undertow.security.handlers.AbstractConfidentialityHandler.handleRequest(AbstractConfidentialityHandler.java:46)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.security.ServletConfidentialityConstraintHandler.handleRequest(ServletConfidentialityConstraintHandler.java:64)
    at io.undertow.core@2.2.2.Final//io.undertow.security.handlers.AuthenticationMechanismsHandler.handleRequest(AuthenticationMechanismsHandler.java:60)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.security.CachedAuthenticatedSessionHandler.handleRequest(CachedAuthenticatedSessionHandler.java:77)
    at io.undertow.core@2.2.2.Final//io.undertow.security.handlers.NotificationReceiverHandler.handleRequest(NotificationReceiverHandler.java:50)
    at io.undertow.core@2.2.2.Final//io.undertow.security.handlers.AbstractSecurityContextAssociationHandler.handleRequest(AbstractSecurityContextAssociationHandler.java:43)
    at io.undertow.core@2.2.2.Final//io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.security.jacc.JACCContextIdHandler.handleRequest(JACCContextIdHandler.java:61)
    at io.undertow.core@2.2.2.Final//io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.deployment.GlobalRequestControllerHandler.handleRequest(GlobalRequestControllerHandler.java:68)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.SendErrorPageHandler.handleRequest(SendErrorPageHandler.java:52)
    at io.undertow.core@2.2.2.Final//io.undertow.server.handlers.PredicateHandler.handleRequest(PredicateHandler.java:43)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler.handleFirstRequest(ServletInitialHandler.java:269)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler.access$100(ServletInitialHandler.java:78)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:133)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler$2.call(ServletInitialHandler.java:130)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.core.ServletRequestContextThreadSetupAction$1.call(ServletRequestContextThreadSetupAction.java:48)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.core.ContextClassLoaderSetupAction$1.call(ContextClassLoaderSetupAction.java:43)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.security.SecurityContextThreadSetupAction.lambda$create$0(SecurityContextThreadSetupAction.java:105)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.deployment.UndertowDeploymentInfoService$UndertowThreadSetupAction.lambda$create$0(UndertowDeploymentInfoService.java:1530)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.deployment.UndertowDeploymentInfoService$UndertowThreadSetupAction.lambda$create$0(UndertowDeploymentInfoService.java:1530)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.deployment.UndertowDeploymentInfoService$UndertowThreadSetupAction.lambda$create$0(UndertowDeploymentInfoService.java:1530)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.deployment.UndertowDeploymentInfoService$UndertowThreadSetupAction.lambda$create$0(UndertowDeploymentInfoService.java:1530)
    at org.wildfly.extension.undertow@21.0.2.Final//org.wildfly.extension.undertow.deployment.UndertowDeploymentInfoService$UndertowThreadSetupAction.lambda$create$0(UndertowDeploymentInfoService.java:1530)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler.dispatchRequest(ServletInitialHandler.java:249)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler.access$000(ServletInitialHandler.java:78)
    at io.undertow.servlet@2.2.2.Final//io.undertow.servlet.handlers.ServletInitialHandler$1.handleRequest(ServletInitialHandler.java:99)
    at io.undertow.core@2.2.2.Final//io.undertow.server.Connectors.executeRootHandler(Connectors.java:387)
    at io.undertow.core@2.2.2.Final//io.undertow.server.HttpServerExchange$1.run(HttpServerExchange.java:841)
    at org.jboss.threads@2.4.0.Final//org.jboss.threads.ContextClassLoaderSavingRunnable.run(ContextClassLoaderSavingRunnable.java:35)
    at org.jboss.threads@2.4.0.Final//org.jboss.threads.EnhancedQueueExecutor.safeRun(EnhancedQueueExecutor.java:1990)
    at org.jboss.threads@2.4.0.Final//org.jboss.threads.EnhancedQueueExecutor$ThreadBody.doRunTask(EnhancedQueueExecutor.java:1486)
    at org.jboss.threads@2.4.0.Final//org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1377)
    at org.jboss.xnio@3.8.2.Final//org.xnio.XnioWorker$WorkerThreadFactory$1$1.run(XnioWorker.java:1280)
    at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: javax.ejb.EJBException: java.lang.NullPointerException
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.tx.CMTTxInterceptor.invokeInOurTx(CMTTxInterceptor.java:266)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.tx.CMTTxInterceptor.required(CMTTxInterceptor.java:388)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.tx.CMTTxInterceptor.processInvocation(CMTTxInterceptor.java:158)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:509)
    at org.jboss.weld.core@3.1.5.Final//org.jboss.weld.module.ejb.AbstractEJBRequestScopeActivationInterceptor.aroundInvoke(AbstractEJBRequestScopeActivationInterceptor.java:72)
    at org.jboss.as.weld.common@21.0.2.Final//org.jboss.as.weld.ejb.EjbRequestScopeActivationInterceptor.processInvocation(EjbRequestScopeActivationInterceptor.java:89)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.interceptors.CurrentInvocationContextInterceptor.processInvocation(CurrentInvocationContextInterceptor.java:41)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.invocationmetrics.WaitTimeInterceptor.processInvocation(WaitTimeInterceptor.java:47)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.security.SecurityContextInterceptor.processInvocation(SecurityContextInterceptor.java:100)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.deployment.processors.StartupAwaitInterceptor.processInvocation(StartupAwaitInterceptor.java:22)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.interceptors.ShutDownInterceptorFactory$1.processInvocation(ShutDownInterceptorFactory.java:64)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.interceptors.LoggingInterceptor.processInvocation(LoggingInterceptor.java:67)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.NamespaceContextInterceptor.processInvocation(NamespaceContextInterceptor.java:50)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.ContextClassLoaderInterceptor.processInvocation(ContextClassLoaderInterceptor.java:60)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.run(InterceptorContext.java:438)
    at org.wildfly.security.elytron-private@1.13.1.Final//org.wildfly.security.manager.WildFlySecurityManager.doChecked(WildFlySecurityManager.java:628)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.AccessCheckingInterceptor.processInvocation(AccessCheckingInterceptor.java:57)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:53)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.ViewService$View.invoke(ViewService.java:198)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.ViewDescription$1.processInvocation(ViewDescription.java:191)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.ProxyInvocationHandler.invoke(ProxyInvocationHandler.java:81)
    at deployment.jwt_service.war//com.axonivy.sec.token.service.TokenService$$$view9.generateTokenForAccessingTenant(Unknown Source)
    at jdk.internal.reflect.GeneratedMethodAccessor118.invoke(Unknown Source)
    at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.base/java.lang.reflect.Method.invoke(Method.java:566)
    at org.jboss.weld.core@3.1.5.Final//org.jboss.weld.util.reflection.Reflections.invokeAndUnwrap(Reflections.java:410)
    at org.jboss.weld.core@3.1.5.Final//org.jboss.weld.module.ejb.EnterpriseBeanProxyMethodHandler.invoke(EnterpriseBeanProxyMethodHandler.java:134)
    at org.jboss.weld.core@3.1.5.Final//org.jboss.weld.bean.proxy.EnterpriseTargetBeanInstance.invoke(EnterpriseTargetBeanInstance.java:56)
    at org.jboss.weld.core@3.1.5.Final//org.jboss.weld.module.ejb.InjectionPointPropagatingEnterpriseTargetBeanInstance.invoke(InjectionPointPropagatingEnterpriseTargetBeanInstance.java:68)
    at org.jboss.weld.core@3.1.5.Final//org.jboss.weld.bean.proxy.ProxyMethodHandler.invoke(ProxyMethodHandler.java:106)
    at deployment.jwt_service.war//com.axonivy.sec.token.service.TokenService$Proxy$_$$_Weld$EnterpriseProxy$.generateTokenForAccessingTenant(Unknown Source)
    at deployment.jwt_service.war//com.axonivy.sec.rest.TokenResource.createTokenForAccessingTenant(TokenResource.java:144)
    at deployment.jwt_service.war//com.axonivy.sec.rest.TokenResource$Proxy$_$$_WeldClientProxy.createTokenForAccessingTenant(Unknown Source)
    at jdk.internal.reflect.GeneratedMethodAccessor131.invoke(Unknown Source)
    at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.base/java.lang.reflect.Method.invoke(Method.java:566)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.MethodInjectorImpl.invoke(MethodInjectorImpl.java:138)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ResourceMethodInvoker.internalInvokeOnTarget(ResourceMethodInvoker.java:543)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTargetAfterFilter(ResourceMethodInvoker.java:432)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ResourceMethodInvoker.lambda$invokeOnTarget$0(ResourceMethodInvoker.java:393)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.interception.PreMatchContainerRequestContext.filter(PreMatchContainerRequestContext.java:358)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ResourceMethodInvoker.invokeOnTarget(ResourceMethodInvoker.java:395)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:364)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.ResourceMethodInvoker.invoke(ResourceMethodInvoker.java:337)
    at org.jboss.resteasy.resteasy-jaxrs@3.13.2.Final//org.jboss.resteasy.core.SynchronousDispatcher.invoke(SynchronousDispatcher.java:440)
    ... 59 more
Caused by: java.lang.NullPointerException
    at deployment.jwt_service.war//com.axonivy.sec.token.jwt.JWTGenerator.addCompanyTenant(JWTGenerator.java:154)
    at deployment.jwt_service.war//com.axonivy.sec.token.service.TokenService.generateTokenForAccessingTenant(TokenService.java:159)
    at jdk.internal.reflect.GeneratedMethodAccessor118.invoke(Unknown Source)
    at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.base/java.lang.reflect.Method.invoke(Method.java:566)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.ManagedReferenceMethodInterceptor.processInvocation(ManagedReferenceMethodInterceptor.java:52)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:509)
    at org.jboss.as.weld.common@21.0.2.Final//org.jboss.as.weld.interceptors.Jsr299BindingsInterceptor.delegateInterception(Jsr299BindingsInterceptor.java:79)
    at org.jboss.as.weld.common@21.0.2.Final//org.jboss.as.weld.interceptors.Jsr299BindingsInterceptor.doMethodInterception(Jsr299BindingsInterceptor.java:89)
    at org.jboss.as.weld.common@21.0.2.Final//org.jboss.as.weld.interceptors.Jsr299BindingsInterceptor.processInvocation(Jsr299BindingsInterceptor.java:102)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:63)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.invocationmetrics.ExecutionTimeInterceptor.processInvocation(ExecutionTimeInterceptor.java:43)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.jpa@21.0.2.Final//org.jboss.as.jpa.interceptor.SBInvocationInterceptor.processInvocation(SBInvocationInterceptor.java:47)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.concurrent.ConcurrentContextInterceptor.processInvocation(ConcurrentContextInterceptor.java:45)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InitialInterceptor.processInvocation(InitialInterceptor.java:40)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:53)
    at org.jboss.as.ee@21.0.2.Final//org.jboss.as.ee.component.interceptors.ComponentDispatcherInterceptor.processInvocation(ComponentDispatcherInterceptor.java:52)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.interceptors.NonPooledEJBComponentInstanceAssociatingInterceptor.processInvocation(NonPooledEJBComponentInstanceAssociatingInterceptor.java:59)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.component.interceptors.AdditionalSetupInterceptor.processInvocation(AdditionalSetupInterceptor.java:54)
    at org.jboss.invocation@1.6.0.Final//org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:422)
    at org.jboss.as.ejb3@21.0.2.Final//org.jboss.as.ejb3.tx.CMTTxInterceptor.invokeInOurTx(CMTTxInterceptor.java:252)
    ... 114 more
```

</div>

</div>
