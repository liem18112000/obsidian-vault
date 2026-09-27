---
ai_hash: add66a767f9716d7
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 4
depth: 3
entities: []
relevance: 0.969
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49390485550/Proposal+eArchived+architecture+direction+for+ePost+web+2
space: Helios
status: reference
tags:
- confluence
- architecture
- space/helios
title: 'Proposal: eArchived architecture direction for ePost web 2'
topic: architecture
type: source
updated: 2026-05-07
---

# Proposal: eArchived architecture direction for ePost web 2

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-05-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49390485550/Proposal+eArchived+architecture+direction+for+ePost+web+2)
> Relevance 0.969 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="bac8083b-3789-49f4-8c33-aa9c31740c69" macro-name="toc">

</div>

# Current eArchive in GUI

- Klara web 1:


![[49390485550-image-20260506-044121.png]]



- In Community platform, the e-Archive look like this:

<div>

|  |  |
|----|----|
| 

![[49390485550-image-20260506-040228.png]]

 | 

![[49390485550-image-20260506-041539.png]]

 |

</div>

------------------------------------------------------------------------

# Proposal: eArchive Architecture Direction for epost web 2

## Decision Requested

Please confirm the target architecture direction for **eArchive in epost web 2**.

### Recommended decision

- keep `luz_docs_view_controller` as the **core archive/domain backend**

- keep the **Next.js server-side layer inside epost web 2** as the common frontend integration layer

- for eArchive, use a **dedicated backend module** (for example `luz-earchive`) instead of mixing it into `luz-unified-inbox`

This gives the clearest separation of responsibilities while keeping the frontend flow unchanged.

------------------------------------------------------------------------

## Important Clarification

The **Next.js server-side layer in epost web 2 is common in all options**.

That means the real architecture decision is **not** whether to use the server-side layer. The real decision is:

> **Which backend should eArchive in epost web 2 call behind that layer?**

------------------------------------------------------------------------

## Background

Today:

- `luz_docs_view_controller` already provides archive capabilities

- **epost app** and **klara web 1** already use this backend

- **epost web 2** has 2 user areas:

  - **Unified Inbox**

  - **eArchive**

- `luz-unified-inbox` already exists for Unified Inbox-related business

- requests from epost web 2 already go through the **Next.js server-side layer**

So the backend choice for **eArchive** is still open.

------------------------------------------------------------------------

## Options Considered

<div>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<th><p>Option</p></th>
<th><p>Summary</p></th>
<th><p>Main Advantage</p></th>
<th><p>Main Concern</p></th>
</tr>
&#10;<tr>
<td><ol>
<li><p><strong>Direct to </strong><code>luz_docs_view_controller</code></p></li>
</ol></td>
<td><p>eArchive calls <code>luz_docs_view_controller</code> directly through the epost web 2 server-side layer</p></td>
<td><p>fastest to deliver</p></td>
<td><p>eArchive in web 2 becomes tightly tied to a shared backend</p></td>
</tr>
<tr>
<td><ol start="2">
<li><p><strong>Reuse </strong><code>luz-unified-inbox</code><strong> for both menus</strong></p></li>
</ol></td>
<td><p>both Unified Inbox and eArchive go through <code>luz-unified-inbox</code></p></td>
<td><p>one backend wrapper for both menus</p></td>
<td><p>archive responsibility becomes mixed into inbox responsibility</p></td>
</tr>
<tr>
<td><ol start="3">
<li><p><strong>Dedicated eArchive module</strong></p></li>
</ol></td>
<td><p>Unified Inbox continues with <code>luz-unified-inbox</code>, while eArchive uses its own module</p></td>
<td><p>clearest ownership and cleanest separation</p></td>
<td><p>extra module to build and maintain</p></td>
</tr>
</tbody>
</table>

</div>

------------------------------------------------------------------------

## Mermaid Overview

<div id="expander-1875388702" class="expand-container conf-macro output-block" hasbody="true" macro-id="88ee0dd2-bb34-4d17-95ef-1f313d3ed294" macro-name="expand">

<div id="expander-control-1875388702" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">mermaid</span>

</div>

