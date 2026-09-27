---
ai_hash: 71090908e51dca01
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.877
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49393959036/await+fetch+vs+FetchBuilder
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: await fetch() vs FetchBuilder<>()
topic: programming
type: source
updated: 2026-05-11
---

# await fetch() vs FetchBuilder<>()

> [!info] Imported from Confluence
> Space **TS** · updated 2026-05-11 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49393959036/await+fetch+vs+FetchBuilder)
> Relevance 0.877 · topic `programming`

# 1. Why await fetch() Is Problematic in Next.js Server Code

In Next.js 14 App Router, globalThis.fetch is **monkey-patched** to add automatic Data Cache + Request Memoization. This wrapper does extra work (cloning, caching, deduping) that you didn't ask for, and naive code patterns trigger memory leaks, race conditions, and the `Response.clone: Body has already been consumed` error you saw in production.

------------------------------------------------------------------------

<div hasbody="true" macro-id="8992b57b-f5a6-4e8a-a0cb-0e779b658f61" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

FIXED: <a href="https://github.com/vercel/next.js/releases/tag/v15.1.0" class="external-link" data-card-appearance="inline" data-local-id="bf6f0e25-ad55-43c0-b30f-1ca7a5b1ac63" rel="nofollow">https://github.com/vercel/next.js/releases/tag/v15.1.0</a>

PR: <a href="https://github.com/vercel/next.js/pull/73274" class="external-link" data-card-appearance="inline" data-local-id="426a5fdc-dff0-4b31-8af3-e6325985aa42" rel="nofollow">https://github.com/vercel/next.js/pull/73274</a>

</div>

</div>

------------------------------------------------------------------------

## Three concrete problems with raw await fetch()

### Problem 1 — The Next.js fetch wrapper clones every response

Next.js patches fetch roughly like this (simplified from next/dist/server/lib/patch-fetch.ts):

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="f55c6225-bcf6-4169-b9ed-08a7aafae2ec" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
const patchedFetch = async (url, init) => {
  const response = await originalFetch(url, init);

  // Next.js wants to cache the body for the Data Cache
  // It does this by tee-ing the response stream
  if (isCacheable(init)) {
    const cacheClone = response.clone();
    queueMicrotask(() => writeToCache(cacheClone));
  }

  return response;
};
```

</div>

</div>

When **you** call await response.json(), the body stream is consumed. If Next.js's clone hasn't finished reading by then → `TypeError: Response.clone: Body has already been consumed`.

### Problem 2 — Forgotten body draining = leaked sockets

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="41aedbbc-078a-4b4c-ab5a-d852f1ca0e39" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
const response = await fetch(url);
if (!response.ok) {
  logger.warn(`Failed: ${response.statusText}`);   // ❌ body never read
  return null;
}
return response.json();
```

</div>

</div>

In Node.js's `undici` (the fetch engine), the response body is a `ReadableStream` backed by a socket. Until the body is **drained or canceled**, undici cannot:

- Return the socket to the keep-alive connection pool

- Free the buffered body chunks already pulled from the wire

- Mark the `Response` object as eligible for GC

So every error path that only reads response.statusText (a header property) **leaves the body undrained** → the Response and its socket stay pinned until eventual GC. Under load, faster than GC can keep up.

### Problem 3 — Inconsistent error shape across files

Compare these two error paths in the same codebase:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="92181ea8-191d-44b3-bae5-bfd33319dbd6" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// File A
if (!response.ok) {
  logger.warn(`...${response.statusText}`);
  return null;
}

// File B
if (!response.ok) {
  const text = await response.text();
  throw new Error(text);
}

// File C
if (!response.ok) {
  return { error: response.status };
}
```

</div>

</div>

Each call site reinvents error handling, often with a leak somewhere. Refactoring is fragile because every consumer expects a different shape.

------------------------------------------------------------------------

## What FetchBuilder does to fix all three

### Fix 1 — Sets signal on every request → defeats the clone race

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9968ee33-75e5-42bf-8820-177883650f2b" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
const response = await fetch(this.url, {
  // ...
  signal: this.abortController.signal,   // ← Next.js treats requests with signals
                                          //   as non-cacheable by default
});
```

