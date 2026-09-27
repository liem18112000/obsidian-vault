---
title: "eLetter Performance - Investigation Report: Load Times and Optimization Recommendations"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49274617874/eLetter+Performance+-+Investigation+Report+Load+Times+and+Optimization+Recommendations
space: "Helios"
topic: other
relevance: 0.721
depth: 2.6
updated: 2026-03-27
attachments: 0
tags:
  - confluence
  - other
  - space/helios
---

# eLetter Performance - Investigation Report: Load Times and Optimization Recommendations

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-03-27 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49274617874/eLetter+Performance+-+Investigation+Report+Load+Times+and+Optimization+Recommendations)
> Relevance 0.721 · topic `other`

# 📊 eLetter Performance Investigation Report

> **📅 Date:** 2026-03-26  
> **👤 Author:** dhthinh  
> **🌐 Environment:** `https://client-dev.klara-epost.tech`  
> **📦 Dataset:** 48 eLetter items · account **GMB**

------------------------------------------------------------------------

## 🔴 Executive Summary

Both measured scenarios **FAIL** against the target threshold of **\< 1,000 ms**.

- The total load time for the eLetter list is **5,620 ms** (~5.6× slower).

- Opening a detailed eLetter view takes **3,170 ms** (~3.2× slower).

- The main bottlenecks are:

  - **High TTFB** (1,432 ms — middleware optimization not yet deployed)

  - **Slow React list rendering** (2,625 ms)

  - **Click dispatch delay** (1,046 ms)

### Key takeaways

- eLetter list and eLetter detail both exceed the 1,000 ms target by 3–5×.

- List TTFB (1,432 ms) is dominated by server-side work that should be reduced by middleware optimization.

- Client-side issues (React list render and click handling) significantly impact perceived responsiveness.

- Network transfer (RSC stream) is relatively cheap compared to server and React rendering time.

<div>

|                                       |              |             |         |
|---------------------------------------|--------------|-------------|---------|
| Scenario                              | Result       | Threshold   | Status  |
| eLetter List — 48 items               | **5,620 ms** | \< 1,000 ms | ❌ FAIL |
| eLetter Content View — click → render | **3,170 ms** | \< 1,000 ms | ❌ FAIL |

</div>

------------------------------------------------------------------------

## 1. Performance Tool Comparison

<div>

|  |  |  |
|----|----|----|
| Tool | Advantages | Disadvantages |
| **Playwright + CDP** ⭐ | ✅ Full auth flow (Keycloak, Konto-Wählen) · ✅ Real end-to-end data · ✅ Automation & CI/CD ready · ✅ Access to `PerformanceResourceTiming` (TTFB, download, connection) · ✅ Precise network timestamps via CDP | ❌ No built-in HTML report · ❌ Cannot separate Next.js vs upstream without `Server-Timing` · ❌ Wall-clock affected by local machine load |
| **Lighthouse** | ✅ Built into Chrome DevTools · ✅ Core Web Vitals scores (LCP, FID, CLS, INP) · ✅ Pretty HTML reports · ✅ Throttling emulation (3G, CPU) | ❌ Only non-authenticated pages (no Keycloak/SSO) · ❌ Synthetic data, not the real inbox · ❌ Dev-server results not representative of production |
| **WebPageTest** | ✅ Filmstrip view (frame-by-frame) · ✅ Multi-location and multi-device · ✅ Full request waterfall · ✅ Custom scripting support | ❌ Public URL required (no localhost) · ❌ SSO handling is hard · ❌ API key/paid plan for advanced features · ❌ CI integration is more complex |
| **Chrome DevTools (manual)** | ✅ No setup required · ✅ Network waterfall + Performance timeline · ✅ JavaScript flame chart | ❌ Not reproducible or automated · ❌ Results vary between runs · ❌ Hard to track over time |

</div>

------------------------------------------------------------------------

## 2. Chosen Tool — Playwright + CDP

> **→** `Playwright + CDP` (`measure-cdp.mjs`) is chosen as the best fit for realistic measurement.

Playwright connects to a running Chrome instance via the Chrome DevTools Protocol (CDP), reusing an already-authenticated session (Michael Dänzer / Keycloak) and measuring against real data (no mocks).

### Reasons for the choice

<div>

|  |  |  |
|----|----|----|
| \# | Reason | Details |
| 1 | **Auth flow support** | Automatically handles the “Konto wählen” dialog and Keycloak OAuth redirect. |
| 2 | **Real data** | Measures the actual inbox (48 items, real PDFs), not synthetic fixtures. |
| 3 | **Per-hop timing** | `callAt` (ISO timestamp) · TTFB from `PerformanceResourceTiming` · `Server-Timing` header · `downloadMs` (RSC stream). |
| 4 | **CI/CD ready** | JSON output (`perf-results.json`) compatible with Grafana / InfluxDB. |
| 5 | **No additional infrastructure** | Reuses the developer’s running Chrome; no separate server required. |

