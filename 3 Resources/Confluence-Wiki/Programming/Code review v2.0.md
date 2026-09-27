---
ai_hash: 9489ffa4f0b34766
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.852
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48935010546/Code+review+v2.0
space: TS
status: reference
tags:
- confluence
- programming
- space/ts
title: Code review v2.0
topic: programming
type: source
updated: 2026-07-09
---

# Code review v2.0

> [!info] Imported from Confluence
> Space **TS** · updated 2026-07-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48935010546/Code+review+v2.0)
> Relevance 0.852 · topic `programming`

### 1. Clean code

- Error handling (check NPE, try/catch, custom exceptions)

- Remove unnecessary comments, console logs

- Unit tests cover new code

- No side effects from the changes

- No duplicated code

- No "Magic Numbers" or hardcoded strings (use Constants/Enums)

#### 2. Pull request

- All tests are passed

- Include a concise summary of changes

- Attach the screenshots of your work

- Break large features into smaller ones

#### 3. Performance & Database (Java)

- No N + 1 issue?

- Are the critical query columns indexed?

- Is `@Transactional` applied correctly (readOnly vs readWrite)?

- Lazy loading, asynchronous, and parallel processing

- Caching and session/application data

- Pagination implemented for list endpoints (prevent fetching 1000s of rows)

- [\[Performance\] Database Migration](https://axonivy.atlassian.net/wiki/spaces/LUZ/blog/2020/05/18/20508023276/Performance+Database+Migration)

#### 4. React & Next.js Code

- Image Optimization: Use `next/image` and specify width/height.

- Default to Server Components. Only use "use client" for interactivity.

- Fetch data on the server to reduce client bundles.<span class="inline-comment-marker" ref="56b1b8b1-55c7-469c-bef2-821afb11992b"> Use </span><span class="inline-comment-marker" ref="56b1b8b1-55c7-469c-bef2-821afb11992b">`Promise.all`</span>.

- <span class="inline-comment-marker" ref="7383a619-18d5-4726-9b77-f1a4516aed85">Avoid </span><span class="inline-comment-marker" ref="7383a619-18d5-4726-9b77-f1a4516aed85">`useEffect`</span><span class="inline-comment-marker" ref="7383a619-18d5-4726-9b77-f1a4516aed85"> without dependencies</span>.

- Lists: Ensure `key` prop is unique and not an array index.

- Use Memoization (useMemo/useCallback) *only if an expensive calculation is proven*.

- Apply Lazy-Loading (`next/dynamic`) where required.

- <span class="inline-comment-marker" ref="5c7c7d07-b273-4b61-8955-79db93753aa5">Avoid direct DOM access (</span><span class="inline-comment-marker" ref="5c7c7d07-b273-4b61-8955-79db93753aa5">`document.getElementById`</span><span class="inline-comment-marker" ref="5c7c7d07-b273-4b61-8955-79db93753aa5">)</span>.

- Accessibility: Check `alt` tags and `aria-labels`**.**

- Type Safety: No usage of `any` type.

- Check that secrets are not prefixed with `NEXT_PUBLIC_`.

%% ai-graph-start %%

**Related notes:**
- [[Prompt Performance Code Review]]
- [[Prompt Architecture Code Review]]
- [[Test and code review report template.2.93]]
- [[Test and code review report template]]
- [[00. Test and code review report template]]

%% ai-graph-end %%