---
ai_hash: de300e734a3b06c7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.812
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38165531818/Source+Analysis
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: Source Analysis
topic: programming
type: source
updated: 2018-02-06
---

# Source Analysis

> [!info] Imported from Confluence
> Space **Helios** · updated 2018-02-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38165531818/Source+Analysis)
> Relevance 0.812 · topic `programming`

We use tool Codelyzer: <a href="https://github.com/mgechev/codelyzer" class="external-link" rel="nofollow">https://github.com/mgechev/codelyzer</a>

Install Codelyzer into Visual Studio Code:

1.  Install plugin TSLint for VSC

2.  Install tslint-angular by command: 

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e4ee3703-2314-4646-b8de-608a4d1aaa89" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    npm i tslint-angular
    ```

    </div>

    </div>

3.  Edit file tslint.json in root folder to:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b8fb7d9c-f70f-4a71-86c4-c7bc634e38ec" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    {
      "extends": ["tslint-angular"],
      "rules": {
         "component-class-suffix": false
      }
    }
    ```

    </div>

    </div>

4.  Check error all project:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c115924c-39d5-40de-bc41-3a9493419198" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
     ./node_modules/.bin/tslint -p tsconfig.json -c tslint.json
    ```

    </div>

    </div>

The problems will be shown in PROBLEM tab

%% ai-graph-start %%

**Related notes:**
- [[Static analysis tool for Android]]
- [[Fallow – Install & Usage Guide (Code Quality for JS TS)]]
- [[Analyse your source code (Copy)]]

%% ai-graph-end %%