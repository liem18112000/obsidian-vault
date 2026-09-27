---
title: "Undrained fetch response bodies leak sockets in Node undici"
created: 2026-09-27
type: gotcha
status: seedling
source: "Confluence: await fetch() vs FetchBuilder (TS)"
tags: [nodejs, fetch, undici, memory-leak, error-handling, confluence-distilled]
---

# Undrained fetch response bodies leak sockets in Node undici

In Node's `undici` (the engine behind global `fetch`), a response body is a `ReadableStream` backed by a live socket. Until that body is **drained or explicitly cancelled**, undici cannot:

- return the socket to the keep-alive connection pool,
- free the body chunks already pulled off the wire,
- mark the `Response` eligible for garbage collection.

So the most natural-looking error path in the world leaks:

```js
const response = await fetch(url);
if (!response.ok) {
  logger.warn(`Failed: ${response.statusText}`);   // ❌ body never read
  return null;
}
return response.json();
```

`response.statusText` is a **header** property — reading it does not touch the body. The happy path calls `.json()` and drains correctly; the error path returns without ever consuming the stream, pinning a socket every time. Under load this outruns GC, and you exhaust the connection pool during exactly the incident that is generating the errors. The failure mode is self-amplifying: more upstream errors → more leaked sockets → more timeouts.

**The fix is to always consume the body, including on failure:**

```js
const response = await fetch(url);
if (!response.ok) {
  const text = await response.text();      // drain it (even if discarded)
  logger.warn(`Failed ${response.status}: ${text.slice(0, 200)}`);
  return null;
}
return response.json();
```

`await response.body?.cancel()` works too when you genuinely do not want the bytes, and is cheaper for large error payloads.

> [!tip] It also makes your logs better
> The leaky version logs `statusText` — usually just `"Bad Request"`. Draining gives you the actual error body, which is where the server explained what was wrong. Correctness and debuggability point the same way here.

> [!warning] Early returns and thrown exceptions are the usual culprits
> Any path that exits between `await fetch(...)` and reading the body leaks: guard clauses, `if (!ok) throw`, a validation check on a header, an early `return` in a `switch`. Audit for `fetch(` followed by a branch that does not consume the body — a linting rule or a small wrapper that drains on every non-OK response is more reliable than remembering.

Related: [[Next.js monkey-patches global fetch, and its response clone races your body read]] — a second, Next-specific hazard on the same API.

Source: [[await fetch() vs FetchBuilder ()]] (TS, Confluence).

## Related

- [[Next.js monkey-patches global fetch, and its response clone races your body read]]
