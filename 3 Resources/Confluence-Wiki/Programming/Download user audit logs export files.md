---
title: "Download user audit logs export files"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47122156152/Download+user+audit+logs+export+files
space: "LUZ"
topic: programming
relevance: 0.716
depth: 2.89
updated: 2023-07-17
attachments: 5
tags:
  - confluence
  - programming
  - space/luz
---

# Download user audit logs export files

> [!info] Imported from Confluence
> Space **LUZ** · updated 2023-07-17 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47122156152/Download+user+audit+logs+export+files)
> Relevance 0.716 · topic `programming`

# 1. API


![[47122156152-image-20220606-025717.png]]



- Endpoint: /api/{tenant-id}/audits/export-files

- Method: GET

- Response Content-Type: application/zip

Request sample:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="93a1f899-7c71-4aaf-b21c-00fdf2d31e79" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
curl --location --request GET 'http://localhost:8090/luz_audit/api/6000e266-71e1-4973-a895-a0c9a70eb5f9/audits/export-files' \
--header 'Authorization: Bearer eyJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJhdS5uZ3V5ZW5waHVvY0BheG9uYWN0aXZlLmNvbSIsImNvbXBhbnktdGVuYW50Ijp7ImNvbXBhbnlJbmZvSWQiOjUyNzYsImNvbXBhbnlJZCI6MSwiY29tcGFueU5hbWUiOiJBVU5HIEx0ZCIsInN0YXR1cyI6IkFDVElWQVRFRCIsInN0YXR1c0V4cGlyeURhdGUiOm51bGwsImlkIjo4Mzc2OCwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIl0sInRlbmFudElkIjoiNjAwMGUyNjYtNzFlMS00OTczLWE4OTUtYTBjOWE3MGViNWY5IiwidXNlcm5hbWUiOiJhdS5uZ3V5ZW5waHVvY0BheG9uYWN0aXZlLmNvbSIsIm5hbWUiOm51bGwsInR5cGUiOiJDT01QQU5ZIiwiY3JlYXRlRGF0ZSI6bnVsbCwidXBkYXRlRGF0ZSI6bnVsbCwiZXhwaXJlRGF0ZSI6bnVsbCwiZXhwaXJlVGltZSI6bnVsbH0sImlzcyI6ImNvbS5heG9uaXZ5IiwidGVuYW50SWQiOiI2MDAwZTI2Ni03MWUxLTQ5NzMtYTg5NS1hMGM5YTcwZWI1ZjkiLCJ1c2VyX3JvbGVzIjpbIkV2ZXJ5Ym9keSIsIkFkbWluaXN0cmF0b3IiXSwicGVyc29uLXRlbmFudCI6eyJpZCI6MCwicm9sZXMiOlsiY29tcGFueV9hZG1pbmlzdHJhdG9yIl0sInRlbmFudElkIjoiMzAyNmZjZjItZmNmZi00OWI0LThiZDItNjU4ZGM1ZmQ5YWY5IiwidXNlcm5hbWUiOiJhdS5uZ3V5ZW5waHVvY0BheG9uYWN0aXZlLmNvbSIsIm5hbWUiOm51bGwsInR5cGUiOiJQRVJTT04iLCJjcmVhdGVEYXRlIjoiTW9uIEFwciAwNCAwODo0NjoxNCBDRVNUIDIwMjIiLCJ1cGRhdGVEYXRlIjpudWxsLCJleHBpcmVEYXRlIjpudWxsLCJleHBpcmVUaW1lIjpudWxsfSwiZXhwIjoxNjU0MjkyMDEwLCJpYXQiOjE2NTQyNDg4MTAsInNlY3VyaXR5X2NsYXNzZXMiOltdfQ.Yel6QxNVTdUqR8oxYc_mnJbEr-OnVoRPO-6L3Kwo-4H32En5kcybpAzKZzzytctI60evaSqxd7uKjNB18iNBGEhe1fgHPF-rmEjKQrq3jBnfigrVknYunNGn62i_ekBDwW8K4sMuW6jKSHKkBgwpaAB4Pk13n8Sq5ySlsK6nabU7pUWLaY0goaskZJ3R9qekb6vrXE0xbJMgUUMkkyy3r7w89jBtAqzcM1XF9d7NHEj7Rf5ooqbgiZjd_ADR4bd4tR1Ej5jp4QyyWHk08EuNdrvvJsSTS4RkQ8o19HIjFo76RjMWESbOUZHSbSVc5sRZu4LCuQvDWu1tKo3oqoYPaw'
```

</div>

</div>


![[47122156152-image-20220606-030858.png]]



Response sample:


![[47122156152-image-20220606-030107.png]]



# 2. Sequence Diagram


![[47122156152-download export files diagram.png]]



# 3. Implementation

Introduce two `ExecutorService`

- store file executor

  - purpose: get files from GCS then store in temp directory

  - max thread configuration: `STORE_CSV_AUDIT_LOG_FILE_NUMBER_THREAD`

  - default max thread: 2

- modify file executor

  - purpose: read file from temp directory then decrypt the audit data (call to vault service)

  - max thread configuration `MODIFY_CSV_AUDIT_LOG_FILE_NUMBER_THREAD`

  - default max thread: 2

Using `CountDownLatch` to wait all threads are finished

Code sample:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7dcb68dd-859f-413c-ad1b-a50c7ed9e8f8" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
 public StreamingOutput downloadAllCSVAuditLog(final String tenantId) throws IOException {
        Iterable<Blob> blobs = gcsService.getBlobs(tenantId);
        int filesNumber = 0;
        if (blobs == null || (filesNumber = Iterables.size(blobs)) == 0) {
            LOGGER.warning("[downloadAllCSVAuditLog] audit Log data is empty for tenant: " + tenantId);
            throw new GoogleCloudStorageException(BundleConstant.FILE_IS_NOT_EXISTED);
        }
        final Path tempDir = Files.createTempDirectory(tenantId + UUID.randomUUID().toString());
        LOGGER.info("[downloadAllCSVAuditLog] temp directory absolutely path: " + tempDir.toFile().getAbsolutePath());
        try {
            CountDownLatch countDownLatch = new CountDownLatch(filesNumber);
            ExecutorService storeFileExecutorService = storeFileExecutor.getExecutorService();
            ExecutorService modifyFileExecutorService = modifyFileExecutor.getExecutorService();
            long start = System.currentTimeMillis();
            LOGGER.info("[downloadAllCSVAuditLog] start download csv at: " + start);
            List<Future<?>> tasks = new ArrayList<>();
            for (Blob blob : blobs) {
                Future<?> task = storeFileExecutorService.submit(() -> {
                    try {
                        LOGGER.info("[downloadAllCSVAuditLog#storeFileExecutorService] thread name: " + Thread.currentThread().getName() + " execute at: " + System.currentTimeMillis());
                        String filePath = GoogleCloudStorageUtils.writeBlobsToTempDir(blob, tempDir);
                        if (StringUtils.isBlank(filePath)) {
                            countDownLatch.countDown();
                            throw new GoogleCloudStorageException(BundleConstant.CAN_NOT_DOWNLOAD_FILE);
                        }
                        LOGGER.info("[downloadAllCSVAuditLog#storeFileExecutorService] temp csv file absolutely path: " + filePath);
                        modifyCSVFile(filePath, tempDir, PropertyRetriever.getAuditTenant(), countDownLatch, modifyFileExecutorService);
                    } catch (Exception e) {
                        LOGGER.warning("[downloadAllCSVAuditLog] throw Exception: " + ExceptionUtils.getStackTrace(e));
                        throw new AuditException(BundleConstant.CAN_NOT_DOWNLOAD_FILE);
                    }
                });
                tasks.add(task);
            }

            countDownLatch.await(filesNumber * TIME_OUT_PER_FILE, TimeUnit.MINUTES);
            long end = System.currentTimeMillis();
            LOGGER.info("[downloadAllCSVAuditLog] finish download csv at: " + end + " time consumption of download csv file: " + (end - start));

            try {
                for (Future<?> task : tasks) {
                    task.get();
                }
            } catch (Exception e) {
                LOGGER.warning("[downloadAllCSVAuditLog] there are some errors that could not be catch while execute tasks: " + ExceptionUtils.getStackTrace(e));
                throw new AuditException(BundleConstant.CAN_NOT_DOWNLOAD_FILE);
            }

            return ZipUtils.createZipOutputStream(tempDir, tenantId);

        } catch (GoogleCloudStorageException gce) {
            LOGGER.warning("[downloadAllCSVAuditLog] throw GoogleCloudStorageException: " + ExceptionUtils.getStackTrace(gce));
            throw new AuditException(gce.getCode());
        } catch (IOException ioe) {
            LOGGER.warning("[downloadAllCSVAuditLog] throw IOException: " + ExceptionUtils.getStackTrace(ioe));
            throw new AuditException(BundleConstant.CAN_NOT_DOWNLOAD_FILE);
        } catch (AuditException ae) {
            LOGGER.warning("[downloadAllCSVAuditLog] throw AuditException: " + ExceptionUtils.getStackTrace(ae));
            throw ae;
        } catch (Exception e) {
            LOGGER.warning("[downloadAllCSVAuditLog] throw Exception: " + ExceptionUtils.getStackTrace(e));
            throw new AuditException(BundleConstant.CAN_NOT_DOWNLOAD_FILE);
        } finally {
            FileUtils.deleteDirectory(tempDir.toFile());
        }
    }
```

