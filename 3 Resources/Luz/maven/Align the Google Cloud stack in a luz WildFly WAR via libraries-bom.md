---
ai_hash: b90c378315e8bd51
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-10
entities: []
source: LUZ-158230 session 2026-08-10
status: seedling
tags:
- luz
- maven
- google-cloud
- libraries-bom
- grpc
- wildfly
title: Align the Google Cloud stack in a luz WildFly WAR via libraries-bom
type: lesson
---

# Align the Google Cloud stack in a luz WildFly WAR via libraries-bom

When adding the Google Cloud stack (Pub/Sub + gRPC + protobuf + guava) to a luz WildFly-26 WAR, align versions by importing `com.google.cloud:libraries-bom` in `<dependencyManagement>` (luz_docs uses **26.42.0**) and excluding `org.apache.avro:avro` from `luz_message_receiver` — rather than manually pinning `grpc-bom` / `protobuf-java` / `guava`.

**Why:** it is the proven, self-consistent recipe already running that lib on the same base image (`luz-wildfly26-all`). A manual grpc-bom pin (e.g. 1.56.1) + individual protobuf/guava versions is an alternative but risks a mismatched set; the BOM keeps the whole Google stack coherent.

**Note:** `libraries-bom` is a `dependencyManagement` import — BOM imports are NOT transitive, so declare it in each module that needs the stack. Adding it pulls newer transitive artifacts (e.g. `error_prone_annotations`) that may need an online resolve if the local `~/.m2` was only populated for older versions.

Surfaced on LUZ-158230 (import-job Tier 4) in `luz_docs_import`.

## Related

- [[Hosting a Pub/Sub consumer in a luz service with luz_message_receiver]]
- [[luz WildFly WARs expose only the MicroProfile Fault-Tolerance spec]]
- [[not SmallRye @ExponentialBackoff]]

%% ai-graph-start %%

**Related notes:**
- [[Hosting a PubSub consumer in a luz service with luz_message_receiver]]
- [[luz WildFly WARs expose only the MicroProfile Fault-Tolerance spec, not SmallRye @ExponentialBackoff]]
- [[luz_docs_import targets Java 17 (javax stack, modern idioms allowed)]]
- [[Kepler PubSub topology HTTP-to-luz_message_broker publisher, luz_message_receiver pull-consumers, jsonstore version-number CAS]]
- [[KlaraLuz Maven builds resolve dependencies from Google Artifact Registry and require gcloud auth]]

%% ai-graph-end %%