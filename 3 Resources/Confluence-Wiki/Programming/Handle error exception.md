---
ai_hash: 8d1bc47629a18114
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 6
depth: 3
entities: []
relevance: 0.837
source: https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20930966439/Handle+error+exception
space: LUZFIN
status: reference
tags:
- confluence
- programming
- space/luzfin
title: Handle error exception
topic: programming
type: source
updated: 2016-08-19
---

# Handle error exception

> [!info] Imported from Confluence
> Space **LUZFIN** · updated 2016-08-19 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZFIN/pages/20930966439/Handle+error+exception)
> Relevance 0.837 · topic `programming`

There are some business rules we want to handle in business layer. 

# **In luz_common**

First of all, ** **we introduce class **LocalizedExceptionMapper, SystemExceptionMapper, LocalizedException**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d4bf2a36-3a59-4528-ac6e-8527bafab2f2" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**LocalizeException**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
package com.axonivy.common.exception;
public class LocalizedException extends Exception {
    private static final long serialVersionUID = 1L;
    private String code;
    private String resouceBundlePath;
    private Object[] params;
    
    public String getLocalizedMessage(Locale preferredLanguage) {
        return ErrorMessageBundle
                .byPath(this.resouceBundlePath)
                .inClassPath(this.getClass().getClassLoader())
                .build()
                .get(code, preferredLanguage, params);
    }
    
    public LocalizedException(String code, String resourceBundlePath, Object... params) {
        this.code = code;
        this.resouceBundlePath = resourceBundlePath;
        this.params = params;
    }
    //... getter and setter
}
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="511d428a-3933-4fd6-aa9b-afd363171b4e" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**LocalizedExceptionMapper**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Provider
public class LocalizedExceptionMapper implements ExceptionMapper<LocalizedException> {
    @Inject
    private HttpServletRequest httpServletRequest;
    @Override
    public Response toResponse(LocalizedException localizedException) {
        
        Map<String, Object> map = new HashMap<String, Object>();
        
        String language = httpServletRequest.getLocale().getLanguage();
        String codeMessage = localizedException.getCode();
        String message = localizedException.getLocalizedMessage(language);
        
        map.put("code", codeMessage);
        map.put("createdTime", new SimpleDateFormat("dd.MM.yyyy H:m:s").format(new Date()));
        map.put("detail", message);
        
        return Response.status(Response.Status.BAD_REQUEST).type(MediaType.APPLICATION_JSON).entity(map).build();
        
    }
}
```

</div>

</div>

This is business error, it mean we know exactly where it come from and we check and throw exception.

 

**In SystemExceptionMapper**

<div hasbody="true" macro-id="432b6d17-1182-487e-8b18-176f34039a33" macro-name="warning">

SystemException

<span class="aui-icon aui-icon-small aui-iconfont-error confluence-information-macro-icon"> </span>

<div>

If I put this mapper in luz_common it catch all exception and it will effect Smuft Team,  
so that currently I will handle system mapper in luzfin_finance first and then if verything is ok, I will move this code to luz_common and synronize with Smuft to have the same way handle exception.

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="6665e634-f9d1-41b8-b5ab-899b26804e2d" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**SystemExceptionMapper**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
@Provider
public class SystemExceptionMapper implements ExceptionMapper<Exception> {

    @Override
    public Response toResponse(Exception exception) {
        
        Map<String, Object> map = new HashMap<String, Object>();
        UUID uuid = UUID.randomUUID();
        
        map.put("createdTime", new SimpleDateFormat("dd.MM.yyyy H:m:s").format(new Date()));
        map.put("detail","UUID: " + uuid + " \n" + exception.getMessage());
        
        Logger.getLogger(SystemExceptionMapper.class.getName()).log(Level.SEVERE, "UUID: " + uuid + exception.getMessage(), exception);
        
        return Response.serverError().type(MediaType.APPLICATION_JSON).entity(map).build();
    }
}
```

</div>

</div>

This is technical error, it mean we don't know when it happen, such as server die suddenly, connection to database fail or system error.

 

