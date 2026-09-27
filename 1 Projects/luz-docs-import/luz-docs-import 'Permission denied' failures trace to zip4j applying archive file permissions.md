---
ai_hash: 1bd173777de0015e
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-14
entities:
- luz-docs-import
- Permission denied
- zip4j
- archive file permissions
- FileUtil.extractAllZipFile
- zipFile.extractAll
- Windows-made zip
- POSIX mode
- owner-read
- jboss WildFly user
- FileInputStream
- EACCES
- processDocumentFile
- data/Lam/import-test-zips/import-test-zips/21-edge-cp437-names.zip
- file_correct_name/Abschaltung von E-Post Office und.pdf
- permission normalization
- Files.walkFileTree
- SimpleFileVisitor
- FileUtil.readAllFileInFolder
- folder.listFiles()
- NPE
- INTERNAL_SERVICE_ERROR
- 0o000 directory
- null guard
- Unix mode
- mount
- filename
- name encoding
- high-word mode
source: session 2026-08-14 case 21 import test
status: seedling
tags:
- luz-docs-import
- zip4j
- import
- bug
- permissions
title: luz-docs-import 'Permission denied' failures trace to zip4j applying archive
  file permissions
type: lesson
---

# luz-docs-import 'Permission denied' failures trace to zip4j applying archive file permissions

When a `luz-docs-import` ZIP import reports a file as **failed** with detail `<extracted-path> (Permission denied)`, the cause is **not** the mount, the filename, or the name encoding — it is zip4j applying the archive entry's stored POSIX mode during `FileUtil.extractAllZipFile` (`zipFile.extractAll`). A Windows-made zip whose entry carries a high-word mode lacking owner-read (e.g. `----w----`) extracts a file the non-root `jboss` WildFly user cannot read → `FileInputStream` throws EACCES → `processDocumentFile` records it as failed. See [[zip4j extractAll applies a ZIP entry's stored Unix mode, so Windows-made zips can extract unreadable files]] for the underlying mechanism.

**Repro:** `data/Lam/import-test-zips/import-test-zips/21-edge-cp437-names.zip` (name is misleading — the entries are plain ASCII; the trigger is the external attrs). Single file `file_correct_name/Abschaltung von E-Post Office und.pdf` fails while the folder lists fine.

**Fix:** after `extractAll`, normalize permissions before `readAllFileInFolder`:
```java
zipFile.extractAll(folderPath);
Files.walkFileTree(Paths.get(folderPath), new SimpleFileVisitor<>() {
  public FileVisitResult preVisitDirectory(Path d, BasicFileAttributes a){ d.toFile().setReadable(true,true); d.toFile().setExecutable(true,true); return CONTINUE; }
  public FileVisitResult visitFile(Path f, BasicFileAttributes a){ f.toFile().setReadable(true,true); return CONTINUE; }
});
```
Set dir perms in `preVisitDirectory` (before descent) so the walk can enter an otherwise-`0o000` dir; the owner can always chmod its own files. Portable — no-ops on Windows local dev.

**Latent second bug uncovered:** `FileUtil.readAllFileInFolder` does `for (File e : folder.listFiles())` with no null-check. A zip that extracts a `0o000` **directory** makes `listFiles()` return `null` → NPE → whole job fails as `INTERNAL_SERVICE_ERROR`. The same permission-normalization fix prevents it; add a null guard too.

## Related

- [[zip4j extractAll applies a ZIP entry's stored Unix mode, so Windows-made zips can extract unreadable files]]

%% ai-graph-start %%

**Related notes:**
- [[zip4j extractAll applies a ZIP entry's stored Unix mode, so Windows-made zips can extract unreadable files]]
- [[zip4j 2.8.0 ZipException is-a IOException, and extractFile keeps Zip-Slip protection (per-entry extraction)]]
- [[luz-docs-import Gap-3 always-UTF-8 ZIP decode is deliberate; CP437 non-flagged zips are accepted best-effort]]
- [[luz_docs_import isExcludedFile misses Thumbs.db and __MACOSX (BR-04 gap)]]
- [[Fail an import on a corrupt ZIP by translating the extraction exception to a domain FailureCode]]

**Relations:**
- luz-docs-import — *reports failure with* — Permission denied
- Permission denied — *traced to* — zip4j
- zip4j — *applies* — archive file permissions
- archive file permissions — *include* — POSIX mode
- archive file permissions — *include* — Unix mode
- zip4j — *uses* — FileUtil.extractAllZipFile
- zip4j — *uses* — zipFile.extractAll
- Windows-made zip — *can store* — high-word mode
- high-word mode — *can lack* — owner-read
- owner-read — *is lacking for* — jboss WildFly user
- jboss WildFly user — *cannot read* — file_correct_name/Abschaltung von E-Post Office und.pdf
- FileInputStream — *throws* — EACCES
- EACCES — *results in* — processDocumentFile recording failure
- processDocumentFile — *records failure for* — file_correct_name/Abschaltung von E-Post Office und.pdf
- data/Lam/import-test-zips/import-test-zips/21-edge-cp437-names.zip — *is a repro case for* — Permission denied
- data/Lam/import-test-zips/import-test-zips/21-edge-cp437-names.zip — *contains* — file_correct_name/Abschaltung von E-Post Office und.pdf
- file_correct_name/Abschaltung von E-Post Office und.pdf — *fails with* — Permission denied
- Fix — *is* — permission normalization
- permission normalization — *uses* — Files.walkFileTree
- Files.walkFileTree — *implements* — SimpleFileVisitor
- permission normalization — *occurs after* — zipFile.extractAll
- permission normalization — *occurs before* — FileUtil.readAllFileInFolder
- FileUtil.readAllFileInFolder — *has bug with* — folder.listFiles()
- 0o000 directory — *causes* — folder.listFiles() to return null
- folder.listFiles() to return null — *leads to* — NPE
- NPE — *causes* — INTERNAL_SERVICE_ERROR
- permission normalization — *prevents* — NPE
- null guard — *prevents* — NPE
- zip4j extractAll applies a ZIP entry's stored Unix mode, so Windows-made zips can extract unreadable files — *explains* — underlying mechanism
- Permission denied — *is not caused by* — mount
- Permission denied — *is not caused by* — filename
- Permission denied — *is not caused by* — name encoding

%% ai-graph-end %%