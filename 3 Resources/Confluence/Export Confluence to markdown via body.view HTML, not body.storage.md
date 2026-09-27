---
title: "Export Confluence to markdown via body.view HTML, not body.storage"
created: 2026-09-27
type: howto
status: seedling
source: "session 2026-09-27 Confluence export"
tags: [confluence, atlassian, pandoc, markdown, export]
---

# Export Confluence to markdown via body.view HTML, not body.storage

When exporting Confluence pages to markdown, request the **rendered** body (`expand=body.view`) rather than the raw storage format (`body.storage`). `body.view` is plain HTML with every macro already expanded by the server, so it pipes straight through pandoc:

```bash
# one call gets body + metadata
GET /wiki/rest/api/content/{id}?expand=body.view,version,history,space,metadata.labels
pandoc -f html -t gfm-raw_html --wrap=none page.html
```

`body.storage` is Confluence XHTML carrying custom namespaces — `<ac:structured-macro>`, `<ac:plain-text-body><![CDATA[...]]>`, `<ri:attachment ri:filename="x.png"/>`, `<ac:layout-cell>`. Pandoc does not understand any of it, so you end up hand-writing a translator for each macro type (code blocks, info/note/warning panels, expands, tables of contents) and still miss the long tail. `body.view` has already done that work.

The one thing you must still fix up is asset URLs. In `body.view`, images arrive as server-relative links:

```html
<img class="confluence-embedded-image"
     src="/wiki/download/attachments/49785077788/diagram.png?version=1&amp;api=v2"
     data-linked-resource-default-alias="diagram.png">
```

Download the attachment list separately, then rewrite each `src` to the local file before running pandoc. Match on the **path basename** or on `data-linked-resource-default-alias`, and strip the query string first — the `?version=&modificationDate=` suffix means naive string equality against the attachment download link fails. Do the rewrite on HTML, not on the markdown pandoc emits; by then the URL may be split across link-reference syntax.

Trade-off to accept knowingly: `body.view` is a rendering, so it loses macro *identity*. A Jira-issue macro becomes its rendered table, not a re-creatable macro call. For an archival/readable export that is the right trade; for a round-trippable migration back into Confluence, you want `body.storage` and the translator.

## Related

- [[Confluence CQL search paginates by opaque cursor]]
- [[not start offset]]
