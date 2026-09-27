---
ai_hash: 44e0024ba0619c4b
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: session 2026-09-27 Confluence export
status: seedling
tags:
- confluence
- atlassian
- rest-api
- pagination
- gotcha
title: Confluence CQL search paginates by opaque cursor, not start offset
type: gotcha
---

# Confluence CQL search paginates by opaque cursor, not start offset

Confluence Cloud's CQL search endpoint (`/wiki/rest/api/content/search`) paginates with an **opaque cursor**, not a numeric offset. The `start` parameter you pass is silently ignored, and `_links.next` is present on *every* response — including the last one. So the familiar offset loop is an infinite loop here:

```js
// WRONG - refetches page 1 forever, `next` never disappears
let start = 0;
while (true) {
  const j = await get(`...&start=${start}`);
  all = all.concat(j.results);
  if (!j._links.next) break;
  start += 100;
}
```

The correct pattern is to follow `_links.next` verbatim (it already carries `?next=true&cursor=<opaque>`), resolve it against `_links.base`, and terminate on **zero new ids** rather than on a missing link:

```js
let url = `${BASE}/wiki/rest/api/content/search?cql=${q}&limit=100`;
const seen = new Map();
for (let hop = 0; hop < 60 && url; hop++) {
  const j = await get(url);
  let fresh = 0;
  for (const p of j.results) if (!seen.has(p.id)) { seen.set(p.id, p); fresh++; }
  if (!j.results.length || !fresh) break;          // real terminator
  const nx = j._links?.next;
  url = nx ? (j._links.base ?? BASE) + nx.replace(/^\/wiki/, "") : null;
}
```

Three defences worth keeping: dedupe by id, a hop cap, and the zero-new-ids break. Any one of them turns a silent 30-minute hang into a clean exit.

The inconsistency is the real trap — **not every Confluence v1 endpoint behaves this way.** `/wiki/rest/api/content/{id}/child/attachment` *does* honour `start`-based offset paging, so the same codebase legitimately needs both loop styles. Check which kind an endpoint is before reusing a pagination helper.

Also note the response has no `totalSize`; you cannot know the result count up front.

## Related

- [[Export Confluence to markdown via body.view HTML, not body.storage]]

%% ai-graph-start %%

**Related notes:**
- [[A pagination token is an opaque cursor, and it must carry the filter it was issued under]]
- [[Export Confluence to markdown via body.view HTML, not body.storage]]
- [[Offset-paging loop with while(offset % pageSize == 0) infinite-loops on exact-multiple counts]]
- [[Pagination Token (Page Token)]]
- [[Bitbucket Cloud API pagination returns full URLs in 'next', not relative paths]]

%% ai-graph-end %%