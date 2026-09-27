---
title: "Headless Chrome print-to-pdf preserves HTML anchor tags as clickable PDF links"
created: 2026-09-08
type: howto
status: seedling
source: "session 2026-09-08 (CV tailoring task)"
tags: [chrome, headless, html-to-pdf, pdf, windows]
---

# Headless Chrome print-to-pdf preserves HTML anchor tags as clickable PDF links

When you build a document as HTML and need a PDF where the `<a href>` links stay **clickable**, render with headless Chrome/Edge's print pipeline — it converts anchor tags into real PDF link annotations (unlike screenshot/image-based exports, which flatten everything).

```bash
chrome.exe --headless=new --disable-gpu --no-pdf-header-footer \
  --print-to-pdf=out.pdf --print-to-pdf-no-header \
  file:///C:/path/to/page.html
```

Notes:
- On Windows, Edge works identically (`msedge.exe`, same flags); both live under `Program Files\...\Application\`.
- Control page size/margins with CSS `@page { size:A4; margin:0 }` — the print engine honors it.
- A `cloud_policy_validator ... signature verification failed` line on stderr is a harmless enterprise-policy warning; the PDF still writes and exit code is 0.
- **Always verify** links survived: reopen with PyMuPDF and count annotations — `sum(len([l for l in doc[p].get_links() if l.get('uri')]) for p in range(doc.page_count))`.
- Embed images as base64 `data:` URIs so the HTML is self-contained and needs no network at render time.

Companion to the extraction step: [[Extract PDF hyperlinks and images with PyMuPDF by mapping link rects to anchor text]].

## Related

- [[Extract PDF hyperlinks and images with PyMuPDF by mapping link rects to anchor text]]
