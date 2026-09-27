---
ai_hash: 2e68c718e4c25d12
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.726
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508022754/Hotfix+0.01.69.01
space: LUZ
status: reference
tags:
- confluence
- infra
- space/luz
title: Hotfix 0.01.69.01
topic: infra
type: source
updated: 2020-05-15
---

# Hotfix 0.01.69.01

> [!info] Imported from Confluence
> Space **LUZ** · updated 2020-05-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/20508022754/Hotfix+0.01.69.01)
> Relevance 0.726 · topic `infra`

1.  Update POS journal template: **<span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20508022754_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-36033" macro-id="53e651c3-107d-4dac-8766-1a012d0a7083" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-36033" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-36033</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>**

2.  Gives public ability to access to all the company documents, articles. **Change config Nginx**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c3d6b32c-e808-4f20-9d9b-0397f955b59c" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    #proxy to access company document
    location ~ ^/company-info/(?<companyId>[^.]+)/documents/(?<documentId>[^.]+) {
         proxy_pass http://10.124.0.20:8080/luz_online/api/$name/documents/$documentId;
         proxy_set_header X_CUSTOMER_HEADER $name;
         include /etc/nginx/klara-reverse.conf;
    }

    Change the old above script  to NEW one below
     
    #proxy to access company document
    location ~ ^/company-info/logo {
        proxy_pass http://10.124.0.20:8080/luz_online/api/$name/company-info/logo;
        proxy_set_header X_CUSTOMER_HEADER $name;
        include /etc/nginx/klara-reverse.conf;
    }
    ```

    </div>

    </div>

3.  Resize the  onlineshop widget  <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_20508022754_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="LUZ-36072" macro-id="f25c04e9-f8a3-45f8-917b-7d3bfc73f3c3" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/LUZ-36072" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>LUZ-36072</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span>

%% ai-graph-start %%

**Related notes:**
- [[Apply changes on luz_kubernetes]]
- [[POS & myKLARA nginx ingress quick notes]]
- [[Joint review 0.03.24.00 (16.06.2026 - 29.06.2026)]]
- [[Get article thumbnail API - related modules]]
- [[Impact of code changes on common components]]

%% ai-graph-end %%