</div>

</div>

When a signal is present, Next.js's wrapper skips the `clone()` step → no body-consumption race possible.

### Fix 2 — Always drains the body on every code path

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="d664fa3a-4f40-4068-921f-2d07359f2d1d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Error path
if (!response.ok) {
  const errorText = await response.text();   // ✅ drains body
  return { error: { status: response.status, reason: errorText || response.statusText } };
}

// DELETE path
if (this.method === 'DELETE') {
  await response.text();   // ✅ drains body even when we don't need it
  return { result: {} as T };
}

// Success paths
const data = await response.json();   // ✅ drains body
// or
const text = await response.text();   // ✅ drains body
```

</div>

</div>

There is no path through FetchBuilder.execute() that returns without consuming the body. Sockets are released immediately to the pool.

### Fix 3 — Uniform { result?, error? } envelope

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f37aba8-354a-472f-b29f-8ab684f392dd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
const response = await new FetchBuilder<MyType>().withUrl(url).execute();
if (response.error) { ... }
return response.result ?? null;
```

</div>

</div>

Every caller handles errors the same way. Adding logging, retries, tracing, or telemetry happens once in FetchBuilder and applies everywhere.

### Bonus — built-in features

- Centralized timeout via AbortController

- Automatic logging

- Tenant/workplace ID injection

- cache / tags / next.tags for opt-in Next.js caching when you actually want it

# 2. When you can keep raw fetch()

It's not always wrong. Acceptable uses:

<div>

|  |  |
|----|----|
| Use case | Why OK |
| Browser-side fetch (Client Component, hook, browser util) | No Node heap pressure; browser GC handles it |
| Inside `app/api/.../route.ts` for stream-through proxying (new Response(upstream.body)) | Body is forwarded as a stream, never consumed by your code |
| Calling third-party SDKs that wrap fetch internally (e.g. matrix-js-sdk) | The SDK manages its own response lifecycle |
| Tests / Storybook stories | Not in production hot paths |

</div>

For everything **server-side that consumes the body**, prefer FetchBuilder

# 3. The leak risk per call site (general rules)

<div>

|  |  |
|----|----|
| Call site characteristic | Leak risk |
| Server-side raw await fetch() without cache: 'no-store' | **HIGH** — Next.js wraps it for caching, race possible |
| Error path that doesn't drain the body (response.statusText only) | **HIGH** — body buffer + socket pinned |
| `DELETE` / `204 No Content` without consuming body | **MEDIUM** — body is empty but socket still locked |
| response.json() followed by anything else | **HIGH** — clone race window |
| Called frequently (per render, per poll, per WebSocket message) | **AMPLIFIED** — leak compounds |
| Called once at startup | **LOW** — leak exists but bounded |

</div>

# 4. Analyze code in luz-next

## Workspace audit: 71 raw await fetch()

After excluding the legitimate ones, here's the breakdown:

### Excluded — NOT a leak risk

<div>

|  |  |
|----|----|
| File | Why safe |
| `service/fetch-builder.ts` | The implementation itself (drains body correctly) |
| Browser-side calls (`'use client'` files, hooks, components, `app/api/.../route.ts` proxies) | Run in browser or are HTTP route handlers — Next.js fetch wrapper applies but no SSR memory pressure |
| image-upload.stories.tsx | Storybook only |
| blob-to-file.ts, pdf-preview.tsx, doc-upload.tsx, timeline-utils.ts | Browser blob/URL fetches |

</div>

### **HIGH PRIORITY** — server-side, frequently called

These are `'use server'` files exhibiting the **same pattern** as the leaking tenant-service.ts:

<div>

