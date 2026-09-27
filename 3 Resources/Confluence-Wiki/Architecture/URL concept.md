---
ai_hash: 742b992119d977dd
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 1
depth: 2.37
entities: []
relevance: 0.721
source: https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457115956/URL+concept
space: AI
status: reference
tags:
- confluence
- architecture
- space/ai
title: URL concept
topic: architecture
type: source
updated: 2018-05-16
---

# URL concept

> [!info] Imported from Confluence
> Space **AI** · updated 2018-05-16 · [open original](https://axonivy.atlassian.net/wiki/spaces/AI/pages/2457115956/URL+concept)
> Relevance 0.721 · topic `architecture`

This document describes the URL concept of the public API Gateway.

The API Gateway is the single entry point for all clients:

- Provide access to HTML5 based user interfaces
- Expose APIs for 3rd party applications

## Domain name

    axonivy.ai

## Entry points

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Hostname</th>
<th>Description</th>
</tr>
&#10;<tr>
<td>axonivy.ai</td>
<td><p>Stable – The latest officially released services.</p></td>
</tr>
<tr>
<td>testing.axonivy.ai</td>
<td><p>Test – Services that haven't been accepted for being officially released, but they are in the queue for that.</p>
<p>The test environment provides more recent versions of software.</p></td>
</tr>
<tr>
<td>unstable.axonivy.ai</td>
<td><p>Unstable – The unstable environment is where active development occurs.</p>
<p>This is used by developers and those who like to live on the edge.</p></td>
</tr>
</tbody>
</table>

</div>

## URL Pattern

    https://axonivy.ai/<service>[/<version>]/<resource>

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th>Element</th>
<th>Description</th>
</tr>
&#10;<tr>
<td><pre><code>service</code></pre></td>
<td>Name of the service</td>
</tr>
<tr>
<td><pre><code>version</code></pre></td>
<td>API version where applicable (make it optional with an empty alias)</td>
</tr>
<tr>
<td><pre><code>resource</code></pre></td>
<td>The service specific resource</td>
</tr>
</tbody>
</table>

</div>

## Service (API) versions

Use major versions 

![[2457115956-unknown-macro.png]]

 and be backward compatible as long as the version remains the same.

The version specifier is prefixed by the single character 'v' followed by a positive integer.

- v1

- v12

Example of a full qualified URL:

- https://axonivy.ai/foo/v1/bar?baz=qux

## Service (API) versions for initial development version

Major version one (1.y.z) is for initial development. Anything may change at any time. The public API should not be considered stable.

The version specifier could therefore include the minor version.

- `v1`
- `v1.1`
- `v1.2`

%% ai-graph-start %%

**Related notes:**
- [[Invoice API Java Client]]
- [[OCR API Java Client]]

%% ai-graph-end %%