</div>

------------------------------------------------------------------------

## 3. Measurement Results — client-dev.klara-epost.tech (2026-03-26)

> **🌐 Environment:** `https://client-dev.klara-epost.tech`  
> **📦 Dataset:** 48 eLetter items · account **GMB**  
> **⏰ Measurement window:** 2026-03-26T02:58 – 03:00 UTC

### 3.1 Summary tables

#### 3.1.1 eLetter List — real data (48 items loaded)

<div>

|  |  |  |  |  |
|----|----|----|----|----|
| Phase | Time (ms) | Target | Status | Notes |
| Nav reload → DOM ready (HTML parsed) | 1,612 | \< 500 | ❌ | Slow DOM/hydration |
| Client → Next.js (connection setup) | 0 | \< 100 | ✅ | Negligible |
| TTFB: client sent → Next.js first byte back | 1,432 | \< 1,000 | ❌ | Includes Next.js + inbox; no `Server-Timing` split |
| Next.js → client (RSC stream download) | 4 | \< 500 | ✅ | Small RSC payload |
| Client received data → React renders items | 2,625 | \< 500 | ❌ | React list render is slow |
| **TOTAL: navigation → first item visible** | **5,620** | \< 1,000 | ❌ | End-to-end list load |

</div>

> **Scroll note:** All 5 scroll attempts found no new items — the list is fully loaded (48 items total). Scroll performance is not applicable here.

#### 3.1.2 eLetter Content View — first inbox item — click → full render

<div>

|  |  |  |  |  |
|----|----|----|----|----|
| Phase | Time (ms) | Target | Status | Notes |
| Click → first POST sent (client dispatch) | 1,046 | \< 200 | ❌ | Click handling delayed; main thread likely busy |
| Group: client → Next.js (connection setup) | 0 | \< 100 | ✅ | Negligible |
| Group: TTFB (client sent → Next.js first byte) | 877 | \< 1,000 | ✅ | Includes Next.js + luz-unified-inbox |
| Group: Next.js → client (RSC stream) | 422 | \< 300 | ❌ | RSC stream somewhat slow |
| Last API response → message render (React) | 1,243 | \< 500 | ❌ | React re-render is slow |
| **TOTAL: click → detail panel visible** | 1,341 | \< 1,000 | ❌ | Panel visible but content incomplete |
| **TOTAL: click → content area visible** | 1,344 | \< 1,000 | ❌ | Main content area visible |
| **TOTAL: click → message fully rendered** | **3,170** | \< 1,000 | ❌ | Fully rendered state |

</div>

### 3.2 Raw output from `measure-cdp.mjs` (reference)

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fd13cde2-7084-40a6-96d0-d0ae959e3096" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
📊 eLetter List — real data (48 items loaded)
──────────────────────────────────────────────────────────────────────
  ❌ ① nav reload → DOM ready (HTML parsed)       1612ms   (< 500ms)
  ✅ ② client → Next.js (connection setup)        0ms      (< 100ms)  🕐 called 2026-03-26T02:58:53.917Z
  ❌ ③ TTFB: client sent → Next.js 1st byte back  1432ms   (< 1000ms) 🕐 called 2026-03-26T02:58:53.917Z
         └ (Next.js + inbox time — add Server-Timing to split) 1432ms   (info)
  ✅ ④ Next.js → client (RSC stream download)     4ms      (< 500ms)
  ❌ ⑤ client received data → React renders items 2625ms   (< 500ms)
  ❌ ⑥ TOTAL: nav → first item visible            5620ms   (< 1000ms)
  Total: 5620ms | Overall: ❌ FAIL
⏱  Measuring: scroll performance...
  ℹ️  All 5 scroll attempts found no new items — list is fully loaded (48 items total). Scroll perf N/A.
⏱  Measuring: eLetter content (group detail) load...
   📡 [2026-03-26T03:00:00.546Z] 1st POST sent (group detail)  (client → Next.js)
   📥 [2026-03-26T03:00:01.423Z] 1st POST response (200)  +877ms  (Next.js → client)
   ℹ️  Server-Timing not present for group call
