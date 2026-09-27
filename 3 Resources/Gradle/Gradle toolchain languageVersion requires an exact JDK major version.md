---
ai_hash: 8938f61884ae93e9
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-24
entities: []
source: session 2026-08-24 ecomart-java analysis
status: seedling
tags:
- gradle
- java
- toolchain
- gotcha
- build
title: Gradle toolchain languageVersion requires an exact JDK major version
type: lesson
---

# Gradle toolchain languageVersion requires an exact JDK major version

A Gradle `java { toolchain { languageVersion.set(JavaLanguageVersion.of(18)) } }` block requires an **exactly-matching** JDK (major version 18) to be installed, or a toolchain download repository configured — it does **not** mean "18 or higher".

If only other JDKs are present (e.g. 17, 19, 25), the build fails at *toolchain resolution*, before compiling anything:

```
No matching toolchains found for requested specification:
{languageVersion=18, vendor=any, implementation=vendor-specific}
No locally installed toolchains match and toolchain download repositories have not been configured.
```

**Fixes:** install the exact JDK; add the foojay `org.gradle.toolchains.foojay-resolver-convention` plugin so Gradle auto-downloads the right JDK; or relax the pinned version.

**Gotcha to watch:** version drift between the README ("Java 18 or higher"), the toolchain pin (exactly 18), and CI `actions/setup-java` (which may install a *different* version like 17). All three should agree, or the build is not reproducible.

## Related

- [[Verify gradle-wrapper.jar integrity before running gradlew]]

%% ai-graph-start %%

**Related notes:**
- [[gradlew wrapper upgrades run under the OLD Gradle version - pick the JDK accordingly]]
- [[Java 25 requires Gradle 9.1.0 or later, not Gradle 9.0.0]]
- [[Verify gradle-wrapper.jar integrity before running gradlew]]
- [[String-typed org.gradle.jvm.environment attribute collides with Gradle 7+ typed TargetJvmEnvironment]]
- [[Guava jreandroid variant ambiguity declare TargetJvmEnvironment standard-jvm]]

%% ai-graph-end %%