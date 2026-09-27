---
ai_hash: c1338315676abf73
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 3
depth: 3
entities: []
relevance: 0.837
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47111077956/Adding+filter+-+public+api+adapter
space: Arrow
status: reference
tags:
- confluence
- programming
- space/arrow
title: Adding filter - public api adapter
topic: programming
type: source
updated: 2022-05-16
---

# Adding filter - public api adapter

> [!info] Imported from Confluence
> Space **Arrow** · updated 2022-05-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/47111077956/Adding+filter+-+public+api+adapter)
> Relevance 0.837 · topic `programming`

- Find json file in module luz_public_api_adapter with path `src/main/webapp/resources/group-swagger-definitions.json`

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a9b1afae-7491-49e2-953d-00f5fe3e3de8" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  [
    {
      "group": "All",
      "tags": []
    },
    {
      "group": "ePost",
      "tags": [
        "operations-tag-Klara_authentication",
        "operations-tag-Standard_ePost_communication",
        "operations-tag-Standard_ePost_identity_matching_before_delivery",
        "operations-tag-Onboarding_and_profile_management",
        "operations-tag-ePost_communication_-_V1",
        "operations-tag-ePost_identity_matching_before_delivery"
      ]
    }
  ]
  ```

  </div>

  </div>

- group-swagger-definitions.json contains json array and each object will define a group with their tags

  - group attribute:  
    - Group name will be display in filter label

    

![[47111077956-image-20220516-043301.png]]



  - tags attribute:

    - Each value in this array is an id of a section tag in public api.

    - This id will generate by swagger with syntax operations-tag-\[tagName\].

    - You can input the tagName by replace the whitespace (“ “) with the underscore (“\_“) of the annotation @Tag value (declared in each resource file)  
      Example: Standard ePost comunication → operations-tag-**Standard_ePost_communication**

      

![[47111077956-image-20220516-042657.png]]

%% ai-graph-start %%

**Related notes:**
- [[Check subscription on Public API]]
- [[Catalog for 3rd party system API]]
- [[Swagger UI]]
- [[APF swagger for project eapf_web]]
- [[Analytics Analyze API call when accessing eArchive]]

%% ai-graph-end %%