📊 eLetter Content View — first inbox item — click → full render
──────────────────────────────────────────────────────────────────────
  ❌ ① click → 1st POST sent (client dispatch)    1046ms   (< 200ms)
  ✅ ② group: client → Next.js (connection setup) 0ms      (< 100ms)  🕐 called 2026-03-26T03:00:00.546Z
  ✅ ③ group: TTFB (client sent → Next.js 1st byte) 877ms  (< 1000ms) 🕐 called 2026-03-26T03:00:00.546Z
         └ (Next.js + luz-unified-inbox — add Server-Timing to split) 877ms    (info)
  ❌ ④ group: Next.js → client (RSC stream)       422ms    (< 300ms)
  ❌ ⑧ last API response → message renders (React) 1243ms  (< 500ms)
  ❌ ⑨ TOTAL: click → detail panel visible        1341ms   (< 1000ms)
  ❌ ⑩ TOTAL: click → content area visible        1344ms   (< 1000ms)
  ❌ ⑪ TOTAL: click → message fully rendered      3170ms   (< 1000ms)
  Total: 3170ms | Overall: ❌ FAIL
💾 Results saved to tests/perf/results/perf-results.json
══════════════════════════════════════════════════════════════════════
📋 SUMMARY
══════════════════════════════════════════════════════════════════════
  ❌ eLetter List — real data (48 items loaded):                  5620ms
  ❌ eLetter Content View — first inbox item — click → full render: 3170ms
```

</div>

</div>

> **Comment:** List TTFB of 1,432 ms indicates that the middleware optimization (skipping `isSmartSendSubscribed` for non-smartsend routes) has **not yet been deployed** on client-dev. Once it is deployed, the list TTFB is expected to drop from ~1,400 ms to ~50 ms.

------------------------------------------------------------------------

## 4. Recommendations & Next Actions

### High priority 🔴

<div>

|  |  |
|----|----|
| Issue | Action |
| **List TTFB 1,432 ms** | Deploy the middleware optimization (skip `isSmartSendSubscribed` for non-smartsend routes) — expected TTFB ~50 ms. |
| **Slow React list render (2.6 s)** | Profile with React DevTools; 2.6 s to mount 48 items is abnormal. Apply list virtualization and `React.memo`. |
| **RSC stream 422 ms** | Reduce serialized data in the group detail server action; target RSC stream \< 300 ms. |

</div>

### Medium priority 🟡

<div>

|  |  |
|----|----|
| Issue | Action |
| **DOM ready 1,612 ms** | Identify large JS bundles that block parsing; use dynamic import for heavy components. |
| **TTFB (list)** | Add a `Server-Timing` header to the Next.js server action to distinguish Next.js vs upstream. |

</div>

### Low priority 🟢

<div>

|  |  |
|----|----|
| Issue | Action |
| **Click → dispatch delay 1,046 ms** | Main thread is busy during React render — use `useTransition` to move low-priority updates. |

</div>

------------------------------------------------------------------------

## 5. Client-Side Analysis — What Can Be Improved?

> Based on three purely client-side aspects observed on client-dev: DOM parse (1.6 s), React list render (2.6 s), and click dispatch delay (1.0 s).

### 5.1 Slow React list render — 2,625 ms to mount 48 items

**Possible causes**

- Rendering all 48 items at once, without virtualization.

- eLetter item components re-render unnecessarily (missing `React.memo`).

- Heavy computations during render (sort, filter, format) without memoization.

**A — List virtualization (most impactful)**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="bb817cb4-92ff-46ed-be66-78cb602db3cc" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import { useVirtualizer } from '@tanstack/react-virtual';
function ELetterList({ items }: { items: ELetter[] }) {
  const parentRef = useRef<HTMLDivElement>(null);
  const virtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 72, // estimated row height in px
  });
  return (
    <div
      ref={parentRef}
      style={{ height: '600px', overflow: 'auto', position: 'relative' }}
    >
      <div style={{ height: virtualizer.getTotalSize() }}>
        {virtualizer.getVirtualItems().map(vItem => (
          <div
            key={vItem.key}
            style={{
              position: 'absolute',
              transform: `translateY(${vItem.start}px)`,
              width: '100%',
            }}
          >
            <ELetterItem item={items[vItem.index]} />
          </div>
        ))}
      </div>
    </div>
  );
}
```

</div>

</div>

> With 48 items and ~8 visible at a time, this typically reduces render work by ~5×.

**B — Memoize the item component**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f66deb5-35eb-412a-8b45-1bc3d62db17f" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
function ELetterItemBase({ item }: { item: ELetter }) {
  return /* ... */;
}
export const ELetterItem = React.memo(
  ELetterItemBase,
  (prev, next) =>
    prev.item.id === next.item.id &&
    prev.item.isRead === next.item.isRead
);
```

</div>

</div>

**C — Use** `useMemo` for derived data

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="41d230a8-51b2-43a1-9ce6-6610f8dd5a4c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Without memo: runs every render
const sorted = items.sort((a, b) => b.date - a.date);
// With memo: recomputes only when items change
const sorted = useMemo(
  () => [...items].sort((a, b) => b.date - a.date),
  [items]
);
```

