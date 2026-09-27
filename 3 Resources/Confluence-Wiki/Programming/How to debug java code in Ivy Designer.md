---
title: "How to debug java code in Ivy Designer"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/PT/pages/25287337918/How+to+debug+java+code+in+Ivy+Designer
space: "PT"
topic: programming
relevance: 0.89
depth: 3
updated: 2017-04-24
attachments: 7
tags:
  - confluence
  - programming
  - space/pt
---

# How to debug java code in Ivy Designer

> [!info] Imported from Confluence
> Space **PT** · updated 2017-04-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/PT/pages/25287337918/How+to+debug+java+code+in+Ivy+Designer)
> Relevance 0.89 · topic `programming`

1.  **Requirement**  
    When issues come from code of java class and you want debug it to find rootcause.

2.  **Solution**

**        First of All If you very new with Ivy Designer, Please check *"***Xpert.ivy Designer.ini***" to make sure it has configured to active remote debugging in Ivy Designer. If didn't configured yet, just open it and add this line at the end of this file***

***        

![[25287337918-edit ivy designer.png]]

***

<div class="preformatted panel conf-macro output-block" hasbody="true" macro-id="111c88a6-f197-4fcf-836d-eba59d68c45a" macro-name="noformat" style="border-width: 1px;">

<div class="preformattedContent panelContent">

    -agentlib:jdwp=transport=dt_socket,server=y,address=8001,suspend=n

</div>

</div>

        **Step#1:** Create remote debug

        

![[25287337918-Axon.ivy Designer_2017-04-07_11-53-17.png]]



  

         

![[25287337918-Axon.ivy Designer_2017-04-07_11-55-03.png]]



  

**  Step#2:** Toggle breakpoint and run remote debug

         

![[25287337918-Axon.ivy Designer_2017-04-07_14-23-02.png]]



        

![[25287337918-Axon.ivy Designer_2017-04-07_14-22-19.png]]

        

Enjoy coding <img src="https://jira.axonivy.com/confluence/s/en_GB/7103/9740d52e06037c926d0bef8c46735f0805791491/_/images/icons/emoticons/smile.png" title="(smile)" class="emoticon emoticon-smile" data-border="0" alt="(smile)" />
