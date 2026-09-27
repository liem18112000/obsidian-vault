---
title: "[Discuss] LUZ-154249 Architecture for shared components/features between apps"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49434361890/Discuss+LUZ-154249+Architecture+for+shared+components+features+between+apps
space: "Helios"
topic: architecture
relevance: 0.806
depth: 2.73
updated: 2026-06-15
attachments: 1
tags:
  - confluence
  - architecture
  - space/helios
---

# [Discuss] LUZ-154249 Architecture for shared components/features between apps

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-06-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49434361890/Discuss+LUZ-154249+Architecture+for+shared+components+features+between+apps)
> Relevance 0.806 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="a9c6eda6-3af4-441c-990a-08805f7a8a2c" macro-name="toc">

</div>

## I. Goals

<div hasbody="true" macro-id="2c4a610c-213e-4537-ba17-f7c29779aa13" macro-name="info">

<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon"> </span>

<div>

- The same feature should be usable in multiple apps, like `luz-epost` and `luz-klara`.

- A feature should not depend on one app only, and it should be easy to maintain on its own.

- Each feature should own its own:

  - Pages or routes

  - UI rendering

  - UI behavior and state

  - API / data access

  - Business logic

</div>

</div>

## II. Proposal Architectures

------------------------------------------------------------------------

<div>

<table data-table-display-mode="default">
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr>
<td></td>
<td><p><strong>Architecture 1 — (Current)</strong></p>
<h3 id="id-[Discuss]LUZ-154249Architectureforsharedcomponents/featuresbetweenapps-Layer-SeparatedSharedUIArchitecture" data-local-id="e2aba0533465"><strong>Layer-Separated Shared UI Architecture</strong></h3></td>
<td><p><strong>Architecture 2 —</strong></p>
<h3 id="id-[Discuss]LUZ-154249Architectureforsharedcomponents/featuresbetweenapps-VerticalSliceLibswithRouteSegmentRe-export" data-local-id="38d907ee70e7"><strong>Vertical Slice Libs with Route Segment Re-export</strong></h3></td>
<td><p><strong>Architecture 1.5 —</strong></p>
<h3 id="id-[Discuss]LUZ-154249Architectureforsharedcomponents/featuresbetweenapps-VerticalSliceLibswithApp-OwnedRoutesAppliedthisapproachforluz-next" data-local-id="3c5e93206fd7"><strong>Vertical Slice Libs with App-Owned Routes</strong><br />
<br />

![[49434361890-check.png]]

 <span>Applied this approach for luz-next </span></h3></td>
