---
ai_hash: 28531ef36f863d47
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: Proof of concept Export and download storage (TP2020)'
status: seedling
tags:
- async
- export
- object-storage
- signed-url
- tree-reconstruction
- confluence-distilled
title: 'Long exports: acknowledge immediately, deliver by emailed link to object storage'
type: lesson
---

# Long exports: acknowledge immediately, deliver by emailed link to object storage

A "download everything" button cannot be a synchronous response. Zipping a whole archive takes minutes, exceeds every proxy timeout, and holds a request thread the entire time. The workable shape acknowledges immediately and delivers out of band:

1. User clicks **Download storage**.
2. Return at once (a `CompletionStage` / 202) — the request ends here.
3. In the background: fetch letters and folders, build the tree, **zip** it.
4. **Upload the zip to object storage.**
5. **Generate a download URL and email it to the user.**
6. Record download history against the exported items.

**Why email rather than polling.** For an operation measured in minutes, the user has already navigated away. Email is a durable, asynchronous delivery channel that does not require the page to stay open — and the link remains usable later. Polling is the better choice when the wait is seconds; email when it is minutes and the result is a file.

**Why object storage rather than streaming the zip.** The artefact outlives the request. It can be re-downloaded, resumed on a flaky connection, and served directly by the storage layer instead of through your application. A signed URL also means the download never touches your service at all.

**Rebuilding the folder tree from flat metadata** is the fiddly part, and the technique generalises. Folders arrive as a flat list with parent references, so create them in dependency order and keep an id → path dictionary as you go:

1. Split into **main (root) folders** and **sub-folders**.
2. Create the main folders; record `folderId → path` in a **folder-paths dictionary**.
3. Create sub-folders by looking up the parent's path in the dictionary, then add each new folder's own path to it.
4. Place each file by looking up its folder id in the same dictionary.

One dictionary, built in dependency order, turns "reconstruct a hierarchy" into two linear passes with no recursion and no repeated tree walks.

> [!warning] A signed URL in an email is a bearer credential
> Anyone with the link has the export — which contains the user's entire archive. Give it a **short expiry**, and prefer a link that requires authentication over a long-lived signed URL. Email is forwarded, archived, and synced to devices you do not control.

> [!tip] Tell the user what happens if it fails
> The user is gone by the time step 3 runs. If zipping fails, nothing arrives and they have no way to tell "still working" from "broken". Send a failure email too, and make the acknowledgement say roughly how long to expect — see [[Score async API designs on crash recovery and multi-instance, not latency]].

Source: [[Proof of concept Export and download storage|Proof of concept  Export and download storage]] (TP2020, Confluence).

## Related

- [[Score async API designs on crash recovery and multi-instance, not latency]]

%% ai-graph-start %%

**Related notes:**
- [[Proof of concept Export and download storage]]
- [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]]
- [[If upstream holds memory until you ack, your write latency is their OOM risk]]
- [[Score async API designs on crash recovery and multi-instance, not latency]]
- [[luz_docs Improvement - Document Reliable Delivery Proof Of Concept]]

%% ai-graph-end %%