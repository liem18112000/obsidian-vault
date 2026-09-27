---
title: "Library to read .eml file"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47158362487/Library+to+read+.eml+file
space: "LUZ"
topic: programming
relevance: 0.842
depth: 3
updated: 2022-08-05
attachments: 7
tags:
  - confluence
  - programming
  - space/luz
---

# Library to read .eml file

> [!info] Imported from Confluence
> Space **LUZ** · updated 2022-08-05 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/47158362487/Library+to+read+.eml+file)
> Relevance 0.842 · topic `programming`

The library that i choose to read/extract eml file is apache commons email

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1706c040-8f25-491d-95b4-f3eaaaf23480" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-email</artifactId>
    <version>1.5</version>
</dependency>
```

</div>

</div>

Example code to extract content from .eml file:

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="52503d49-107b-465f-9cfa-d0a4252b9966" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public static void main(String[] args) throws Exception {
        String filePath = "files/emlFile_3.eml";
        File file = getDocumentFile(filePath);
        MimeMessage mimeMessage = MimeMessageUtils.createMimeMessage(null, file);
        MimeMessageParser parser = new MimeMessageParser(mimeMessage);

        MimeMessageParser parsed = parser.parse();
        
        Collection<String> htmlAttachs = parsed.getContentIds();
        Map<String, String> mapAttachments = new HashMap<String, String>();
        for (String string : htmlAttachs) {
            DataSource dataSource = parsed.findAttachmentByCid(string);
            byte[] sourceBytes = IOUtils.toByteArray(dataSource.getInputStream());
            String encodedString = Base64.getEncoder().encodeToString(sourceBytes);
            String prefix = "data:"+dataSource.getContentType()+";base64,";
            mapAttachments.put("cid:"+string, prefix + encodedString);
        }
        
        if (parsed.hasPlainContent()) {
            System.out.println("plain text:\n" + parsed.getPlainContent());
        } 
        if (parsed.hasHtmlContent()) {
            String htmlContent = parsed.getHtmlContent();
            for (Map.Entry<String, String> entry : mapAttachments.entrySet()) {
                htmlContent = htmlContent.replaceAll(entry.getKey(), entry.getValue());
            }
            System.out.println("html text:\n" + htmlContent);
        }
        
        //writeHeaders(parser);
        
        if (parsed.hasAttachments()) {
            //Getting and saving attachments from eml
            List<DataSource> attachments = parsed.getAttachmentList();
            for (DataSource attachment : attachments) {
                if (attachment.getName() != null && !attachment.getName().isEmpty()) {
                    try (InputStream is = attachment.getInputStream()) {
                        File save = new File("D:\\TestEML" + File.separator + attachment.getName());
                        FileOutputStream fos = new FileOutputStream(save);
                        byte[] buf = new byte[4096];
                        int bytesRead;
                        while ((bytesRead = is.read(buf)) != -1) {
                            fos.write(buf, 0, bytesRead);
                        }
                        fos.close();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    private static void writeHeaders(MimeMessageParser parser) throws Exception {
        System.out.println("From :" + parser.getFrom() + "\n");
        System.out.println("To：" + parser.getTo() + "\n");
        System.out.println("Subject：" + parser.getSubject() + "\n");
        System.out.println("Message:" + "\n" + "\n");
    }
    
    private static File getDocumentFile(String filePath) {
        return new File(Thread.currentThread().getContextClassLoader().getResource(filePath).getPath());
    }
```

</div>

</div>

Example eml file:

<span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="6f9ad481-8c90-41ed-8ee2-9a45ab60281b" macro-name="view-file"><a href="../_attachments/47158362487-emlFile_3.eml" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/47158362487/emlFile_3.eml?version=1&amp;modificationDate=1659668240311&amp;cacheVersion=1&amp;api=v2" data-mime-type="message/rfc822" data-has-thumbnail="true">

![[47158362487-emlFile_3.eml]]

</a></span>

After extracted


![[47158362487-image-20220805-030118.png]]



In these files, the red ones is the image for the html content like this image:


![[47158362487-image-20220805-030434.png]]



so for these images, they relate to the html content, team arrow will take care to parse into base 64 content then insert into html content and build final html


![[47158362487-image-20220805-031037.png]]




![[47158362487-chrome_Grv4xHIPuk.gif]]



remaining attachments are **IncaMail.html** and **smime.p7s**

This is the content when open using outlook that we can compare:


![[47158362487-image-20220805-031135.png]]
