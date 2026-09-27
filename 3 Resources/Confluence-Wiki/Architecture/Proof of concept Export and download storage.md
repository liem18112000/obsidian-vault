---
ai_hash: 8b9b27b93147e883
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 9
depth: 2.44
entities: []
relevance: 0.711
source: https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47022477625/Proof+of+concept+Export+and+download+storage
space: TP2020
status: reference
tags:
- confluence
- architecture
- space/tp2020
title: '[Proof of concept] Export and download storage'
topic: architecture
type: source
updated: 2021-12-30
---

# [Proof of concept] Export and download storage

> [!info] Imported from Confluence
> Space **TP2020** · updated 2021-12-30 · [open original](https://axonivy.atlassian.net/wiki/spaces/TP2020/pages/47022477625/Proof+of+concept+Export+and+download+storage)
> Relevance 0.711 · topic `architecture`

<div class="toc-macro client-side-toc-macro conf-macro output-block" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6,H7" macro-id="4daeb268-6088-41a1-be6b-857ce2ceaf3c" macro-name="toc">

</div>

First of all, for the API, we can use CompletionStage to response immediately when a user clicks download button, then we can process the flow in the background and send the email to the user the flow is finished.


![[47022477625-image-20211227-034024.png]]



The draft implementation of the PoC goes here: <a href="https://bitbucket.org/axonivy-prod/luz_docs_view_controller/branch/pioneer/luz-61086/export-storage-poc#diff" class="external-link" rel="nofollow">Draft implementation</a>

The download flow will be:

1.  A user clicks download storage button

2.  fetch letters and folders from luz-docs '

3.  zip the letters and the folders

4.  upload the zipped file to Google cloud storage

5.  Generate download URL and send to user email

6.  Update download histories for letters.

# A. Demonstrate

<span class="confluence-embedded-file-wrapper image-center-wrapper">[[47022477625-export storage demo.mp4|export storage demo.mp4]]</span>

# B. Detail

## I. Create files and folders bases on metadata

**We will fetch letters and folders data from luz_docs**

### 1. Create folders.

The solution is to get all custom folders of a tenant. Bases on the response, we will separate the folders into 2 lists, main custom folders list and sub custom folders list.

- Bases on the main custom folders list, we can create main custom folders using common java IO utility and put the id and the path of the created main folders into a map called “folder paths dictionary”.

- Bases on the “folder paths dictionary”, we continue to create sub folders and also put subfolder id and subfolder path to the “folder paths dictionary” so that we can base on the dictionary to put letter in later.

Example code:


![[47022477625-image-20211227-034928.png]]



### 2. Create Files

We will get all letters of the tenant, use java IO utility to create files bases on letter metadata and put it to the root folder. Then we will use the “folder paths dictionary” created from above step to put the files into main folders and subfolders.

## II. Zip files and folders

We can use default Zip related classes to zip files and folders, no additional library needed.

<div id="expander-204893039" class="expand-container conf-macro output-block" hasbody="true" macro-id="3d0d045f-cbea-4350-8799-a40df4640476" macro-name="expand">

<div id="expander-control-204893039" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Example:</span>

</div>

<div id="expander-content-204893039" class="expand-content expand-hidden">

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8b51fcab-b8b8-4f72-b158-733e4e5fe198" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public static void zipFolder(File folder, File zipFile) throws IOException {
        FileOutputStream fileOutputStream = new FileOutputStream(zipFile);
        ZipOutputStream zipOutputStream = new ZipOutputStream(fileOutputStream);
        try {
            processFolder(folder, zipOutputStream, folder.getPath().length() + 1);
        } catch (Exception e) {
            // TODO: handle exception
        }finally {
            zipOutputStream.finish();
            fileOutputStream.close();
            zipOutputStream.close();
        }
    }

    private static void processFolder(File folder, ZipOutputStream zipOutputStream, int prefixLength)
            throws IOException {
        for (File file : folder.listFiles()) {
            if (file.isFile()) {
                ZipEntry zipEntry = new ZipEntry(file.getPath().substring(prefixLength));
                zipOutputStream.putNextEntry(zipEntry);
                try (FileInputStream inputStream = new FileInputStream(file)) {
                    IOUtils.copy(inputStream, zipOutputStream);
                }
                zipOutputStream.closeEntry();
            } else if (file.isDirectory()) {
                ZipEntry zipEntry = new ZipEntry(file.getPath().substring(prefixLength) + File.separator);
                zipOutputStream.putNextEntry(zipEntry);
                zipOutputStream.closeEntry();
                processFolder(file, zipOutputStream, prefixLength);
            }
        }
    }
