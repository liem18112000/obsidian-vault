---
title: "Secure File Upload via Public API to eArchive"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/49392615425/Secure+File+Upload+via+Public+API+to+eArchive
space: "TS"
topic: programming
relevance: 0.777
depth: 2.88
updated: 2026-05-07
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Secure File Upload via Public API to eArchive

> [!info] Imported from Confluence
> Space **TS** · updated 2026-05-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/49392615425/Secure+File+Upload+via+Public+API+to+eArchive)
> Relevance 0.777 · topic `programming`

### **Open Questions – Public API Upload to eArchive**

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>Category</strong></p></th>
<th><p><strong>Question</strong></p></th>
<th><p><strong>Why it matters</strong></p></th>
<th><p><strong>Suggestion / Answer GUI(FE)</strong></p></th>
<th><p><strong>Suggestion / Answer Public API</strong></p></th>
</tr>
&#10;<tr>
<td><p><strong>Upload Scope</strong></p></td>
<td><p>Do we support single file upload only or also batch upload?</p></td>
<td><p>Impacts API design and complexity</p></td>
<td><p>Each request contains a single file, served by <code>luz_docs_view_controller</code> — the layer already targeted by the frontend. The FE sends one <code>files</code> part per POST.</p></td>
<td><p>The new external API is exposed as a route on <code>luz_docs_view_controller</code> (e.g. <code>POST /api/v1/{tenantId}/documents</code>). One <code>files</code> part per request; reject anything else with <code>400 TOO_MANY_FILES</code> rather than the current silent drop in <code>luz_docs.processFiles</code>.</p></td>
</tr>
<tr>
<td></td>
<td><p>If batch: one request with multiple files or multiple requests?</p></td>
<td><p>Affects performance and error handling</p></td>
<td><p>Not supported. The FE allows selecting multiple files but each is sent as its own POST.</p></td>
<td><p><code>luz_docs_view_controller</code> does not currently support batch operations. Enabling true batch upload requires DB-side batch handling and per-entry error reporting. Phase 1: many requests (one per file). A <code>/documents:batch</code> endpoint can be designed later.</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we support folder uploads or only file-to-directory?</p></td>
<td><p>Defines the scope of the feature</p></td>
<td><p>File-to-directory, or no directory (root / Inbox).</p></td>
<td><p>File-to-directory, or no directory (root).</p>
<p>File-to-directory only. Folder <em>creation</em> lives in a separate endpoint.</p></td>
</tr>
<tr>
<td><p><strong>File Limits</strong></p></td>
<td><p>What is the maximum file size?</p></td>
<td><p>Impacts infra, scanning, UX</p></td>
<td><p>Same as UI — 200 MiB per file (<code>LettersUploadViewHandler.getSizeLimit() = 209 715 200</code>). Enforced client-side and server-side inside <code>xpertline-luz-components.FileUploadMultiViewHandler</code>.</p></td>
<td><ul>
<li><p>Same UI 200 MiB per file. Validate at the BFF (don't trust the client). If a file exceeds 200 MiB → route through the existing <code>/{tenantId}/documents/large-file</code> endpoint.</p></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>Do we need chunked/resumable uploads?</p></td>
<td><p>Required for large files</p></td>
<td><p>No. The FE never sends &gt;200 MiB.</p></td>
<td><p>No. If &gt;200 MiB is ever required, use <code>/{tenantId}/documents/large-file</code> (bypasses BFF bulkhead).</p></td>
</tr>
<tr>
<td></td>
<td><p>Are limits tenant-specific or global?</p></td>
<td><p>Impacts configurability</p></td>
<td colspan="2"><p>Global — single 200 MiB cap for every tenant.</p></td>
</tr>
<tr>
<td><p><strong>File Types</strong></p></td>
<td><p>Which file types (MIME) are allowed?</p></td>
<td><p>Security &amp; compliance</p></td>
<td><p>Same as frontend eArchive PLUS</p>
<p>FE eArchive set defined in <code>LetterUploadSupportedFileTypes.java</code>:<br />
<code>.pdf .doc .docx .rtf .txt .odt .ods .odp .odf .xls .xlsx .gif .jpg .jpeg .jpe .jfif .png .bmp</code></p></td>
<td rowspan="2"><ul>
<li><p>S<em>hould all file types be allowed or the same GUI-eArchive, and should we restrict only executable files? The restriction should apply to the following extensions:</em> <code>.exe</code><em>,</em> <code>.js</code><em>,</em> <code>.ps1</code><em>,</em> <code>.bat</code><em>,</em> <code>.sh</code><em>,</em> <code>.com</code><em>,</em> <code>.scr</code><em>,</em> <code>.vbs</code><em>,</em> <code>.jar</code><em>,</em> <code>.html</code><em>,</em> <code>.svg</code><em>.</em></p></li>
<li><p>Should validate on <strong>Public API</strong></p></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>Allowlist or blocklist approach?</p></td>
<td><p>Simplicity vs flexibility</p></td>
<td><p>Allowlist (FE only — extension check via <code>&lt;p:fileUpload accept="…"&gt;</code> and the xpertline base class).</p></td>
</tr>
<tr>
<td></td>
<td><p>Are executables/scripts allowed?</p></td>
<td><p>High security risk</p></td>
<td><p>Implicitly rejected by the allowlist.</p></td>
<td><p>Rejected by the allowlist. Add explicit deny list as defence-in-depth: <code>.exe .js .ps1 .bat .sh .com .scr .vbs .jar .html .svg</code> → <code>415 UNSUPPORTED_FILE_TYPE</code>.</p></td>
</tr>
<tr>
<td></td>
<td><p>Are archives (ZIP) allowed and scanned recursively?</p></td>
<td><p>Malware risk</p></td>
<td><p>Not allowed by FE allowlist.</p></td>
<td><p>Support <code>.zip</code>. BFF rules:<br />
1. Server unzips into temp.<br />
2. Apply allowlist + size cap to each entry.<br />
3. Refuse zip-bombs (uncompressed ≤ 1 GiB, ratio ≤ 100×).<br />
4. Refuse nested zips (depth = 1) unless explicitly required.<br />
5. Run AV on each extracted entry.<br />
6. Reject password-protected archives.<br />
Note: <code>luz_docs.createDocumentFromZipFile</code> exists but is for pre-built enriched bundles (<code>Content-FileType: application/zip</code> / <code>isDocumentEnriched=true</code>), not user zips — a new BFF path is needed.</p></td>
</tr>
<tr>
<td><p><strong>Metadata</strong></p></td>
<td><p>Is <code>directoryId</code> mandatory?</p></td>
<td><p>Core routing logic</p></td>
<td><ul>
<li><p><em>No, optional. Empty/missing = upload to root.</em> Today's FE behavior.</p></li>
</ul></td>
<td><ul>
<li><p><em>No, optional. Empty/missing = upload to root</em></p></li>
</ul></td>
</tr>
<tr>
<td></td>
<td><p>What metadata is required vs optional?</p></td>
<td><p>API usability</p></td>
<td colspan="2"><p><strong>Required:</strong> <code>fileName</code>, <code>documentTitle</code>, <code>documentReferenceDate</code> (yyyy-MM-dd). <strong>Optional:</strong> <code>senderName</code>, <code>origin</code>, <code>documentTypes[]</code>, <code>folderIds[]</code>, <code>securityClassCodes[]</code>, <code>tags[]</code>, <code>description</code>, <code>currency</code>, <code>amount</code>, custom attributes.</p></td>
</tr>
<tr>
<td></td>
<td><p>How is <code>securityClass</code> passed (if at all)?</p></td>
<td><p>Alignment with internal model</p></td>
<td><p>Today: assigned <strong>after</strong> upload via <code>PUT /letters/{id}</code> with the full <code>Letter</code> body (codes only, not full <code>SecurityClass</code>). For an external API, accept <code>securityClassCodes: [string]</code> in the upload metadata so it can land in one round-trip.</p></td>
<td><p>The feature must include an upload API. Let’s attempt an initial implementation first.</p></td>
</tr>
<tr>
<td></td>
<td><p>Can external systems pass custom metadata?</p></td>
<td><p>Integration flexibility</p></td>
<td colspan="2"><p>No implement</p></td>
</tr>
<tr>
<td><p><strong>Permissions</strong></p></td>
<td><p>How do we verify write access to a <code>directoryId</code>?</p></td>
<td><p>Prevents security bypass</p></td>
<td><p>UI-only checks: <code>UploadableLetterboxContentHandler.isAbleToUpload</code>, <code>KlaraBusinessAGFolderLetterBoxContentHandler.isUploadable</code> (only <code>OwnDocuments</code> writable). The backend trusts the JWT but does not verify caller has WRITE on the specific folder beyond the tenant</p></td>
<td><p>Validate <code>directoryId</code> must belong eArchive and tenant<br />
</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we need a permission-check endpoint?</p></td>
<td><p>UX vs backend responsibility</p></td>
<td colspan="2"><p>No need</p></td>
</tr>
<tr>
<td></td>
<td><p>What happens on insufficient permissions?</p></td>
<td><p>Error handling</p></td>
<td><p>UI hides the upload action; no server error path is exercised.</p></td>
<td><p><em>Return</em> <code>404 FOLDER_NOT_FOUND</code> <em>when the provided</em> <code>folderId</code> <em>in the request body does not exist.</em></p></td>
</tr>
<tr>
<td><p><strong>Malware Scanning</strong></p></td>
<td><p>Synchronous or asynchronous scanning?</p></td>
<td><p>UX vs performance trade-off</p></td>
<td><p>Synchronous. <code>luz_docs.antivirusService.scan</code> runs in-request before metadata + storage. Skipped when <code>origin == KLARA_BUSINESS</code>, <code>isLargeFileSupport</code>, <code>isBasicDocument</code>, or <code>isInternalDocument</code>.</p></td>
<td><p><strong>Synchronous</strong>, <em><strong>Scan runs by default.</strong> It only skips if the origin is trusted ("Klara Business"), or the caller opts out via</em> <code>isLargeFileSupport</code><em>,</em> <code>isBasicDocument</code><em>, or</em> <code>Internal-Document</code><em>, or there's no file. <strong>For ZIP uploads, only</strong></em> <code>isBasicDocument</code> <em><strong>opts out.</strong></em></p></td>
</tr>
<tr>
<td></td>
<td><p>Where is file stored during scanning (quarantine)?</p></td>
<td><p>Data integrity</p></td>
<td colspan="2"><p>Same — local temp on the BE / <code>luz_docs</code> node only. GCS write happens only after scan passes.</p></td>
</tr>
<tr>
<td></td>
<td><p>What happens if scanner fails or times out?</p></td>
<td><p>Reliability</p></td>
<td colspan="2"><h2 id="SecureFileUploadviaPublicAPItoeArchive-AVscannerfailure&amp;timeout" data-local-id="6a97cb8d6e50">AV scanner failure &amp; timeout</h2>
<ul>
<li><p><strong>Breaker open</strong> (after ~3/5 failures, 17s window) → <code>503 antivirus.service.unavailable</code> via fallback. Clean.</p></li>
<li><p><strong>Transport error / scanner 5xx / null body</strong> → retried 3× (500ms), then masked as <code>400 can.not.create.document</code>. Real cause only in logs. Should be <code>503</code> but <code>ProcessingException</code> is wrapped into <code>RuntimeException</code>, hiding it from the mapper.</p></li>
<li><p><strong>Hang / timeout</strong> → no <code>readTimeout</code> on the AV client, no <code>@Timeout</code> on <code>scan</code> — the upload thread blocks indefinitely. <code>TimeoutExceptionMapper</code> never fires.</p></li>
<li><p><strong>Document state on failure</strong> → nothing persisted (scan runs before Mongo + GCS), temp file cleaned up.</p></li>
</ul>
<p><strong>Three gaps to fix:</strong> add <code>connectTimeout</code>/<code>readTimeout</code> for <code>LUZ_ANTIVIRUS_HOST_PORT</code>, add <code>@Timeout</code> to <code>scan(...)</code>, and stop wrapping <code>ProcessingException</code> into <code>RuntimeException</code> so the existing <code>503</code> mapper works.</p></td>
</tr>
<tr>
<td></td>
<td><p>What happens if malware is detected?</p></td>
<td><p>Compliance requirement</p></td>
<td colspan="2"><h2 id="SecureFileUploadviaPublicAPItoeArchive-AVfailure&amp;timeoutbehavior" data-local-id="f6c93dd43b2c">AV failure &amp; timeout behavior</h2>
<ul>
<li><p><strong>Malware</strong> → <code>400 malware.file.detected</code> (no retry, no breaker hit, nothing persisted).</p></li>
</ul>
<div id="expander-1480902919" class="expand-container conf-macro output-block" data-hasbody="true" data-macro-id="50bd92f7-064a-4342-8d7c-c9413ec80eed" data-macro-name="expand">
<div id="expander-control-1480902919" class="expand-control">
<span class="expand-control-icon icon"> </span><span class="expand-control-text">Click here to expand...</span>
</div>
<div id="expander-content-1480902919" class="expand-content expand-hidden">
<div class="code panel pdl conf-macro output-block" data-hasbody="true" data-macro-id="a51a5523-9dc6-49f7-a97c-5ee4c269a5bb" data-macro-name="code" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>HTTP/1.1 400 Bad Request
Content-Type: application/json
{
  &quot;code&quot;: &quot;malware.file.detected&quot;,
  &quot;detail&quot;: &quot;File invoice.pdf rejected due to malware detection. Threat details: &lt;resultDetail&gt;&quot;,
  &quot;createTime&quot;: &quot;...&quot;,
  &quot;businessError&quot;: true
}</code></pre>
</div>
</div>
</div>
</div></td>
</tr>
<tr>
<td><p><strong>Versioning &amp; Duplicates</strong></p></td>
<td><p>What happens if same file is uploaded twice?</p></td>
<td><p>Data consistency</p></td>
<td rowspan="2"><p>Match webclient. Today: regular folders allow duplicates; KLARA Business AG <code>OwnDocuments</code> rejects same-name uploads (<code>OwnDocumentUploadHandler.validateUniqueName</code> → <code>FileAlreadyExistsException</code> → CMS <code>UploadError.FILE_ALREADY_EXIST</code>).</p></td>
<td rowspan="4"><p><strong>Should we allow duplicated</strong>? Becasue public api not support <strong>KLARA Business AG</strong></p>
<p>If<br />
Yes. BFF accepts an <code>Idempotency-Key</code> header (UUID); cache the result for 24 h. A replay returns the same <code>documentId</code> + 200 instead of creating a duplicate. Critical because the BFF's forced <code>Connection: close</code> to <code>luz_docs</code> causes routine retries on transient TLS failures.</p></td>
</tr>
<tr>
<td></td>
<td><p>Same filename in same folder: overwrite, reject, version?</p></td>
<td><p>Business rule clarity</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we need idempotency support?</p></td>
<td><p>Prevent duplicate uploads</p></td>
<td><p>None. UI re-tries are user-driven; double-clicks can create duplicates in non-KLARA Business AG folders.</p></td>
</tr>
<tr>
<td><p><strong>Error Handling</strong></p></td>
<td><p>What error codes do we expose?</p></td>
<td><p>Integration clarity</p></td>
<td><p>(<code>OwnDocumentUploadHandler.validateUniqueName</code> → <code>FileAlreadyExistsException</code> → CMS <code>UploadError.FILE_ALREADY_EXIST</code>).</p></td>
</tr>
<tr>
<td><p><strong>Performance &amp; Reliability</strong></p></td>
<td><p>What throughput is expected?</p></td>
<td><p>Capacity planning</p></td>
<td colspan="2" rowspan="4"><p>BE</p>
<h3 id="SecureFileUploadviaPublicAPItoeArchive-Limitsactuallyconfigured" data-local-id="26631c8337cf">Limits actually configured</h3>
<div>
<table>
<tbody>
<tr>
<th><p>Limit</p></th>
<th><p>Value</p></th>
<th><p>Where</p></th>
</tr>
&#10;<tr>
<td><p>Concurrent uploads on <code>/documents</code></p></td>
<td><p><strong>10</strong> (default <code>@Bulkhead</code>)</p></td>
<td><p><code>DocumentResource.uploadFile</code></p></td>
</tr>
<tr>
<td><p>Concurrent uploads on <code>/documents/large-file</code></p></td>
<td><p><strong>unlimited</strong> (no <code>@Bulkhead</code>)</p></td>
<td><p><code>DocumentResource.uploadLargeFile</code></p></td>
</tr>
<tr>
<td><p>Body size on <code>/documents</code></p></td>
<td><p><code>NORMAL_POST_REQUEST_SIZE</code> env</p></td>
<td><p><code>PostRequestSizeFilter</code></p></td>
</tr>
<tr>
<td><p>Body size on <code>/documents/large-file</code></p></td>
<td><p>unbounded (filter exempts it)</p></td>
<td><p>—</p></td>
</tr>
<tr>
<td><p>Connect timeout to luz-docs</p></td>
<td><p><strong>1000 ms</strong></p></td>
<td><p><code>microprofile-config.properties</code> (<code>LUZ_DOCS_HOST_PORT/mp-rest/connectTimeout</code>)</p></td>
</tr>
<tr>
<td><p>Read timeout to luz-docs (upload route)</p></td>
<td><p><strong>not configured</strong></p></td>
<td><p><code>LUZ_DOCS_HOST_PORT</code> has no <code>readTimeout</code></p></td>
</tr>
<tr>
<td><p>Read timeout for the dedicated upload client</p></td>
<td><p><strong>1 200 000 ms = 20 min</strong></p></td>
<td><p><code>LUZ_DOCS_CREATE_DOCUMENT_HOST_PORT/mp-rest/readTimeout</code> (only used if you switch the upload to <code>LuzDocsCreateDocumentRestClient</code>)</p></td>
</tr>
<tr>
<td><p>HTTP keep-alive to luz-docs</p></td>
<td><p><strong>disabled</strong></p></td>
<td><p><code>@ClientHeaderParam("Connection", "close")</code> on <code>LuzDocsRestClient</code></p></td>
</tr>
<tr>
<td><p>Retries on upload</p></td>
<td><p><strong>none</strong></p></td>
<td><p>upload path has no <code>@Retry</code> (only Pub/Sub publishers retry)</p></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr>
<td></td>
<td><p>How many concurrent uploads must be supported?</p></td>
<td><p>Scaling</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we support retries?</p></td>
<td><p>Robustness</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we define SLA for upload + scan?</p></td>
<td><p>Business expectation</p></td>
</tr>
<tr>
<td><p><strong>Audit &amp; Compliance</strong></p></td>
<td><p>What events must be logged?</p></td>
<td><p>Regulatory requirement</p></td>
<td colspan="2" rowspan="3"><p>BE<br />
<code>luz_docs.auditLogDocumentEventService.logAuditEvent</code> writes a storage-commit entry only. AV results, validation failures, etc. only show up in the app log.</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we log upload, scan, and final commit separately?</p></td>
<td><p>Traceability</p></td>
</tr>
<tr>
<td></td>
<td><p>Do we track calling system identity?</p></td>
<td><p>Accountability</p></td>
</tr>
<tr>
<td><p><strong>Security</strong></p></td>
<td><p>How are files transferred securely?</p></td>
<td><p>Data protection</p></td>
<td><p>TLS, Bearer token.</p></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>Do we validate checksums?</p></td>
<td><p>Integrity</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>Do we sanitize filenames?</p></td>
<td><p>Prevent exploits</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>Do we need signed requests?</p></td>
<td><p>API security</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><strong>Operations</strong></p></td>
<td><p>How are failed or stuck uploads handled?</p></td>
<td><p>Supportability</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>Do we need admin visibility for quarantined files?</p></td>
<td><p>Operational control</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>How long are quarantined files retained?</p></td>
<td><p>Storage &amp; compliance</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td><p><strong>API Design</strong></p></td>
<td><p>Multipart upload or pre-signed upload?</p></td>
<td><p>Implementation choice</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>Do we return immediate response or job-based response?</p></td>
<td><p>UX &amp; async handling</p></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td><p>When do we return document ID (before or after scan)?</p></td>
<td><p>Consistency</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

</div>
