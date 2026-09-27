---
title: "Share features as vertical slices with app-owned routes and an injected adapter"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: LUZ-154249 Architecture for shared components between apps (Helios)"
tags: [architecture, monorepo, nextjs, vertical-slice, dependency-inversion, confluence-distilled]
---

# Share features as vertical slices with app-owned routes and an injected adapter

When the same feature must run inside two apps, the instinct is to extract shared code **by technical layer** — a UI library, a services library, routing left in the apps. That splits one feature across three packages, so every change to it touches all three and neither app can vary its behaviour without a flag inside the shared code.

A three-way comparison for sharing an eArchive feature between `luz-epost` and `luz-klara`:

**Architecture 1 (the existing one) — separated by layer.** App layer owns routing and server actions; a shared UI library owns rendering, Storybook and mock data; a shared service library owns API access. Cohesive per layer, incoherent per feature.

**Architecture 2 — vertical slices that export routes.** Each feature lib exports real Next.js route segments (`page.tsx`, `layout.tsx`) and the apps **re-export** them from their route folders. Feature flags get checked in app layout/middleware. Maximum sharing — but the app no longer controls its own route entry point.

**Architecture 1.5 — vertical slices, app-owned routes.** The one adopted.

- **The feature lib owns everything about the feature**: UI, state, hooks, types, services, Storybook, mock data, and the `IService` contract — `libs/features/<name>/`.
- **The app owns the route.** `apps/*/app/.../page.tsx` is a *real page*, not a re-export. It reads params, handles auth, checks feature flags, and renders the feature's shell inside an app-specific provider.
- **The app injects behaviour through an adapter.** The lib exposes `IEarchiveService`, a `useEarchiveService()` hook, and a `MockEarchiveAdapter`; each app implements `EpostEarchiveAdapter` / `KlaraEarchiveAdapter` and supplies it via context.
- **One hard rule: no lib → app imports, ever.** Shared cross-cutting concerns (`getTenantAuth`, `FetchBuilder`) live in a services library, never reached upward from a lib.

**Why the middle option wins.** Routing is where the things that genuinely *differ per app* live — authentication, feature flags, URL shape, params. Re-exported routes force those differences back into the shared lib as conditionals. App-owned routes keep the variation where it belongs and leave the lib ignorant of which app is hosting it.

The adapter is **dependency inversion**: the lib declares what it needs (`IService`) and the app decides how it is satisfied. That is what makes two apps able to share one feature with different backends, and it is what makes the lib testable — `MockEarchiveAdapter` ships with it, so Storybook and tests need no app at all.

> [!tip] The rule that keeps it honest
> *"No lib → app imports, ever."* Dependencies must point one way. The moment a lib imports from an app, the lib is no longer shareable and the whole structure quietly reverts to a monolith with extra folders. Enforce it with a lint rule or module boundaries, not a convention.

> [!warning] Vertical slices duplicate a little on purpose
> Two features each owning their own hooks and types will repeat some code that a layer-based split would have shared once. That is the trade being bought: a feature you can change, test and delete in one place, at the cost of some duplication between features. If you find yourself extracting a "common" layer back out of the slices, you are on the way back to Architecture 1.

Source: [[Discuss LUZ-154249 Architecture for shared components features between apps]] (Helios, Confluence).

## Related

- [[Strike what every option shares to find the real architecture decision]]
