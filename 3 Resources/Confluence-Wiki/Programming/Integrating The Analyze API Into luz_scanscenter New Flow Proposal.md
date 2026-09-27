---
title: "Integrating The Analyze API Into luz_scanscenter: New Flow Proposal"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/48692756549/Integrating+The+Analyze+API+Into+luz_scanscenter+New+Flow+Proposal
space: "TS"
topic: programming
relevance: 0.846
depth: 2.98
updated: 2025-10-09
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Integrating The Analyze API Into luz_scanscenter: New Flow Proposal

> [!info] Imported from Confluence
> Space **TS** · updated 2025-10-09 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/48692756549/Integrating+The+Analyze+API+Into+luz_scanscenter+New+Flow+Proposal)
> Relevance 0.846 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="31506da3-2f2e-4543-8300-31016d976993" macro-name="toc">

</div>

## Situation

luz_scanscenter currently receives ZIP files with scanned letters and XML, then sends the letters to oneAPI. The XML contains a “matched tenant,” the matching result from Tessi. We use this result to identify recipients and send to oneAPI.

## Goal

We want to replace tenant matching from Tessi with the Analyze API developed by the AI Team. Migration to the Analyze API has three phases.

## Proposal for Phase 1: Integrate Analyze API

### Current flow in luz_scancenter

<a href="https://axonivy.atlassian.net/wiki/spaces/TS/whiteboard/48692658445" data-card-appearance="embed" data-width="100.00" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/TS/whiteboard/48692658445</a>

### Proposal

We will create a new process to call the Analyze API.

For each ZIP file process:

- After building document metadata, trigger the Analyze API process.

  - <span class="inline-comment-marker" ref="a5a47844-4257-404d-bd41-583178bd0d0f">If the Analyze API matches tenants, send to oneAPI and follow existing steps.</span>

  - If the Analyze API does not match tenants, try internal matching by calling luz_tenant_dir and follow existing steps.

#### The Analyze API process

<span class="inline-comment-marker" ref="215788e7-0c0a-4980-9af9-5e7638bd5f23">Create a single client to manage multiple jobs for letters in each ZIP file.</span>

When all jobs finish, update matching results from the Analyze API in the document metadata of letters.

<a href="https://axonivy.atlassian.net/wiki/spaces/TS/whiteboard/48700063766" data-card-appearance="embed" data-width="100.00" rel="nofollow">https://axonivy.atlassian.net/wiki/spaces/TS/whiteboard/48700063766</a>

## Open questions

If luz_scancenter or the Analyze API crashes <span class="inline-comment-marker" ref="6ff3c456-619f-4378-b7c0-ec2d245f4f15">during</span> processing, should we track processed steps/letters or restart from the beginning after restarting?