<div id="expander-content-1875388702" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a41e5e00-73b1-4951-b4a1-35e780f6f970" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
​flowchart TB
    APP[epost app]
    WEB1[klara web 1]
    WEB2[epost web 2]
    NEXT[Next.js server-side layer]

    LVC[luz_docs_view_controller]
    LUI[luz-unified-inbox]
    LEA[luz-earchive]

    APP --> LVC
    WEB1 --> LVC
    WEB2 --> NEXT

    NEXT --> D{Options for eArchive}

    D --> O1[Option 1<br/>Direct to luz_docs_view_controller]
    D --> O2[Option 2<br/>Reuse luz-unified-inbox]
    D --> O3[Option 3<br/>Dedicated eArchive module]

    O1 --> LVC
    O2 --> LUI
    LUI --> LVC
    O3 --> LEA
    LEA --> LVC

    classDef existing fill:#dbeafe,stroke:#2563eb,stroke-width:2px,color:#111827;
    classDef client fill:#fef3c7,stroke:#d97706,stroke-width:1.5px,color:#111827;
    classDef recommended fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#111827;
    classDef neutral fill:#e5e7eb,stroke:#6b7280,stroke-width:1.5px,color:#111827;

    class APP,WEB1,WEB2 client;
    class NEXT,LVC,LUI existing;
    class LEA,O3 recommended;
    class D,O1,O2 neutral;
```

</div>

</div>

</div>

</div>


![[49390485550-image-20260506-043622.png]]



------------------------------------------------------------------------

## Recommendation (suggested by AI for the decision)

### Recommend Option 3

Use a **dedicated eArchive module** behind the existing epost web 2 server-side layer.

In practice:

- **Unified Inbox** in epost web 2 continues to use `luz-unified-inbox`

- **eArchive** in epost web 2 uses a dedicated eArchive backend module

- that eArchive module can still call `luz_docs_view_controller` as the core archive/domain backend

### Why this is recommended

1.  **Clear ownership**  
    Unified Inbox and eArchive remain separate responsibilities.

2.  **Cleaner backend design**  
    `luz-unified-inbox` does not become a catch-all module for unrelated responsibilities.

3.  **Stable frontend model**  
    epost web 2 still uses the same Next.js server-side flow regardless of the backend choice.

4.  **Better long-term maintainability**  
    eArchive can evolve without forcing inbox-specific modules to absorb archive behavior.

------------------------------------------------------------------------

## Fallback Options

- If delivery speed is the highest priority, **Option 1** is the simplest short-term solution.

- If the team explicitly wants one wrapper module for both menus, **Option 2** is still possible, but ownership must be accepted clearly.

------------------------------------------------------------------------

## What We Are Not Recommending

### Not recommended as target state: reusing `luz-unified-inbox` for everything

Reason: this increases the risk that archive and inbox responsibilities become mixed in one module.

### Not recommended as long-term target: direct eArchive calls to `luz_docs_view_controller`

Reason: this is simple short-term, but it keeps web 2 archive behavior tightly tied to a shared backend contract.

------------------------------------------------------------------------

## Expected Benefits of the Recommended Direction

- clear responsibility split between Unified Inbox and eArchive

- simpler long-term maintenance

- less risk of turning `luz-unified-inbox` into a catch-all module

- no change to the frontend flow in epost web 2

- `luz_docs_view_controller` remains the core archive/domain backend

------------------------------------------------------------------------

## <u>Requested Confirmation</u>:

Please confirm one of the following:

1.  **Approve recommended direction**  
    Keep the Next.js server-side layer as the common frontend integration layer, and use a dedicated eArchive backend module for eArchive

2.  **Approve fallback direction**  
    Use direct integration from epost web 2 to `luz_docs_view_controller` for eArchive

3.  **Approve alternative direction**  
    Reuse `luz-unified-inbox` for both Unified Inbox and eArchive

If no objection exists, the recommended direction should be used as the target architecture for eArchive in epost web 2.

%% ai-graph-start %%

**Related notes:**
- [[Architecture]]
- [[Discuss LUZ-154249 Architecture for shared components features between apps]]
- [[Strike what every option shares to find the real architecture decision]]
- [[Business concept for frontend]]
- [[Copy 4. Architecture for delivering eLetter after email verified]]

%% ai-graph-end %%