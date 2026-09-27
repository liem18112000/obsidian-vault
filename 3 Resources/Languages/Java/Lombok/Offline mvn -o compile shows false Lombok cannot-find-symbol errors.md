---
ai_hash: 6ce3a9d64fa5a565
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-20
entities: []
source: session 2026-08-20 code review of luz_docs_import k6-load-test branch
status: seedling
tags:
- lombok
- maven
- java
- build
- gotcha
- luz-docs-import
title: Offline mvn -o compile shows false Lombok cannot-find-symbol errors
type: lesson
---

# Offline mvn -o compile shows false Lombok cannot-find-symbol errors

Running `mvn -o compile` (offline) on a Lombok + Maven project reports a flood of FALSE `cannot find symbol` errors for every Lombok-generated member — `getX()`/`setX()`/`builder()` on `@Getter`/`@Setter`/`@Builder` types — because offline mode skips Lombok annotation processing. Those same errors would appear against a clean `master`, so they are noise, not defects introduced by your diff.

When compile-verifying a diff on such a repo (e.g. luz_docs_import):

- Prefer ONLINE `mvn clean compile` so annotation processing actually runs.
- If you only have the offline output, treat as REAL only the Lombok-INDEPENDENT errors: an undefined local/field (e.g. `cannot find symbol: variable store`), a constructor/argument type mismatch, a duplicate method, a bad import. Discard anything shaped like a missing generated accessor/builder.

Example: a JobProgressWriter refactor renamed a record component `store` -> `publisher` but left `store.updateJob(...)` behind. Offline compile buried `cannot find symbol: variable store` (the real break) among dozens of phantom `getSuccessfulFiles()`/`builder()` errors; filtering to the Lombok-independent one confirmed the genuine compile failure.

Related: the phantom errors also cascade the way a single bad symbol does — see the cascade note.

## Related

- [[Lombok one bad symbol cascades into hundreds of phantom missing-method errors]]
- [[luz_docs_import targets Java 17 (javax stack]]
- [[modern idioms allowed)]]

%% ai-graph-start %%

**Related notes:**
- [[Lombok one bad symbol cascades into hundreds of phantom missing-method errors]]
- [[Verify wildcard-to-explicit import cleanup by compiling]]
- [[Run mvn test-compile after changing a recordctor signature — Cloud Build compiles tests, local mvn compile does not]]
- [[A refactor that removes a method must grep tests for its name before merging]]
- [[luz_finance and luz_components move in lockstep SNAPSHOTs; a 'method not applicable' compile error usually means a skew]]

%% ai-graph-end %%