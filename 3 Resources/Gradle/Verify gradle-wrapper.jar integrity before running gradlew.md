---
ai_hash: 176ab05f2183d888
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24 ecomart-java analysis
status: seedling
tags:
- gradle
- security
- supply-chain
- wrapper
title: Verify gradle-wrapper.jar integrity before running gradlew
type: lesson
---

# Verify gradle-wrapper.jar integrity before running gradlew

`gradle-wrapper.jar` runs with **your** privileges the first time anyone executes `./gradlew`, so a tampered wrapper jar is a classic supply-chain foothold. Verify it before trusting an unfamiliar repo.

- Confirm the size/checksum against a known-good release. The official **Gradle 8.3** wrapper jar is **61,608 bytes**, SHA-256 `91941f522fbfd4431cf57e445fc3d5200c85f957bda2de5251353cf11174f4b5`.
- `sha256sum gradle/wrapper/gradle-wrapper.jar` and compare.
- Check `gradle/wrapper/gradle-wrapper.properties` → `distributionUrl` points at the official `https://services.gradle.org` over HTTPS.

**Hardening:** add a `distributionSha256Sum=<hash>` line to `gradle-wrapper.properties` so the wrapper refuses to run a distribution whose hash doesn't match. GitHub's `gradle/wrapper-validation-action` automates this check in CI.

Note: a `gradlew`/`gradlew.bat` showing as "modified" in git is often just CRLF↔LF line-ending churn — diff the content before assuming an injected payload.

## Related

- [[Gradle toolchain languageVersion requires an exact JDK major version]]

%% ai-graph-start %%

**Related notes:**
- [[Gradle toolchain languageVersion requires an exact JDK major version]]
- [[gradlew wrapper upgrades run under the OLD Gradle version - pick the JDK accordingly]]
- [[Check git check-ignore -v when adding a Gradle wrapper to a legacy repo]]
- [[Give the gradlew distribution download its own retried Docker layer]]
- [[Baseline-diff gates must compare post-build to post-build when artifacts are committed]]

%% ai-graph-end %%