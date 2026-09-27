---
title: "luz_docs_import dedup: folders via view-controller API, documents via import job history"
created: 2026-09-15
type: lesson
status: seedling
source: "Testing-Agent refine session 2026-09-14"
tags: [luz-docs, luz_docs_import, luz_docs_view_controller, dedup, zip-import, LUZ-158230]
---

# luz_docs_import dedup: folders via view-controller API, documents via import job history

> [!warning] Document-dedup mechanism corrected against the confirmed spec
> The "document dedup via import job history / filename" claim was an interrogation guess. **Spec truth:** a document already present in the **target folder with the SAME NAME AND SAME SIZE is SKIPPED**, existing kept unchanged (an idempotent skip). See [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]].

In the Post Health ZIP import (luz_docs_import), duplicate detection:
- **Document dedup** (spec) = **same name + same size in the target folder → skipped**, existing document kept unchanged.
- **Folder handling** (spec) = folders (incl. nested) recreated **1:1** in ePost; existing folders matched, not duplicated. Implementation detail (plausible, not spec-stated): folder resolve/create goes through the **luz_docs_view_controller folder API** before uploading docs.

Why it matters: a tester exercises document dedup by re-sending an identical name+size file (expect skip) and folder handling by re-importing an existing folder tree (expect 1:1 match, no duplicate folders). No recipient/folder auto-provisioning.

## Related

- [[LUZ-158230 ePost ZIP import spec v1.0 (authoritative)]]
- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
- [[LUZ-158230 QA edge-case decisions (ZIP import)]]

## Related

- [[LUZ-158230 Post Health ZIP import v1 scope decisions]]
