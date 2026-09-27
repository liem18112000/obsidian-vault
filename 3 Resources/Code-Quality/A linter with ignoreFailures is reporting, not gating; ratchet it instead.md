---
title: "A linter with ignoreFailures is reporting, not gating; ratchet it instead"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Static analysis tool for Android (Helios)"
tags: [static-analysis, gradle, checkstyle, ci, technical-debt, confluence-distilled]
---

# A linter with ignoreFailures is reporting, not gating; ratchet it instead

Wiring Checkstyle, FindBugs, PMD and Lint into the build and CI feels like adding a quality gate. Look at the task configuration before believing it:

```groovy
task checkstyle(type: Checkstyle) {
    ignoreFailures = true          // ← the build can never fail on this
    configFile file("${project.rootDir}/config/quality/checkstyle/checkstyle.xml")
    configProperties.checkstyleSuppressionsPath =
        file("${project.rootDir}/config/quality/checkstyle/suppressions.xml").absolutePath
    source 'src'
    include '**/*.java'
    exclude '**/gen/**'
}
```

With `ignoreFailures = true`, every violation is **reported and ignored**. The tool is integrated, CI is green, and the count of violations is free to grow forever. That is a **reporting** setup, not a gate — and the difference matters, because the team believes it has the second.

**`ignoreFailures = true` is the right *starting* position.** Turning a linter on against an existing codebase with failures enabled means an immediate red build and thousands of violations nobody can triage. Starting permissive is how you get the tool in at all. The failure is stopping there.

**The ratchet that actually improves things:**

1. Start with `ignoreFailures = true`; record the **current violation count** as a baseline.
2. Fail the build when the count **rises above the baseline** — new code is held to the standard, existing code is not.
3. Lower the baseline whenever it drops. It can never go up.
4. Once it reaches zero, switch to `ignoreFailures = false` and delete the baseline.

This gets the value without the flag day, and — crucially — makes progress **monotonic**. Without step 2, permissiveness is indistinguishable from having no tool.

> [!tip] Check the suppressions file alongside the config
> `suppressions.xml` is where rules quietly get switched off per file or per pattern. A config that looks strict plus a suppressions file that exempts half the tree is the same non-gate wearing better clothes. Review the two together, and treat growth in suppressions as growth in violations.

> [!warning] Four overlapping tools is three chances to be ignored
> Checkstyle, FindBugs, PMD and Lint overlap substantially. Running all four permissively produces four reports nobody reads. One tool that can fail the build beats four that cannot.

Related: [[Fallow analyses a JS-TS repo as a graph and exposes it to agents over MCP]] — a different analysis shape, same question: does it gate anything?

Source: [[Static analysis tool for Android]] (Helios, Confluence). The ratchet is my reading; the page documents the permissive setup.

## Related

- [[Fallow analyses a JS-TS repo as a graph and exposes it to agents over MCP]]
