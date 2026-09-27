---
ai_hash: d6547d86d3a2822d
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-27
entities: []
source: session 2026-08-27 — kga extract.py
status: seedling
tags:
- atlassian
- adf
- confluence
- jira
- link-extraction
title: Extracting every link from Jira ADF and Confluence storage
type: howto
---

# Extracting every link from Jira ADF and Confluence storage

To reliably pull **every** link from Atlassian content, use three sources and dedup by a canonical key:

**Jira ADF (Atlassian Document Format)** — a JSON tree. Links live in two shapes:
- text nodes with a `link` **mark**: `node.marks[].type=="link"` → `attrs.href` (anchor = the node text).
- **smart-links** as their own nodes: `type in {inlineCard, blockCard, embedCard}` → `attrs.url` (no anchor).
Recurse `node["content"]`. A URL pasted as **plain text** has NO link mark — you miss it unless you also regex the concatenated ADF text. That is the classic gap.

**Confluence storage format** — XHTML. Anchors are `<a href>`; external macros are `<ri:url ri:value="...">`. BeautifulSoup(`html.parser`) parses namespaced tags, but a robust cheap trick is: collect `a[href]` via the parser AND regex the whole storage string for `https?://` (catches `ri:url` values + raw URLs). Watch for xmlns noise on real bodies.

**Classify + dedup:** map each URL to a `(type, canonical)` — `jira:<KEY>` from `/browse/`, `confluence:<id>` from `/wiki/.../pages/<id>`, else host-based (bitbucket/figma/google-doc/attachment/external). Dedup by canonical so the same page found via description + remotelink collapses to one record. Issue links and child pages give the KEY/id directly — set their canonical explicitly, do not URL-classify. Mark `in_scope = type in follow_types` (follow) vs record-only.

Context: kga `extract.py` (LUZ-159671 test-agent).

## Related

- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]

%% ai-graph-start %%

**Related notes:**
- [[Knowledge-Gathering loop is a bounded frontier crawl with a verify edge]]
- [[A link-following crawl pulls in graph-adjacent but topically-tangential nodes]]
- [[Export Confluence to markdown via body.view HTML, not body.storage]]
- [[Bitbucket Cloud API differs from JiraConfluence host, auth, raw src]]
- [[KGA crawler fetches repo source files via client mixin plus NodeFetcher registered by kind]]

%% ai-graph-end %%