**ErrorMessageBundle class**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="44c56713-f055-441e-a411-0e3335a600dd" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**BundleResouce**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class ErrorMessageBundle {
    public static class Builder {
        private String bundlePath;
        private Optional<ClassLoader> classLoader = Optional.empty();
        public Builder(String bundlePath) {
            this.bundlePath = bundlePath;
        }
        public Builder inClassPath(ClassLoader classLoader) {
            this.classLoader = Optional.of(classLoader);
            return this;
        }
        public ErrorMessageBundle build() {
            return new ErrorMessageBundle(this.bundlePath, classLoader.orElse(Thread.currentThread().getContextClassLoader()));
        }
    }
    
    private static final String EMPTY_MESSAGE = "";
    private String resouceBundlePath;
    private ClassLoader classLoader;
    public static Builder byPath(String bundlePath) {
        return new Builder(bundlePath);
    }
    
    private ErrorMessageBundle(String resouceBundlePath, ClassLoader classLoader) {
        this.resouceBundlePath = resouceBundlePath;
        this.classLoader = classLoader;
    }
    public String get(String key, Locale preferredLanguage, Object... params) {
        try {
            Locale languageToUse = Optional.ofNullable(preferredLanguage).orElse(Locale.ENGLISH);
            ResourceBundle bundle = ResourceBundle.getBundle(this.resouceBundlePath, languageToUse, this.classLoader);
            if(bundle.containsKey(key)) {
                MessageFormat message = new MessageFormat(bundle.getString(key));
                String result = message.format(params);
                return result.replaceAll("\\{[0-9]+\\}", "");
            } 
            return EMPTY_MESSAGE;
        } catch (Exception slientAllErrors) {
            Logger.getLogger(this.getClass().getName()).log(Level.SEVERE, slientAllErrors.getMessage(),
                    slientAllErrors);
            return EMPTY_MESSAGE;
        }
    }
}
```

</div>

</div>

 

# **In module luzfin_finance**

<div hasbody="true" macro-id="47a0ae0d-d861-407b-a1de-4c9cd8e9060e" macro-name="warning">

<span class="aui-icon aui-icon-small aui-iconfont-error confluence-information-macro-icon"> </span>

<div>

Other module want to handle error exception should follow luzfin_finance module

</div>

</div>

I will introduce OrderManageException and extend LocalizedException like bellow, 

I overwrite resouce bundle path when we want to handle exeption,   
we also have resouce bundble file in luzfin_finance.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="61b4525d-defa-4f78-b617-cccf84d0d8d8" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**OrderManagementException**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class OrderManagementException extends LocalizedException {
    
    private static final long serialVersionUID = 1L;
    private static final String ORDER_MANAGEMENT_RESOURCE_BUNDLE_PATH= "messages/order_management/error_message";
    
    public OrderManagementException(String code) {
        super(code, ORDER_MANAGEMENT_RESOURCE_BUNDLE_PATH);
    }
    //nothing else
}
```

</div>

</div>

 

**Define resouce bundle in luzfin_finance**

 


![[20930966439-image2016-8-11 10-40-12.png]]



**Usage in luzfin_finance**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e353976c-3eb5-47fb-993c-ee55b58efe37" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**Use in luzfin_finance**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
if(offer == null) {
    throw new OrderManagementException("offer.notfound");
}
```

</div>

</div>

 

**Result respone**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="578aff54-6f93-4bcb-967f-f46b0326aef9" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
{
    "code": "offer.notfound",
    "createdTime": "13.02.2016 8:30:10",
    "detail": "Could not find this offer in system."
}
```

</div>

</div>

- **code**: Define code from resouce bundle  
- **createdTime**: Time error happen.  
- **detail**: Message detail about error. If error comes from business exception we always have resouce bundle for multi language otherwise we get message from exception by method **e.getMessage()**;

 

**And the file bundle resource like following.**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aeafd18e-95fc-4a9e-a390-2372776787d6" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**error_message_en.properties**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
offer.notfound = Could not found this offer in system.
offer.accept.comment.require = Comment must be not null when accept an offer.
offer.accept.invalid = This offer could not accepted. In order to accept an offer, this offer is latest in order and status is SENT.
```

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Throw error codes not sentences; localise at the request boundary]]
- [[Ivy conventions]]
- [[Scan to book - booking exception]]
- [[Enumerate a dependency's exception surface and decide each one before integrating]]
- [[Error handling for delete and undo]]

%% ai-graph-end %%