---
ai_hash: 9cf0bf1e19029724
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-12
entities: []
source: luz_docs_import large-ZIP latency investigation 2026-08-12
status: seedling
tags:
- nginx-ingress
- kubernetes
- upload
- buffering
- performance
- gotcha
title: nginx-ingress proxy-request-buffering stages the whole request body before
  forwarding to the pod
type: gotcha
---

# nginx-ingress proxy-request-buffering stages the whole request body before forwarding to the pod

nginx-ingress defaults to **`proxy-request-buffering: on`**, which buffers the **entire request body at the ingress before forwarding a single byte to the upstream pod**. It effectively serializes "client to ingress" then "ingress to pod", and spills bodies larger than `client-body-buffer-size` to the ingress's own disk.

**Consequence for large uploads:** an extra full-size staging step that never appears in application logs — the pod's handler does not even start reading until the upload is fully received at the proxy. Set `nginx.ingress.kubernetes.io/proxy-request-buffering: "off"` on the upload route to stream through to the pod instead.

Also verify, for large uploads: `proxy-body-size` must be at least the max upload (else 413), and `proxy-read-timeout`/`proxy-send-timeout` must exceed the worst-case upload duration (else a 504 mid-upload).

Related: [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]], [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]].

## Related

- [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]]
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]

%% ai-graph-start %%

**Related notes:**
- [[luz_docs_import upload-zip is slow for large files due to a synchronous double-write]]
- [[Decouple upload API latency from file size with pre-signed direct-to-object-storage uploads]]
- [[RESTEasy MultipartFormDataInput buffers the whole upload to tmp before the app reads it]]
- [[GKE pd-standard disk throughput is low and scales with provisioned volume size]]
- [[luz-docs-import upload-zip endpoint is the ingestion saturation point under perf load]]

%% ai-graph-end %%