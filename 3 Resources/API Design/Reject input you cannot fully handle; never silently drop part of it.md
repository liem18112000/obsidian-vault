---
title: "Reject input you cannot fully handle; never silently drop part of it"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Secure File Upload via Public API to eArchive (TS)"
tags: [api-design, validation, error-handling, data-loss, confluence-distilled]
---

# Reject input you cannot fully handle; never silently drop part of it

The worst possible response to input you cannot handle is to **quietly accept it and do less than asked**. A file-upload design review found exactly that: the existing code accepted a multi-file request and silently processed one, discarding the rest.

The fix specified was to **reject explicitly**:

> One `files` part per request; reject anything else with **`400 TOO_MANY_FILES`** rather than the current **silent drop** in `processFiles`.

**Why the silent drop is worse than an error:**

- **The caller believes it succeeded.** A `200` after uploading five files and storing one is a lie the client has no way to detect without re-reading its own upload.
- **The data loss is invisible until someone looks for the document.** Which may be months later, in an archive, when the original is gone.
- **It cannot be distinguished from a bug elsewhere.** Support sees "my file is missing" with a successful request in the logs.

An explicit `400` with a **named code** — `TOO_MANY_FILES`, not a prose message — costs the caller one failed request and tells them precisely what to change. It is also machine-readable, so a client can branch on it (split into N requests) rather than parse English.

> [!tip] Look for silent drops wherever a loop meets a limit
> The pattern hides in `list.get(0)`, `.first()`, `take(1)`, and "we only support one for now" comments. The tell is code that **narrows** input without checking whether it narrowed anything. Any place you take the head of a collection the caller supplied, ask: *what if there were more?* Either handle them or reject them — never drop them.

> [!note] Phase the capability, but not the honesty
> The same design defers real batch upload: phase 1 is many requests, one file each, with a `/documents:batch` endpoint designed later — because true batching needs DB-side batch handling and **per-item error reporting** (see [[Bulk operations need per-item outcomes, not one status code]]). Deferring the feature is fine. Accepting batch input and silently honouring part of it is not.

Source: [[Secure File Upload via Public API to eArchive]] (TS, Confluence).

## Related

- [[Bulk operations need per-item outcomes, not one status code]]
