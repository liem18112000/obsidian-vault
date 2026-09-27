---
ai_hash: ed01624f52093e10
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-08-11
entities: []
source: session 2026-08-11, LUZ-158230 Gap 3
status: seedling
tags:
- zip
- unicode
- nfc
- nfd
- testing
- python
- gotcha
title: Building a ZIP fixture to test NFC/NFD + UTF-8-flag entry-name handling
type: howto
---

# Building a ZIP fixture to test NFC/NFD + UTF-8-flag entry-name handling

To test how an importer decodes **ZIP entry names** (the folder/file names, not the archived content) you need a fixture that varies three things independently: the Unicode form, the UTF-8 flag, and non-ASCII coverage.

**1. Pick names whose NFC and NFD byte sequences actually differ.** Not every diacritic decomposes: Vietnamese `đ` (U+0111) has *no* decomposition, so a name of only `đ` is identical in NFC and NFD. Use characters that DO decompose — `ồ ợ ế ủ ả ệ` (Vietnamese) or `ü ö ä` (German). Verify with `unicodedata.normalize('NFC',s) != unicodedata.normalize('NFD',s)`.

**2. NFC vs NFD simulates Windows vs macOS producers.** macOS stores filenames decomposed (NFD); Windows composed (NFC). A dedup/matching key that is a raw byte-match drifts between the two for the same visual name. The fix is to `Normalizer.normalize(name, NFC)` the key (Java `java.text.Normalizer`) — then an NFC zip and an NFD zip of the same tree produce *identical* keys even though their raw entry bytes differ.

**3. Simulating a "legacy" zip (UTF-8 bytes, EFS flag unset).** Python's `zipfile` AUTO-sets general-purpose bit 11 (0x0800, the "language encoding"/EFS flag) whenever a name is non-ASCII, and encodes it UTF-8. To get UTF-8 bytes with the flag *unset* you must post-process the finished archive: clear bit 0x0800 in the flag field of every **local file header** (offset +6, found via `ZipInfo.header_offset`) and every **central directory header** (offset +8, walked from the EOCD record `PK\x05\x06` → central-dir offset at +16, count at +10). Use the EOCD offsets, not signature scanning, because file data can contain `PK` bytes.

**Gotcha / bonus demonstration:** once the flag is cleared, Python's own `zipfile` reader falls back to CP437 and shows the name as mojibake (e.g. `Hồ sơ` → `Hß╗ô s╞í`). That is exactly the mis-decode a naive reader makes — and exactly what forcing UTF-8 on read (zip4j `ZipFile.setCharset(StandardCharsets.UTF_8)`, applied unconditionally) prevents.

Tool that implements all three: `luz_docs_import/data/Lam/new-import-generator/generate_import_zip.py --gap3 [--normalization nfc|nfd] [--utf8-flag on|off]`.

## Related

- [[luz-docs-import ingest ZIP format (folders + per-file .metadata.json sidecar)]]

%% ai-graph-start %%

**Related notes:**
- [[ZIP entry names decode as CP437 mojibake when the UTF-8 EFS flag is unset]]
- [[luz-docs-import dedup identity is the uploaded zip filename (importZipName)]]
- [[NFC-normalize dedup keys so macOS NFD and Windows NFC file re-exports converge]]
- [[luz-docs-import Gap-3 always-UTF-8 ZIP decode is deliberate; CP437 non-flagged zips are accepted best-effort]]
- [[Windows Python stdout is cp1252 and crashes on emoji; reconfigure to UTF-8]]

%% ai-graph-end %%