</div>

</div>

------------------------------------------------------------------------

### 5.2 Click → POST dispatch delay — 1,046 ms

**Cause**

The main thread is busy when the user clicks, so the click event is queued and dispatched late.

Likely reasons:

1.  React is still rendering the list (the 2.6 s render overlaps with the click).

2.  A heavy handler runs synchronously in the click path.

**A —** `useTransition` for non-urgent updates

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f83bc88-1071-46fa-8536-41d7fff7a986" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import { useTransition, useState } from 'react';
function ELetterListContainer() {
  const [isPending, startTransition] = useTransition();
  const [selectedId, setSelectedId] = useState<string | null>(null);
  const handleSelect = (id: string) => {
    startTransition(() => {
      setSelectedId(id);
    });
  };
  return (
    <>
      {isPending && <Spinner />}
      <ELetterList onSelect={handleSelect} selectedId={selectedId} />
    </>
  );
}
```

</div>

</div>

**B — Defer heavy filtering with** `useDeferredValue`

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="2153d27a-f153-4469-9602-bc45ce5c3878" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
const deferredQuery = useDeferredValue(searchQuery);
const filteredItems = useMemo(
  () => items.filter(item => matchesQuery(item, deferredQuery)),
  [items, deferredQuery]
);
```

</div>

</div>

------------------------------------------------------------------------

### 5.3 DOM ready 1,612 ms — JS bundle blocking parse

**Cause**

The browser must download, parse, and execute a large JS bundle before DOM is ready (blocking scripts).

**A — Dynamic import for heavy components**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8c3c67c1-2f54-4dca-9391-10fec796a58a" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Static import (bundled into main chunk)
// import { PDFViewer } from '@/components/pdf-viewer';
const PDFViewer = dynamic(() => import('@/components/pdf-viewer'), {
  loading: () => <Skeleton />,
  ssr: false,
});
```

</div>

</div>

**B — Inspect bundle size with Next.js Bundle Analyzer**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="094eec77-1831-4fb3-85fb-0eacfea55ebd" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
ANALYZE=true nx build luz-epost
```

</div>

</div>

Identify large eagerly-loaded chunks (\> 100 KB) that are not needed for the initial render.

**C — Preload critical data in parallel**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="ace605cb-3d83-4161-bd73-07bc95a959d2" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
// Sequential
export default async function Layout() {
  const session = await getSession();
  const settings = await getSettings(/* ... */);
}
// Parallel
export default async function Layout() {
  const [session, settings] = await Promise.all([
    getSession(),
    getSettings(/* ... */),
  ]);
}
```

</div>

</div>

------------------------------------------------------------------------

### 5.4 Client-side summary

<div>

|  |  |  |  |
|----|----|----|----|
| Issue | Solution | Effort | Expected impact |
| React render 2.6 s (48 items) | List virtualization (`@tanstack/virtual`) | Medium | ~70–80% render time reduction |
| Excess React re-renders | `React.memo` + `useMemo` | Low | ~20–40% fewer re-renders |
| Click dispatch delay ~1 s | `useTransition` for list updates | Low | Click feels immediate |
| DOM ready 1.6 s | Dynamic import for heavy components | Medium | Smaller initial bundle |
| DOM ready 1.6 s | Bundle analyzer to identify hot chunks | Low | Pinpoint root causes |

</div>

> **Note:** Server-side fixes (compression, smaller RSC payloads) and client-side optimizations should be implemented in parallel.

------------------------------------------------------------------------

## 6. Technical Guide — Adding a `Server-Timing` Header

Once this header is added, `measure-cdp.mjs` will show per-hop breakdowns that help pinpoint whether bottlenecks are in Next.js or `luz-unified-inbox`.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0a77a854-6c08-493c-b8cf-a4202f2d232c" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
import { headers } from 'next/headers';
export async function someServerAction() {
  const actionStart = performance.now();
  const upstreamStart = performance.now();
  const result = await fetchFromUnifiedInbox(/* ... */);
  const upstreamMs = Math.round(performance.now() - upstreamStart);
  const totalMs = Math.round(performance.now() - actionStart);
  headers().set(
    'Server-Timing',
    `next-action;dur=${totalMs}, upstream;dur=${upstreamMs};desc="luz-unified-inbox"`
  );
  return result;
}
```

</div>

</div>

**Sample output after adding:**

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="81e8b886-70cb-4d38-9983-803aa0de079e" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
③ group: TTFB                                   628ms
   └ [Server-Timing] next-action                628ms
   └ [Server-Timing] upstream: luz-unified-inbox 580ms
```

</div>

</div>