```

</div>

</div>

</div>

</div>

## III. Upload zipped file to the cloud

The zipped file will be uploaded to the Google cloud storage service. Klara has been using google cloud storage already so we have experience with it.

Firstly we need to create a new Google cloud storage bucket since we want to apply “time to live” rule, and that rule has not supported file level but only bucket level.

After created the bucket, in tab life cycle, we will add a new rule


![[47022477625-image-20211230-073731.png]]



Select delete action


![[47022477625-image-20211230-073814.png]]



Select age for condition and put the days we want the object to live.


![[47022477625-image-20211230-073935.png]]



By that config, the zip files will be deleted after 14 days.

We will call Google cloud storage API and upload the Zip to the correct bucket. The zip file will be inside the tenant folder as below


![[47022477625-image-20211230-074120.png]]



## IV. Handle download histories

After successfully send the download URL to the user email. We will trigger another thread, still using CompletionStage to update the download histories that contains actor and the timestamp for the downloaded letters.

## V. Download mechanism

**We have two proposal solutions:**

### 1. Send the google signed download URL to the user email (no login needed)

with this solution, we will call to google cloud storage API to generate a signed URL that will available in a period of time. Users will not need to login to Klara, they just need to login to their email, click to the link and start downloading and Goggle will handle the download performance.

The the default signing option is the signature v2, there is **no** maximum days specified, we can set it to 14 days.

But with the v4 signature signing option, the URL will only be available in **7 days** in maximum.

<div id="expander-1764870994" class="expand-container conf-macro output-block" hasbody="true" macro-id="d0691427-9b92-4e97-9066-0ad3f13a9ba8" macro-name="expand">

<div id="expander-control-1764870994" class="expand-control">

<span class="expand-control-icon icon"> </span><span class="expand-control-text">Examples:</span>

</div>

<div id="expander-content-1764870994" class="expand-content expand-hidden">

Signature v2

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="99bc7789-a736-4951-b882-d87457586526" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
URL url = storage.signUrl(blobInfo, duration, timeUnit); //defauls signing signature is v2
```

</div>

</div>

Signature v4

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3f5d7e25-175b-4bb6-a561-69ec5ba88cee" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
//Sign with v4 signature, the URL lives in 7 days maximum
URL url = storage.signUrl(blobInfo, duration, timeUnit, Storage.SignUrlOption.withV4Signature());
```

</div>

</div>

</div>

</div>

### 2. Send the Klara url to the user email (Login needed)

With this solution, we will send a Klara URL to the user, for example : <a href="https://dev.klara.tech/storage/download?storagePath=nhannguyen.zip" class="external-link" rel="nofollow">https://dev.klara.tech/storage/download?storagePath=nhannguyen.zip</a>

and when user trigger that URL we can call to backend and have 2 solutions:

a\. Generate a google signed URL to download, but we can set the expiration time shorter, 1 minute for example and redirect the user to the signed URL and let google handle the download

b\. Download the zip file first then return the file stream to start the download, it means Klara will handle the download.

→ Our proposal is the first solution “send the google signed download URL to the user email” since we can save the performance for our system.

%% ai-graph-start %%

**Related notes:**
- [[Long exports acknowledge immediately, deliver by emailed link to object storage]]
- [[Download user audit logs export files]]
- [[CROSS-TEST LUZ-159442 Implement real ZIP download for eArchive folders]]
- [[KLARA Documents Concept - Solution Design]]
- [[Architecture]]

%% ai-graph-end %%