</div>

</div>

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e9c99296-6c0f-486f-9650-04517350dd03" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
private void modifyCSVFile(String filePath, Path dir, String tenantId, CountDownLatch countDownLatch, ExecutorService modifyFileExecutorService) throws ExecutionException, InterruptedException {
        Future<?> task = modifyFileExecutorService.submit(() -> {
            LOGGER.info("[modifyCSVFile#modifyFileExecutorService] thread name: " + Thread.currentThread().getName() + " execute at: " + System.currentTimeMillis());
            Path tempFile = Paths.get(dir.toFile().getAbsolutePath() + File.separator + UUID.randomUUID().toString() + ".csv");
            Path source = Paths.get(filePath);

            try (Reader reader = new BufferedReader(new FileReader(filePath));
                 CSVParser parser = new CSVParser(reader, CSVFormat.DEFAULT.withFirstRecordAsHeader());
                 BufferedWriter writer = new BufferedWriter(new FileWriter(tempFile.toFile()))) {

                List<CSVRecord> records = parser.getRecords();
                Map<String, Integer> headers = parser.getHeaderMap();
                Integer auditDataIndex = headers.get(AUDIT_DATA_CSV_HEADER_TITLE);
                if (auditDataIndex == null) {
                    return;
                }

                writer.write(CsvBuilder.HEADER + "\n");
                for (int i = 0; i < records.size(); i++) {
                    String encryptedAuditData = records.get(i).get(auditDataIndex);
                    String decryptedData = null;
                    if (StringUtils.isNotBlank(encryptedAuditData)) {
                        decryptedData = decryptAuditData(encryptedAuditData, tenantId);
                    }
                    String line = CsvBuilder.modifyCSVLine(records.get(i), headers.size(), auditDataIndex, decryptedData);
                    writer.write(line + "\n");
                }
                writer.flush();

                Files.move(tempFile, tempFile.resolveSibling(source.getFileName()),
                        StandardCopyOption.REPLACE_EXISTING);
            } catch (Exception e) {
                LOGGER.warning("[modifyCSVFile] can not modify csv file: " + ExceptionUtils.getStackTrace(e));
                throw new AuditException(BundleConstant.CAN_NOT_MODIFY_CSV_FILE);
            } finally {
                FileUtils.deleteQuietly(tempFile.toFile());
                countDownLatch.countDown();
            }
        });

        try {
            task.get();
        } catch (Exception e) {
            throw e;
        }
    }
