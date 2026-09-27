---
ai_hash: da9731043825a1cc
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-14
entities:
- luz-docs-import
- Permission denied
- zip4j
- archive file permissions
- stored POSIX mode
- FileUtil.extractAllZipFile
- zipFile.extractAll
- Windows-made zip
- owner-read permission
- jboss WildFly user
- FileInputStream
- EACCES
- processDocumentFile
- import-test-zips/21-edge-cp437-names.zip
- file_correct_name/Abschaltung von E-Post Office und.pdf
- permission normalization
- Files.walkFileTree
- SimpleFileVisitor
- preVisitDirectory
- visitFile
- FileUtil.readAllFileInFolder
- listFiles()
- NullPointerException (NPE)
- INTERNAL_SERVICE_ERROR
- 0o000 directory
- Unix mode
- zip4j extractAll applies a ZIP entry's stored Unix mode
- Windows-made zips can extract unreadable files
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
- luz-docs-import — *HAS_FAILURE_TYPE* — Permission denied
- Permission denied — *TRACES_TO* — zip4j
- zip4j — *APPLIES* — archive file permissions
- zip4j — *APPLIES* — stored POSIX mode
- FileUtil.extractAllZipFile — *IS_FUNCTION_OF* — zip4j
- zipFile.extractAll — *IS_FUNCTION_OF* — zip4j
- Windows-made zip — *LACKS* — owner-read permission
- owner-read permission — *IS_REQUIRED_BY* — jboss WildFly user
- jboss WildFly user — *CANNOT_READ_FILE_CAUSES* — FileInputStream
- FileInputStream — *THROWS* — EACCES
- EACCES — *CAUSES* — processDocumentFile
- processDocumentFile — *TO_RECORD_FAILURE* — file_correct_name/Abschaltung von E-Post Office und.pdf
- import-test-zips/21-edge-cp437-names.zip — *IS_REPRO_CASE_FOR* — Permission denied
- import-test-zips/21-edge-cp437-names.zip — *CONTAINS* — file_correct_name/Abschaltung von E-Post Office und.pdf
- file_correct_name/Abschaltung von E-Post Office und.pdf — *FAILS_WITH* — Permission denied
- permission normalization — *IS_FIX_FOR* — Permission denied
- permission normalization — *OCCURS_AFTER* — zipFile.extractAll
- permission normalization — *OCCURS_BEFORE* — FileUtil.readAllFileInFolder
- Files.walkFileTree — *IS_USED_FOR* — permission normalization
- SimpleFileVisitor — *IS_USED_WITH* — Files.walkFileTree
- preVisitDirectory — *SETS* — directory permissions
- visitFile — *SETS* — file permissions
- FileUtil.readAllFileInFolder — *USES* — listFiles()
- 0o000 directory — *CAUSES* — listFiles() TO_RETURN null
- listFiles() TO_RETURN null — *CAUSES* — NullPointerException (NPE)
- NullPointerException (NPE) — *CAUSES* — INTERNAL_SERVICE_ERROR
- permission normalization — *PREVENTS* — NullPointerException (NPE)
- zip4j extractAll applies a ZIP entry's stored Unix mode — *IS_RELATED_TO* — Permission denied
- Windows-made zips can extract unreadable files — *IS_RELATED_TO* — Permission denied
- zip4j extractAll applies a ZIP entry's stored Unix mode — *DESCRIBES* — underlying mechanism
- Windows-made zip — *CAN_EXTRACT* — unreadable files

%% ai-graph-end %%