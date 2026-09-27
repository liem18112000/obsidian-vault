---
ai_hash: 87b18509c3a7d088
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 10
depth: 2.76
entities: []
relevance: 0.762
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/30906866274/Technical+Review+for+WebClient+Design+Sprint+28+6+4+2021
space: TP2020
status: reference
tags:
- confluence
- programming
- space/tp2020
title: Technical Review for WebClient Design Sprint 28 (6/4/2021)
topic: programming
type: source
updated: 2021-04-06
---

# Technical Review for WebClient Design Sprint 28 (6/4/2021)

> [!info] Imported from Confluence
> Space **TP2020** · updated 2021-04-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/30906866274/Technical+Review+for+WebClient+Design+Sprint+28+6+4+2021)
> Relevance 0.762 · topic `programming`

# **1. UI**

**Handle KLP login exception**

**

![[30906866274-image2021-4-6_11-17-54.png]]

**

**<span class="legacy-color-text-blue3">E-Post Business registration flow</span>**

**<span class="legacy-color-text-blue3">

![[30906866274-image2021-4-6_10-42-47.png]]

    

![[30906866274-image2021-4-6_10-44-26.png]]

</span>**

**<span class="legacy-color-text-blue3">Mylife webclient</span>**

**<span class="legacy-color-text-blue3">

![[30906866274-image2021-4-6_10-48-29.png]]

</span>**

**<span class="legacy-color-text-blue3">Manage profile page</span>**

**<span class="legacy-color-text-blue3">

![[30906866274-image2021-4-6_13-40-6.png]]

</span>**

**<span class="legacy-color-text-blue3">Reset password</span>**

**<span class="legacy-color-text-blue3">

![[30906866274-image2021-4-6_13-41-1.png]]

     

![[30906866274-image2021-4-6_13-41-31.png]]

</span>**

# **2. CODE**

**Registration flow**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5f230e26-0bf8-444b-baa3-dfe3b50d14b4" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**template.ftl**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
............
    <div class="user-registration-panel">
        <#nested "form">
        <#nested "powerBy">
    </div>
...........
```

</div>

</div>

**  **

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e64f54b2-3be6-49ff-9872-e2c912306e35" macro-name="code" style="border-width: 1px;">

<div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">

**login.ftl**

</div>

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
............
<#if section="powerBy">
    <#include "${properties.powerBy}"/>
</#if>
............
```

</div>

</div>

  

<div id="expander-1907023072" class="expand-container conf-macro output-block" hasbody="true" macro-id="be8c8c08-f544-419c-8fa1-4d7be874de2f" macro-name="expand">

<div id="expander-control-1907023072" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">login flow</span>

</div>

<div id="expander-content-1907023072" class="expand-content expand-hidden">


![[30906866274-image2021-4-6_11-37-44.png]]



</div>

</div>

<div id="expander-546266928" class="expand-container conf-macro output-block" hasbody="true" macro-id="35b8cc61-b88f-4769-a0e6-fc285d8797f8" macro-name="expand">

<div id="expander-control-546266928" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Hide menu based on client</span>

</div>

<div id="expander-content-546266928" class="expand-content expand-hidden">


![[30906866274-image2021-4-6_11-52-32.png]]



</div>

</div>

<div id="expander-227187812" class="expand-container conf-macro output-block" hasbody="true" macro-id="6d8df3d3-bae6-457a-927f-d4f42776a172" macro-name="expand">

<div id="expander-control-227187812" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">template.ftl</span>

</div>

<div id="expander-content-227187812" class="expand-content expand-hidden">


![[30906866274-image2021-4-6_11-55-31.png]]



</div>

</div>

- Displaying documents in Letterbox.
- Displaying document in detail.
- Handle animation stuff.

%% ai-graph-start %%

**Related notes:**
- [[Test and code review report template]]
- [[Programming]]
- [[Joint review 0.03.28.00 (11.08.2026 - 24.08.2026)]]
- [[How to implement a feature hint for eArchive (reuse new common component )]]
- [[Hotfix 0.01.69.01]]

%% ai-graph-end %%