---
ai_hash: 607585d324214f23
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 5
depth: 3
entities: []
relevance: 0.779
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49122770989/UIB+LUZ-146496+-+eLetter+inline+edit
space: Helios
status: reference
tags:
- confluence
- programming
- space/helios
title: '[UIB] LUZ-146496 - eLetter inline edit'
topic: programming
type: source
updated: 2026-02-06
---

# [UIB] LUZ-146496 - eLetter inline edit

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-02-06 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49122770989/UIB+LUZ-146496+-+eLetter+inline+edit)
> Relevance 0.779 · topic `programming`

From Unified Inbox, we would like to edit eLetter’s metadata inline


![[49122770989-image-20260206-025231.png]]



------------------------------------------------------------------------

### Current behavior (Webclient \_1 & ePost)

- When users edit an eLetter and click **Save**, both Webclient\\1 and ePost call a **full update** endpoint on `luz-docs-view-controller`:

  - <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="64c97637-bbc4-46bc-9482-5dcbcc579cd4" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    PUT /luz_docs_view_controller/api/{tenant-id}/letters/{letter-id}
    ```

    </div>

    </div>

- Payload is the **full eLetter model** (complete object, not partial).

- In `luz-unified-inbox`, we currently **wrap eLetter into our own generic “message” model**, treating it like any other message type.

- Problem: **every small edit in Unified Inbox triggers a full object update**, which causes **performance overhead** (large payload, more processing, higher latency).

<div id="expander-308549850" class="expand-container conf-macro output-block" hasbody="true" macro-id="aca8aa2a-f8f0-49a8-838e-01e070373a70" macro-name="expand">

<div id="expander-control-308549850" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">tested when we only update title</span>

</div>

<div id="expander-content-308549850" class="expand-content expand-hidden">

update eletter


![[49122770989-image-20260206-035449.png]]



the business field supported when edit eletter will be removed


![[49122770989-image-20260206-035328.png]]



</div>

</div>

### Target improvement

- Introduce a **real eLetter domain model** in `luz-unified-inbox` (supports only eLetter use cases) instead of the current wrapper-as-message approach.

- Goal: reduce payload size and processing by using partial updates where possible.

------------------------------------------------------------------------

## Partial update endpoint (fits Unified Inbox requirements)

- `luz-docs-view-controller` provides a metadata-only partial update endpoint:

  - <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="edb964d8-18db-4ca7-9a73-ae3d4d381fa7" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    PATCH /luz_docs_view_controller/api/{tenant-id}/letters/{letter-id}
    ```

    </div>

    </div>

- Payload is a list of JSON Patch (-like) operations:

  - Supported ops: `add`, `replace`, `remove`

  - Example: replace `documentTitle`, remove `invoiceData/dueDate`, etc.

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9904d1b5-2461-4b21-82fb-50ae8274c84f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    [
      {
        "op": "replace",
        "path": "/documentTitle",
        "value": "document title updated"
      },
      {
        "op": "replace",
        "path": "/documentTypes",
        "value": [
          "contract"
        ]
      },
      {
        "op": "replace",
        "path": "/documentReferenceDate",
        "value": "2026-02-06"
      },
      {
        "op": "replace",
        "path": "/invoiceData/amount",
        "value": "100.00"
      },
      {
        "op": "remove",
        "path": "/invoiceData/dueDate"
      }
    ]
    ```

    </div>

    </div>

- This endpoint is **better aligned with Unified Inbox** because UI edits often change only a few fields.

### Key limitation / business logic gap

- The PATCH endpoint **only updates metadata** and **does not include additional business logic** that currently happens with the PUT approach and/or client behavior.

  - because of the logic added, we also limit the API, since it will check specific data that allowed to be updated.

- Check existing field to decide which operation will be handled from client (luz-unified-inbox). or we will use “add“ operation instead of “replace“ (with the business logic added, it will detect the changes)

  - trying with the changes by reuse the part to build history from option 1 → this approach also very complex when we do need to check each line of code (the test result below from our implementation to verify the possible approach)

  - tested with “add“ and “replace”, both will throw the same error → **<u>seems it’s not just simple re-use existing business code, but need to understand and build correct payload for partial update</u>**

    

![[49122770989-image-20260206-041436.png]]



------------------------------------------------------------------------

## Concrete issue causing functional mismatch

Scenario (confirmed via Webclient 1 testing):

1.  Change document type from `invoice` to another type  
    → document type updates AND `dueDate` is cleared.

2.  Later, in Unified Inbox, change document type back to `invoice` and set `dueDate`.

Problem for Unified Inbox:

- To correctly update `dueDate`, Unified Inbox must know whether `dueDate` is:

  - an existing field to **replace**, or

  - a missing field that must be **added**.

- That means Unified Inbox needs **state-aware patch generation logic** (diffing against current server state), otherwise it may send incorrect operations.

------------------------------------------------------------------------

## History tracking requirement

- With the PATCH approach, **history data is not available / not produced** as required.

- Additional logic is needed to ensure history/audit is created:

  - either implement/extend this business logic inside `luz-unified-inbox`, or

  - adapt/extend logic inside `luz-docs-view-controller` so PATCH updates also generate history.

------------------------------------------------------------------------

## Decision needed (where to implement the missing logic)

Two options:

1.  **Add logic in** `luz-unified-inbox` (follow webclient 1 & epost app)

    - Need specific model fully support for eLetter.

      - we fetch the data and handle everything in json object.

    - Performance → every inline changes → fully update eletter

2.  **Adapt** `luz-docs-view-controller` (adapt existing API)

    - Keep Unified Inbox thin: send intent, let server handle consistent patching rules (e.g., invoice type toggles, dueDate behavior).

    - Build correct `add/replace/remove` operations (requires reading current state or tracking previous state).

    - re-check the business and build correct payload

      - we tried to reuse code with build history, but it gonna replace more thing inside, including technical fields which partial update does not support in luz-docs

    - Centralizes business behavior and history generation for all consumers.

    - Likely simpler for clients and more consistent across systems.

    - Limit the original support from the API → before it’s allow to update every business field

3.  New API to support our specific case

    - same implementation with 2 but we could avoid to see and side affect because of the limit we add to 2

We’re continue to check with option 2

%% ai-graph-start %%

**Related notes:**
- [[LUZ-146746 Use correct API's for delete, restore and their undo]]
- [[Full-object PUT instead of dedicated endpoint is a REST caller anti-pattern]]
- [[Public API client performance analysis]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]
- [[iLetter current backend architecture]]

%% ai-graph-end %%