---
ai_hash: f46e4fa63f3e3af1
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-27
entities: []
source: 'Confluence: await fetch() vs FetchBuilder (TS)'
status: seedling
tags:
- nextjs
- fetch
- caching
- race-condition
- app-router
- confluence-distilled
title: Next.js monkey-patches global fetch, and its response clone races your body
  read
type: gotcha
---

# Next.js monkey-patches global fetch, and its response clone races your body read

In the Next.js App Router, `globalThis.fetch` is **monkey-patched** to add the Data Cache and Request Memoization. Your `await fetch(...)` is not the platform `fetch` — it is a wrapper doing work you did not ask for, and that work can collide with your own body read.

Simplified from `next/dist/server/lib/patch-fetch.ts`:

```js
const patchedFetch = async (url, init) => {
  const response = await originalFetch(url, init);
  if (isCacheable(init)) {
    const cacheClone = response.clone();              // tee the stream
    queueMicrotask(() => writeToCache(cacheClone));   // read it *later*
  }
  return response;
};
```

`response.clone()` tees the body into two streams, and the cache write is deferred to a microtask. When **you** then call `await response.json()`, you consume your branch. If the cache's clone has not finished reading its branch, you get:

```
TypeError: Response.clone: Body has already been consumed
```

It surfaces in production and not locally because it is a **race** — it needs the timing of real network latency and real concurrency to lose.

**What to do:**

- **Upgrade.** Fixed in **Next.js 15.1.0** ([PR #73274](https://github.com/vercel/next.js/pull/73274)). If you are on 14.x App Router, this is a known bug and not your code.
- **Opt out of the patch** where you do not want caching — `cache: 'no-store'` makes the response non-cacheable, so no clone is taken.
- **Wrap fetch behind your own client.** A builder/helper gives you one place to set cache options, drain bodies, and normalise error shapes, instead of every call site re-deciding.

> [!tip] The generalisable lesson
> A framework that patches a global you already know is a framework that has changed the contract of that global **silently**. Your mental model of `fetch` is now wrong in ways no type signature shows. When a standard API misbehaves only under a particular framework, check whether that framework patches it before you debug your own code.

> [!note] Inconsistent error shapes are the third cost
> The same page notes error handling diverging file-by-file across the codebase — some paths reading `statusText`, others parsing a body, others throwing. That is what a shared client fixes, independently of this bug.

Related: [[Undrained fetch response bodies leak sockets in Node undici]] — the leak hiding in those same error paths.

Source: [[await fetch() vs FetchBuilder ()]] (TS, Confluence).

## Related

- [[Undrained fetch response bodies leak sockets in Node undici]]

%% ai-graph-start %%

**Related notes:**
- [[await fetch() vs FetchBuilder ()]]
- [[Undrained fetch response bodies leak sockets in Node undici]]
- [[Two next dev instances sharing one .next corrupt the webpack PackFileCache]]
- [[Next.js dev server webpack chunk cache corrupts after many route addsdeletes]]
- [[Hydration mismatches only surface as minified errors in production not dev]]

%% ai-graph-end %%