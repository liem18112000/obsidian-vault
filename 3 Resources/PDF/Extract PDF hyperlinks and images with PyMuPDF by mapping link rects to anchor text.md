---
ai_hash: 673c1b2da4881367
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-09-08
entities: []
source: session 2026-09-08 (CV tailoring task)
status: seedling
tags:
- pymupdf
- fitz
- pdf
- python
- hyperlinks
title: Extract PDF hyperlinks and images with PyMuPDF by mapping link rects to anchor
  text
type: howto
---

# Extract PDF hyperlinks and images with PyMuPDF by mapping link rects to anchor text

To re-author a PDF (e.g. a resume) into a new format **without losing its embedded hyperlinks or photos**, extract those assets with PyMuPDF (`fitz`) before rebuilding.

**Links:** `page.get_links()` returns dicts with a `uri` and a `from` rectangle. The rectangle is *where* the link sits, not the visible label — recover the anchor text with `page.get_textbox(link['from'])`. That gives you a clean `{anchor_text -> uri}` map to reattach in the rebuilt document.

**Images:** `page.get_images(full=True)` lists xrefs; `doc.extract_image(xref)` returns the raw bytes plus `ext`/`width`/`height`. Re-embed as a base64 `data:` URI so the rebuilt HTML stays self-contained.

Gotcha: the plain `Read`/text extraction of a PDF gives you the visible text but **drops the URL targets** — you must go through the link annotations to recover them.

```python
import fitz
doc = fitz.open('in.pdf'); page = doc[0]
for l in page.get_links():
    if l.get('uri'):
        label = page.get_textbox(l['from']).strip()
        print(label, '->', l['uri'])
d = doc.extract_image(page.get_images(full=True)[0][0])  # bytes in d['image'], type in d['ext']
```

Pairs with the HTML→PDF render step: [[Headless Chrome print-to-pdf preserves HTML anchor tags as clickable PDF links]].

## Related

- [[Headless Chrome print-to-pdf preserves HTML anchor tags as clickable PDF links]]

%% ai-graph-start %%

**Related notes:**
- [[Update a designed PDF without its source by rebuilding as HTML and printing with headless Edge]]
- [[Headless Chrome print-to-pdf preserves HTML anchor tags as clickable PDF links]]

%% ai-graph-end %%