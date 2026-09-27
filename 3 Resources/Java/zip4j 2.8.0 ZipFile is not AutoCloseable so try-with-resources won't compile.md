---
ai_hash: aac1118af969887e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: luz_docs_import LUZ-158230 · 2026-08-11
status: seedling
tags:
- java
- zip4j
- gotcha
- maven
title: zip4j 2.8.0 ZipFile is not AutoCloseable so try-with-resources won't compile
type: lesson
---

# zip4j 2.8.0 ZipFile is not AutoCloseable so try-with-resources won't compile

In zip4j **2.8.0**, `net.lingala.zip4j.ZipFile` does **not** implement `Closeable`/`AutoCloseable`, so wrapping it in a try-with-resources (`try (ZipFile z = new ZipFile(path)) {…}`) fails to compile with *"cannot be converted to java.lang.AutoCloseable"*.

Use an explicit reference plus a `try/finally` if you need cleanup — or skip closing entirely (the extraction itself opens/closes per-entry streams; the `ZipFile` object mainly holds metadata). The important half of hardening extraction is not-swallowing the failure, not the close.

**Gotcha within the gotcha:** finding the jar in `.m2` or assuming a class implements an interface is not proof — a `mvn compile` is. Verify capabilities against the actual compile classpath, not by reading the dependency.

Related: [[Fail an import on a corrupt ZIP by translating the extraction exception to a domain FailureCode]]

## Related

- [[Fail an import on a corrupt ZIP by translating the extraction exception to a domain FailureCode]]

%% ai-graph-start %%

**Related notes:**
- [[Fail an import on a corrupt ZIP by translating the extraction exception to a domain FailureCode]]
- [[zip4j 2.8.0 ZipException is-a IOException, and extractFile keeps Zip-Slip protection (per-entry extraction)]]
- [[zip4j extractAll applies a ZIP entry's stored Unix mode, so Windows-made zips can extract unreadable files]]
- [[luz-docs-import 'Permission denied' failures trace to zip4j applying archive file permissions]]
- [[try-with-resources surfaces the checked exception of close() even when the block body throws nothing]]

%% ai-graph-end %%