|  |  |  |  |
|----|----|----|----|
| File | \# calls | Frequency | Notes |
| feature-switch-service.ts:45 | 1 | **5× per page render, every user** | The single biggest amplifier — your logs show 5 feature switch calls per navigation |
| communities-service.ts:208 | 11 | Per chat render/room load | Most calls in the workspace |
| community-epost-directory-caller.ts:25 | 6 | Per Matrix message author | `getUserDomainMappingByMatrixId` is the worst |
| community-setting-caller.ts:182 | 6 | Per community settings load | Same pattern |
| token-service.ts:12 | 3 | **Per login + refresh cycle** | Token leaks are particularly bad — long-lived sockets |
| terms-of-use-service.ts:97 | 2 | Per login | Logs show this firing for every user |

</div>

### **MEDIUM PRIORITY** — server-side, less frequent

<div>

|  |  |  |
|----|----|----|
| File | \# calls | Notes |
| smartsend-subscription-service.ts:36 | 1 | Per smartsend page render |
| web-push-notification-caller.ts:10 | 3 | Per subscription event |
| luzsec-caller.ts:35 | 1 | Auth-related |
| import-contact-service.ts:121 | 2 | Per contact import |
| apps/luz-epost/app/\[locale\]/(auth)/communities/actions/e2ee/backup-keys.ts | 2 | E2EE setup |
| apps/luz-epost/app/\[locale\]/(auth)/smart-letter/\_actions/pre-resolve-api-sources.ts | 2 | Server action |
| apps/luz-epost/app/\[locale\]/smartletter/letters/\[letterId\]/\_utils/submit-letter-request.ts | 1 | Letter submission |

</div>

### **LOW PRIORITY** — server-side, rare or already flagged

<div>

|  |  |
|----|----|
| File | Notes |
| apps/luz-epost/app/api/auth/\[...nextauth\]/refresh-token.ts | NextAuth refresh — runs per session refresh |
| apps/luz-epost/app/api/auth/\[...nextauth\]/refresh-access-token.ts | Same |
| apps\luz-epost\app\api\auth\logout\route.ts:29 | Logout — fire-and-forget without awaiting body, **also leaks** |
| apps\luz-epost\app\api\unified-inbox\letter-thumbnail\route.ts:27 | Image proxy — large bodies, HIGH risk per call but rare |
| apps\luz-epost\app\api\unified-inbox\letter-logo\route.ts:40 | Same |
| apps\luz-epost\app\api\unified-inbox\download-letter\route.ts:42 | PDF proxy — VERY large bodies |
| apps\luz-epost\app\api\internal\e2e-test\cleanup-rooms\route.ts:31 | Test infra |
| apps\luz-epost\app\api\internal\e2e-test\cleanup-contacts\route.ts:31 | Test infra |
| apps/luz-epost/app/api/smart-letter/\[letterId\]/proxy/route.ts | Smart letter proxy |
| apps/luz-epost/app/\[locale\]/(auth)/communities/matrix-client/SlidingSyncManager.ts | Sliding sync init — runs per Matrix client setup |

</div>

<div hasbody="true" macro-id="7383aa52-de69-4df7-9534-593b56318efa" macro-name="note">

<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon"> </span>

<div>

## The "biggest bang for buck" priorities

1.  **feature-switch-service.ts:45** — runs **5 times per page render per user**. Single biggest amplifier in your logs (~50+ calls in the 10-minute window).

2.  **communities-service.ts:208** — 11 raw fetch calls in one file, called heavily during Matrix room rendering.

3.  **token-service.ts:12** — token-related calls keep sockets pinned for the duration of token lifetime.

</div>

</div>

%% ai-graph-start %%

**Related notes:**
- [[Next.js monkey-patches global fetch, and its response clone races your body read]]
- [[Undrained fetch response bodies leak sockets in Node undici]]
- [[Code review v2.0]]
- [[Concurrency Design Patterns]]
- [[Classify stream failures on the server, not the client]]

%% ai-graph-end %%