</tr>
<tr>
<td><p><strong>Overall Pattern</strong></p></td>
<td><p>Responsibilities are separated by technical layer (not by feature).</p>
<ul>
<li><p><strong>App layer:</strong> handles routing and server actions.</p></li>
<li><p><strong>Shared UI library (theme-business):</strong> provides eArchive UI, Storybook stories, and mock data.</p></li>
<li><p>Shared service library (<code>luz-services</code>): handles API/data access.</p></li>
</ul></td>
<td><ul>
<li><p>Features are <strong>route-slice libs</strong>. Each feature exports real Next.js route segments (page.tsx, layout.tsx).</p></li>
<li><p>Apps re-export those segments directly from their file-system route folders.</p></li>
<li><p>Feature flags are checked explicitly in app layout/middleware code.</p></li>
</ul></td>
<td><ul>
<li><p>Features are <strong>vertical slice libs</strong> (libs/features/&lt;name&gt;/). Each feature owns its own UI, state, hooks, types, services, Storybook, mock data, <strong>and the</strong> <code>IService</code> <strong>contract</strong>.</p></li>
<li><p><strong>App owns routes</strong>: <code>apps/*/app/.../page.tsx</code> is a real page, not a re-export. It reads params, handles auth, checks feature flags, and renders the shell wrapped in an app-specific adapter provider.</p></li>
<li><p><strong>App injects behavior via adapter</strong>: the lib exposes <code>IEarchiveService</code> + <code>useEarchiveService()</code> hook + <code>MockEarchiveAdapter</code>. Each app implements its own adapter (<code>EpostEarchiveAdapter</code> / <code>KlaraEarchiveAdapter</code>) and provides it via context.</p></li>
<li><p><strong>No lib → app imports</strong>, ever. Shared concerns (getTenantAuth, <code>FetchBuilder</code>) stay in <code>luz-services</code>.</p></li>
</ul></td>
</tr>
<tr>
<td><p><strong>Folder Structure</strong></p></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="5901d738-b299-43fa-8ce2-c4d93923f788" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>apps/
&#10;  luz-epost/
    app/
      [locale]/
        (auth)/
          earchive/
            page.tsx                 # feature page entry
            layout.tsx               # feature layout
            actions/
              index.ts
              earchive-action.ts     # server actions for eArchive
        api/
          earchive/
            documents/
              route.ts        # BFF handler if needed (thin, calls feature lib service)
&#10;  luz-klara/
    app/
      [locale]/
        (auth)/
          earchive/
            page.tsx                 # feature page entry
            layout.tsx               # feature layout
            actions/
              index.ts
              earchive-action.ts     # server actions for eArchive
        api/
          earchive/
            documents/
              route.ts        # BFF handler if needed (thin, calls feature lib service)      
&#10;
libs/
&#10;  theme-business/
    src/
      lib/
        earchive/                    # UI-centric eArchive implementation
          earchive-shell.tsx
          earchive-shell.test.tsx
          earchive-shell.stories.tsx
          index.ts
          EARCHIVE-DEVELOPMENT.md
          _components/
          _hooks/
          _services/
          _state/
          _stores/
          _types/
          _utils/
          _mappers/
          _constants/
          _i18n/
          _models/
          _assets/
          _docs/
          _mock-data/
&#10;  luz-services/
    src/
      lib/
        letter/
          service/
            earchive-letter.service.ts   # eArchive-related backend/data access
          mapper/
          interfaces/
          utils/
        constants/
        core/
        interfaces/
        services/
  </code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="c8fb247b-ec4c-484a-81b4-7bbcb05b85a6" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>apps/
├── luz-epost/
│   └── app/
│       ├── [locale]/
│       │   └── (auth)/
│       │       └── earchive/
│       │           ├── page.tsx                     # re-export from feature route
│       │           ├── layout.tsx                   # re-export from feature route
│       │     
│       └── api/
│           └── earchive/
│               └── document/
│                   └── [documentId]/
│                       └── file/
│                           └── route.ts             # re-export from feature API route handler
│
libs/
├── features/
│   └── earchive/
        ├── src/
        │   ├── routes/
        │   │   ├── page.tsx
        │   │   ├── layout.tsx
        │   ├── ui/
        │   │   ├── earchive-shell.tsx
        │   │   ├── earchive-screen.tsx
        │   │   └── components/
        │   │       ├── earchive-header/
        │   │       ├── earchive-nav-sidebar/
        │   │       ├── company-tile/
        │   │       └── folder-tile/
        │   ├── hooks/
        │   │   ├── use-earchive-state.ts
        │   │   └── use-last-used-documents.ts
        │   ├── services/
        │   │   ├── earchive-api.service.ts
        │   │   └── earchive-mapper.service.ts
        │   ├── state/
        │   │   ├── earchive.slice.ts
        │   │   ├── earchive.selectors.ts
        │   │   └── earchive-provider.tsx
        │   ├── types/
        │   │   └── earchive.types.ts
        │   ├── constants/
        │   │   └── earchive.constants.ts
        │   ├── stories/
        │   │   ├── earchive-screen.stories.tsx
        │   │   └── earchive-shell.stories.tsx
        │   └── mock-data/
        │       ├── mock-companies.ts
        │       ├── mock-folders.ts
        │       ├── mock-documents.ts
        │       └── index.ts
        ├── index.ts
        └── project.json
│
└── luz-services/
    └── src/
        ├── lib/
        │   ├── fetch-builder.ts                     # FetchBuilder, HTTP_METHOD, FetchResponse, isErrorResponse
        │   └── server-auth/
        │       └── server-token.ts                  # shared getTenantAuth, TenantAuthContext
        └── server.ts                                # server-only barrel (@luz-next/luz-services/server)</code></pre>
</div>
</div></td>
<td><div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="79fa8ff7-ed11-4985-bd86-051ef4dfc1e8" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>apps/
├── luz-epost/
│   └── app/
│       └── [locale]/
│           └── (auth)/
│               └── earchive/
│                   ├── page.tsx          # real page — injects EpostEarchiveAdapter
│                   ├── layout.tsx        # real layout — owns metadata, error boundary
│                   └── actions/
│                       ├── index.ts
│                       └── earchive-action.ts   # implements IEarchiveService
│
├── luz-klara/
│   └── app/
│       └── [locale]/
│           └── (auth)/
│               └── earchive/
│                   ├── page.tsx          # real page — injects KlaraEarchiveAdapter
│                   ├── layout.tsx        # real layout — owns metadata, error boundary
│                   └── actions/
│                       ├── index.ts
│                       └── earchive-action.ts   # implements IEarchiveService
│
libs/
├── features/
│   └── earchive/
│       ├── src/
│       │   ├── ui/
│       │   │   ├── earchive-shell.tsx
│       │   │   ├── earchive-screen.tsx
│       │   │   └── components/
│       │   │       ├── earchive-header/
│       │   │       ├── earchive-nav-sidebar/
│       │   │       ├── company-tile/
│       │   │       └── folder-tile/
│       │   ├── services/
│       │   │   ├── earchive.service.interface.ts  # IEarchiveService contract
│       │   │   ├── earchive-service.context.tsx   # Context + useEarchiveService()
│       │   │   └── earchive-mapper.service.ts
│       │   ├── adapters/
│       │   │   └── mock-earchive.adapter.ts       # MockEarchiveAdapter
│       │   ├── hooks/
│       │   │   ├── use-earchive-state.ts
│       │   │   └── use-last-used-documents.ts
│       │   ├── state/
│       │   │   ├── earchive.slice.ts
│       │   │   ├── earchive.selectors.ts
│       │   │   └── earchive-provider.tsx
│       │   ├── types/
│       │   │   └── earchive.types.ts
│       │   ├── constants/
│       │   │   └── earchive.constants.ts
│       │   ├── stories/
│       │   │   ├── earchive-screen.stories.tsx
│       │   │   └── earchive-shell.stories.tsx
│       │   └── mock-data/
│       │       ├── mock-companies.ts
│       │       ├── mock-folders.ts
│       │       ├── mock-documents.ts
│       │       └── index.ts
│       ├── index.ts
│       └── project.json
│
└── luz-services/
    └── src/
        ├── lib/
        │   ├── fetch-builder.ts
        │   └── server-auth/
        │       └── server-token.ts       # shared getTenantAuth, TenantAuthContext
        └── server.ts</code></pre>
</div>
</div></td>
</tr>
<tr>
<td><h3 id="id-[Discuss]LUZ-154249Architectureforsharedcomponents/featuresbetweenapps-HowItWorks" data-local-id="d1a21933d634"><strong>How It Works</strong></h3></td>
<td><ol>
<li><p><strong>Shared UI Library</strong> — what <code>EarchiveShell</code> props look like, how it manages internal state, how Storybook uses mock data as props</p></li>
<li><p><strong>App Integration</strong> — how <code>luz-epost</code> wires the server action in <code>actions/earchive-action.ts</code> and passes it into the shell from <code>page.tsx</code></p></li>
<li><p><strong>Data Layer</strong> — how <code>luz-services</code> is the backend boundary; <code>theme-business</code> never calls it directly</p></li>
<li><p><strong>View Models vs API Models</strong> — the mapper pattern that keeps <code>theme-business</code> UI-only</p></li>
</ol></td>
<td><p><strong>1. Page routing</strong></p>
<ul>
<li><p>Feature route module (owned by feature):</p></li>
</ul>
<div id="expander-2085603521" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="fb95064b-c77b-4e7f-ab41-8cd92cda5410" data-macro-name="expand">
<div id="expander-control-2085603521" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">libs/features/earchive/src/routes/page.tsx</span>
</div>
<div id="expander-content-2085603521" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="456fc04c-12c3-45b7-911b-8b3fab0875b3" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>import { getTenantAuth } from &#39;@luz-next/luz-epost/service/server-token&#39;;
import { EarchiveProvider, EarchiveShell } from &#39;@luz-next/theme-business&#39;;
import { fetchLastUsedDocuments } from &#39;./actions&#39;;
&#10;export default async function EarchivePage() {
    const auth = await getTenantAuth();
&#10;    return (
        &lt;div className=&quot;w-full h-full flex flex-col overflow-hidden&quot;&gt;
            &lt;EarchiveProvider&gt;
                &lt;EarchiveShell
                    currentUser={{
                        email: auth.email,
                        username: auth.username,
                    }}
                    navTree={[]}
                    companies={[]}
                    folders={[]}
                    activities={[]}
                    fetchLastUsedDocuments={fetchLastUsedDocuments}
                /&gt;
            &lt;/EarchiveProvider&gt;
        &lt;/div&gt;
    );
}</code></pre>
</div>
</div>
</div>
</div>
<ul>
<li><p>App adapter (thin re-export):</p></li>
</ul>
<div id="expander-1136068747" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="91aed6b3-cfc9-4eeb-9e1e-0b62682c0868" data-macro-name="expand">
<div id="expander-control-1136068747" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">apps/luz-epost/app/[locale]/(auth)/earchive/page.tsx</span>
</div>
<div id="expander-content-1136068747" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="1690ab79-f084-4451-8bc1-67a530d7dabf" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>
export { default } from &#39;@luz-next/features-earchive/routes/page&#39;;</code></pre>
</div>
</div>
</div>
</div>
<p>Rule: app <code>page.tsx</code> and <code>layout.tsx</code> stay as adapters only; feature route module owns page logic.</p>
<p><br />
<strong>2. API routing</strong></p>
<ul>
<li><p>Feature route module (owned by feature):</p></li>
</ul>
<div id="expander-527760166" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="6a77fc3d-fe90-4915-a691-a2d7050016a4" data-macro-name="expand">
<div id="expander-control-527760166" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">libs/features/earchive/src/routes/api/earchive-document-file.route.ts</span>
</div>
<div id="expander-content-527760166" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a2ed5935-61e4-4e78-802a-5af5d5b438cc" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>export async function GET(...) {
  // auth + backend proxy + response handling
}</code></pre>
</div>
</div>
</div>
</div>
<ul>
<li><p>App adapter (thin re-export):</p></li>
</ul>
<div id="expander-1955646881" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="913368c6-55c6-480c-b59c-9c735eb51743" data-macro-name="expand">
<div id="expander-control-1955646881" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">apps/luz-epost/app/api/earchive/document/[documentId]/file/route.ts</span>
</div>
<div id="expander-content-1955646881" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="29203866-1ba3-4c9a-8535-ec9de410563a" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>export { GET } from &#39;@luz-next/features-earchive/routes/api/earchive-document-file.route&#39;;</code></pre>
</div>
</div>
</div>
</div>
<p>Rule: app <code>route.ts</code> files stay as adapters only; feature route modules own API handler logic.<br />
</p>
<p><strong>3. Tenant token</strong></p>
<ul>
<li><p>Feature libs always import from the shared lib (luz-services)</p></li>
</ul>
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="380b243b-4ac5-41af-a2e6-9358588587d2" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>// ✅ lib → lib (correct)
import { getTenantAuth } from &#39;@luz-next/luz-services/server&#39;;</code></pre>
</div>
</div>
<ul>
<li><p>App-level server-token files</p></li>
</ul>
<p>Each app keeps its own <code>service/server-token.ts</code>. Both re-export <code>getTenantAuth</code> from the shared lib and add only app-specific extras on top.</p>
<p>---</p>
<p>Source code tryout: <a href="https://bitbucket.org/axonivy-prod/luz_next/src/463986055ac3edfd7b517e0ad1b07c7eaeb9b2e2/?at=unified-inbox%2FLUZ-154249%2FArchitecture-2-Vertical-Slice-Libs-with-Route-Segment-Re-export" class="external-link" data-card-appearance="inline" data-local-id="55991584fdee" rel="nofollow">https://bitbucket.org/axonivy-prod/luz_next/src/463986055ac3edfd7b517e0ad1b07c7eaeb9b2e2/?at=unified-inbox%2FLUZ-154249%2FArchitecture-2-Vertical-Slice-Libs-with-Route-Segment-Re-export</a></p></td>
<td><ol>
<li><p><strong>Service Contract</strong> — <code>IEarchiveService</code> is defined in the feature lib. Shell calls <code>useEarchiveService()</code> to get data — it never receives <code>fetch*</code> props directly.</p></li>
<li><p><strong>App Integration</strong> — <code>luz-epost</code> implements <code>EpostEarchiveAdapter</code> in <code>actions/earchive-action.ts</code>, then page.tsx provides it via <code>EarchiveServiceContext</code>.</p></li>
<li><p><strong>Mock Adapter</strong> — <code>MockEarchiveAdapter</code> ships in the feature lib. Storybook uses it directly — no app dependency needed.</p></li>
<li><p><strong>Dependency direction</strong> — <code>feature lib → luz-services</code>. App <code>→ feature lib</code>. App <code>→ luz-services</code>. Never the reverse.</p></li>
</ol></td>
</tr>
<tr>
<td><p><strong>Pros &amp; Cons</strong></p></td>
<td><p><strong>Pros:</strong></p>
<p>✅ <strong>Designer-friendly:</strong> UI is in theme-business with Storybook and mock data, enabling designers to iterate independently of backend.</p>
<p>✅ <strong>Clear technical split:</strong> app manages routing, theme-business manages UI, and <code>luz-services</code> manages API/data access.</p>
<p>✅ <strong>Good UI reuse:</strong> shared UI components are reusable across screens and apps.</p>
<p><strong>Cons:</strong></p>
<p>⚠️ <strong>Feature is fragmented</strong>: eArchive is spread across app routes, theme-business, and <code>luz-services</code>, so it is harder to understand end to end.</p>
<p>⚠️ <strong>Weak ownership</strong>: no single module owns the whole feature, which makes team boundaries less clear.</p>
<p>⚠️<strong>Cross-app reuse is harder</strong>: another app can reuse the UI, but still needs extra route and integration wiring.</p>
<p>⚠️ <strong>Less scalable long-term</strong>: as more features follow the same pattern, shared libs can become crowded and harder to maintain.</p></td>
<td><p><strong>Pros:</strong></p>
<p>✅ <strong>More modular than current architecture</strong>: eArchive becomes one feature lib instead of being split across app, UI lib, and services lib.</p>
<p>✅ <strong>Better ownership</strong>: one team can own the feature end-to-end, including routes, UI, state, and services.</p>
<p>✅ <strong>Good reuse across apps</strong>: the same feature lib can be re-exported in <code>luz-epost</code> and <code>luz-klara</code>.</p>
<p>✅ <strong>Cleaner boundary</strong>: business logic stays inside the feature instead of leaking into shared libs.</p>
<p><strong>Cons:</strong></p>
<p>⚠️ <strong>More structure to maintain</strong>: every feature needs its own lib, routes, services, state, and stories.</p>
<p>⚠️ <strong>Some route duplication still exists</strong>: each app still needs thin re-export files for pages, layouts, and API routes.</p>
<p>⚠️ <strong>Harder to customize business logic</strong> by app context</p></td>
<td><p>✅ <strong>Single-owner feature lib</strong>: one team owns the feature end-to-end — UI, state, hooks, types, service contract, Storybook, mock data.</p>
<p>✅ <strong>Per-app routing freedom</strong>: page.tsx and layout.tsx are real pages in each app — full control over <code>generateMetadata</code>, error boundaries, loading UI, feature flag checks.</p>
<p>✅ <strong>Type-safe cross-app contract</strong>: <code>IEarchiveService</code> is compiler-enforced. If <code>luz-klara</code> misses a method, it fails at build time — not silently at runtime.</p>
<p>✅ <strong>Designer workflow preserved</strong>: <code>MockEarchiveAdapter</code> ships in the feature lib. Storybook works without any app dependency.</p>
<p>✅ <strong>Cleanest dependency direction</strong>: feature lib never imports from app. App imports from feature lib only.</p>
<p>✅ <strong>Per-app customization via adapter</strong>, not branching inside the lib.</p>
<p><strong>Cons:</strong></p>
<p>⚠️ <strong>Adapter boilerplate per app</strong>: each app must implement <code>IEarchiveService</code> and provide it via context — unavoidable but structured.</p>
<p>⚠️ <code>IService</code> <strong>contract is a coupling point</strong>: adding a new method to <code>IEarchiveService</code> requires updating all app adapters at the same time.</p>
<p>⚠️ <strong>More lib structure to maintain</strong>: every feature needs its own lib, project.json, path alias, and barrel file.</p>
<p>⚠️ <strong>Route-level code still exists in each app</strong>: page.tsx and layout.tsx are not shared — intentional, but still requires per-app files.</p></td>
</tr>
<tr>
<td><p><strong>Suggestions for Next steps</strong></p></td>
<td><p><strong>Feature teams (communities, smartsend, unified-inbox):</strong></p>
<ul>
<li><p>Define <code>I&lt;Feature&gt;Service</code> contract</p></li>
<li><p>Create React Context + <code>use&lt;Feature&gt;Service()</code> hook</p></li>
<li><p>Create <code>Mock&lt;Feature&gt;Adapter</code> covering all UI states</p></li>
<li><p>Refactor feature shell to read data from context instead of props</p></li>
<li><p>Implement <code>Epost&lt;Feature&gt;Adapter</code> with real server actions</p></li>
<li><p>Update page.tsx to wrap shell with adapter</p></li>
</ul></td>
<td><p><strong>Feature teams (communities, smartsend, unified-inbox):</strong></p>
<ul>
<li><p>Create libs/features/&lt;feature-name&gt;/ following the agreed folder structure</p></li>
<li><p>Move feature code into the lib: <code>routes/</code>, <code>ui/</code>, <code>services/</code>, <code>state/</code>, <code>hooks/</code>, <code>types/</code>, <code>constants/</code>, <code>mock-data/</code></p></li>
<li><p>Move feature-specific API service out of <code>luz-services</code> into the feature lib</p></li>
<li><p>Replace <code>apps/luz-epost/app/.../page.tsx</code> with 1-line re-export</p></li>
<li><p>Update <code>project.json</code> and tsconfig.base.json path alias</p></li>
</ul></td>
<td><p><strong>Feature teams (communities, smartsend, unified-inbox):</strong></p>
<ul>
<li><p>Create libs/features/&lt;feature-name&gt;/ following the agreed folder structure</p></li>
<li><p>Move feature code into the lib: <code>ui/</code>, <code>services/</code>, state/, <code>hooks/</code>, <code>types/</code>, <code>constants/</code>, mock-data/</p></li>
<li><p>Define <code>I&lt;Feature&gt;Service</code> contract in the lib</p></li>
<li><p>Create <code>&lt;Feature&gt;ServiceContext</code> + <code>use&lt;Feature&gt;Service()</code> hook</p></li>
<li><p>Create <code>Mock&lt;Feature&gt;Adapter</code> covering all UI states</p></li>
<li><p>Refactor feature shell to call <code>use&lt;Feature&gt;Service()</code> instead of receiving <code>fetch*</code> props</p></li>
<li><p>Move feature-specific API service out of <code>luz-services</code> into the feature lib</p></li>
<li><p>Implement <code>Epost&lt;Feature&gt;Adapter</code> in luz-epost with real server actions</p></li>
<li><p>Update page.tsx in each app to provide adapter via context</p></li>
<li><p>Update <code>project.json</code> and tsconfig.base.json path alias</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

------------------------------------------------------------------------