```

</div>

</div>

# 4. Measuring

**DEV-VN**

Context:

- tenant id: 114f1fba-1520-420c-a495-7acea20d1dde

- `STORE_CSV_AUDIT_LOG_FILE_NUMBER_THREAD`: 4

- `MODIFY_CSV_AUDIT_LOG_FILE_NUMBER_THREAD`: 4

- Number of audit logs file: 365 files

- File size: 40.3 kb

- Number of rows per file: 101 (include header)

- pods: 1

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>No.</strong></p></th>
<th><p><strong>Time consumption</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><ul>
<li><p>total time consumption: 343619ms (5 Minutes 43 Seconds)</p></li>
</ul></td>
</tr>
<tr>
<td><p>2</p></td>
<td><ul>
<li><p>total time consumption: 308828ms (5 Minutes 8 Seconds)</p></li>
</ul></td>
</tr>
<tr>
<td><p>3</p></td>
<td><ul>
<li><p>total time consumption: 289460ms (4 Minutes 49 Seconds)</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

**DEV**

Context:

- tenant id: 6000e266-71e1-4973-a895-a0c9a70eb5f9

- `STORE_CSV_AUDIT_LOG_FILE_NUMBER_THREAD`: 4

- `MODIFY_CSV_AUDIT_LOG_FILE_NUMBER_THREAD`: 4

- Number of audit logs file: 365 files

- File size: 40.3 kb

- Number of rows per file: 101 (include header)

- pods: 4

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p><strong>No.</strong></p></th>
<th><p><strong>Time consumption</strong></p></th>
</tr>
&#10;<tr>
<td><p>1</p></td>
<td><ul>
<li><p>total time consumption: 189190ms (3 Minutes 9 Seconds)</p></li>
</ul></td>
</tr>
<tr>
<td><p>2</p></td>
<td><ul>
<li><p>total time consumption: 183711ms (3 Minutes 3 Seconds)</p></li>
</ul></td>
</tr>
<tr>
<td><p>3</p></td>
<td><ul>
<li><p>total time consumption: 183277ms (3 Minutes 3 Seconds)</p></li>
</ul></td>
</tr>
</tbody>
</table>

</div>
