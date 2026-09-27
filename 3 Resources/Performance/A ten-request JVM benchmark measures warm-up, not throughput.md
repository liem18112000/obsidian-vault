---
title: "A ten-request JVM benchmark measures warm-up, not throughput"
created: 2026-09-27
type: lesson
status: seedling
source: "Confluence: Performance Wildfly vs Quarkus"
tags: [benchmarking, jvm, quarkus, wildfly, methodology, apache-bench, confluence-distilled]
---

# A ten-request JVM benchmark measures warm-up, not throughput

A JVM framework comparison run as `ab -n10 -c10` measures **startup and JIT warm-up**, not steady-state throughput — and at that sample size the difference between the two frameworks is indistinguishable from noise.

The concrete case: Wildfly vs Quarkus, 3 GB / 2 CPU, `-Xmx2048m`, ten requests uploading a 1 MB PDF. Wildfly took 10.687 s total, Quarkus 10.950 s. That gap is well inside the run-to-run variance you would see repeating either one.

Three things break a benchmark of this shape:

1. **`-n10 -c10` means every request is the first request.** Ten requests, all concurrent, no warm-up: the JVM is interpreting bytecode and compiling on the fly throughout. HotSpot typically needs thousands of invocations before a method is compiled and inlined, so you are timing the compiler, not the code.
2. **The sample is too small for the statistic.** Percentiles over ten data points are theatre — the reported 90th, 95th, 98th and 99th percentiles are all the same number (7286 ms), because they are all the same single slowest request.
3. **The tool said so.** `ab` emitted it verbatim:
   > `ERROR: The median and mean for the initial connection time are more than twice the standard deviation apart. These results are NOT reliable.`

> [!warning] A tool's reliability warning is a result, not decoration
> That line is `ab` telling you the run is not interpretable. Recording the numbers underneath it — and worse, putting them in a comparison table someone will later cite — converts a known-invalid measurement into an apparent finding. If the warning fires, the correct output is "inconclusive, re-run with different parameters", not the table.

**What the run should have been:**

- **Warm up first, then measure.** Discard the first N requests entirely, or run long enough (minutes, not 10 seconds) that warm-up is a rounding error.
- **Separate the two questions.** *Startup/cold-start* and *steady-state throughput* are different metrics with different winners — Quarkus's whole pitch is the former. Measuring them together and reporting one number hides the only difference that matters.
- **Repeat and report spread.** Three runs with min/median/max beats one run with four decimal places.
- **Check what you are actually measuring.** Uploading a 1 MB PDF and generating a thumbnail is dominated by I/O and image processing; the framework is a thin layer around work that both frameworks delegate to the same libraries.

Related: [[Connection count, not tenant count, sizes a multi-tenant Postgres cluster]] — capacity claims deserve the same scrutiny as performance claims.

Source: [[Performance Wildfly vs Quarkus]] (Confluence). The critique here is my reading of the recorded run, not a conclusion stated on the page.

## Related

- [[N+1 hides at the service-call layer too, not just in the ORM]]
