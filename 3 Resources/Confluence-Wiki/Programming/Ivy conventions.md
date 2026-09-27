---
title: "Ivy conventions"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474824144/Ivy+conventions
space: "LUZ"
topic: programming
relevance: 0.738
depth: 2.65
updated: 2018-11-12
attachments: 11
tags:
  - confluence
  - programming
  - space/luz
---

# Ivy conventions

> [!info] Imported from Confluence
> Space **LUZ** · updated 2018-11-12 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20474824144/Ivy+conventions)
> Relevance 0.738 · topic `programming`

1.  **Don't write java code in processes of ivy.**
2.  **Follow code convention of java  **
    <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="86d00588-46a7-4491-9683-e68f090be7e0" macro-name="view-file"><a href="../_attachments/20474824144-Sun_JAVA_Coding_Conventions.pdf" class="confluence-embedded-file" data-nice-type="PDF Document" data-file-src="/wiki/download/attachments/20474824144/Sun_JAVA_Coding_Conventions.pdf?version=1&amp;modificationDate=1538555367000&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/pdf" data-has-thumbnail="true">

![[20474824144-Sun_JAVA_Coding_Conventions.pdf]]

</a></span>  
      
3.  **HTML Dialogs**
    1.  **Page  **
        Should try catch every script plate in start method, we can't show error message  if one of these scripts plate got an error because this time the page don't has context(<span class="legacy-color-text-gray4">*FacesContext.getCurrentInstance()*</span>).  
          
    2.  **Component  **
        -Should not write business code or call Rest API in ivy script, the start method of component will run when the page is rendering whatever the component is rendered or not.  
        -Use the jsf event to init data , reference: <a href="https://docs.oracle.com/javaee/6/javaserverfaces/2.1/docs/vdldocs/facelets/f/event.html" class="external-link" rel="nofollow">https://docs.oracle.com/javaee/6/javaserverfaces/2.1/docs/vdldocs/facelets/f/event.html</a>, **the advantage :this event will not run if the component wasn't rendered.  
          **
4.  **Error Handling  **
    Currently Klara web often got oops screen sometimes, PO want to prevent oops screen, after we search how to catch ivy runtime exception because if we can catch exception on processes, pages and components we can prevent the oops screen appear.  
    Now Klara apply [Process Routing](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20434521389/Process+Routing), ex : *<span class="legacy-color-text-default">ProcessStarterHelper.start(Pages.DASHBOARD), routing to dashboard, if the routing has error  and throw ProcessStartException then the ErrorHandling process will be catch.</span>*  
      
    We already app change address process on luz_xhrm_process  
    1.  **Pages,Components  
          **
        We have to catch exception on every script plate, use ivy <span class="legacy-color-text-gray3"><span class="legacy-color-text-gray3"><span class="resultofText">Error</span> Start Event  (<a href="http://127.0.0.1:56932/help/topic/ch.ivyteam.ivy.designer.help/html/DesignerGuide/index.html" class="external-link" rel="nofollow">Axon.ivy 7.1 Designer Guide</a> \> <a href="http://127.0.0.1:56932/help/topic/ch.ivyteam.ivy.designer.help/html/DesignerGuide/ivy.processmodeling.html" class="external-link" rel="nofollow">Process Modeling</a> \> <a href="http://127.0.0.1:56932/help/topic/ch.ivyteam.ivy.designer.help/html/DesignerGuide/ivy.processmodels.elements.html" class="external-link" rel="nofollow">Process Elements Reference</a>), <span class="legacy-color-text-default">if error code is empty then it will catch every script plate on component  
        </span></span></span>

          

        <span class="legacy-color-text-gray3"><span class="legacy-color-text-default">  
        

![[20474824144-Screenshot_1.png]]

  
        

![[20474824144-Screenshot_2.png]]

  
          
        import ch.klara.luz.components.error.handling.ExceptionMapperHandling;  
        ExceptionMapperHandling.showErrorMessage(error);  
          
        

![[20474824144-error_3.png]]

  
          
        </span></span><span class="legacy-color-text-gray3"><span class="legacy-color-text-default">  
          
        </span></span>

    2.  <span class="legacy-color-text-gray3"><span class="legacy-color-text-default"><span class="legacy-color-text-default">**Error handling process** : Catch exception on all processes throw, then show the general error page.</span>  
          
        

![[20474824144-error_global.png]]

![[20474824144-error_global_page.png]]

  
          
        **Hint**<span class="legacy-color-text-default"> :</span></span></span>

<span class="legacy-color-text-gray3"><span class="legacy-color-text-default"><span class="legacy-color-text-default"> </span>

![[20474824144-error_handling_process.png]]

</span></span>

<span class="legacy-color-text-gray3"><span class="legacy-color-text-default"><span class="legacy-color-text-default">  
</span></span></span>

<span class="legacy-color-text-gray3"><span class="legacy-color-text-default"><span class="legacy-color-text-default">The process name and path should be like picture, i try to change name or path then it didn't work any more, this process only effect on one project, Wow team also apply for </span>**change address process **<span class="legacy-color-text-default">(luz_component, luz_xhrm_process , </span><a href="https://bitbucket.org/axonivy-prod/luz_components/pull-requests/1319/handle-exception/diff" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_components/pull-requests/1319/handle-exception/diff</a><span class="legacy-color-text-default">, </span><a href="https://bitbucket.org/axonivy-prod/luz_xhrm_processes/pull-requests/537/luz-17520-show-oops-screen/diff" class="external-link" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_xhrm_processes/pull-requests/537/luz-17520-show-oops-screen/diff</a><span class="legacy-color-text-default">)</span>  
  
**Disadvantage : **<span class="legacy-color-text-default">Every process, page and component have to put(copy) the same catch exception process,we try to find other way to use java class to catch all exception on ivy, but it didn't success.</span>  
</span></span>
