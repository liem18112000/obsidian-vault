---
title: "Copy 2. QR-Code forwarding to correct app store"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49307549932/Copy+2.+QR-Code+forwarding+to+correct+app+store
space: "LUZ"
topic: programming
relevance: 0.812
depth: 3
updated: 2026-04-08
attachments: 10
tags:
  - confluence
  - programming
  - space/luz
---

# Copy 2. QR-Code forwarding to correct app store

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-04-08 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49307549932/Copy+2.+QR-Code+forwarding+to+correct+app+store)
> Relevance 0.812 · topic `programming`

***Research question***

- *How can the QR-Code process after scanning the QR-Code work (process flow)? (Not the QR-Code itself, but: what happens when the QR-Code link (i.e. public API) is called)*

- *How can we forward to the corresponding app store of the client device* (custom landing page (in the <a href="http://epost.ch/" class="external-link" rel="nofollow">http://epost.ch</a> domain) if it’s not a mobile device)?

- *What new implementations do we need for it (public API endpoint, …)*

- *In which module is this implemented? Is a new one required? (Architecture)*

- The QR-Code generator created in the Hackathon seems to have different “levels”. The lowest level would only be the forwarding to the correct app store (without any verification) → can we reuse it for our case?

***QR-Code process***


![[49307549932-Blank diagram (2).png]]



***Forward to app store***

\- Define URL for App Store/ Google Play Store  

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="643b148f-ee66-4a89-9287-e8e312b9e428" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class RedirectionService implements Serializable {
    private static final long serialVersionUID = 6065851447914081843L;
    
    @Inject
    private HttpServletRequest request;
    
    @Inject
    @ConfigProperty(name = "epost.app.android.download.url", 
    defaultValue = "https://play.google.com/store/apps/details?id=ch.klara.epost_dev")
    private String epostAndroidAppDownloadURL;

    @Inject
    @ConfigProperty(name = "epost.app.ios.download.url",
            defaultValue = "")
    private String epostiOSAppDownloadURL;
    
    @Inject
    @ConfigProperty(name = "epost.landingpage.url",
    defaultValue = "")
    private String klaraEpostLandingpageUrl;
}
```

</div>

</div>

\- Check user agent of client request

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e2483582-aa14-491a-8115-22de37fe3f4d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
redirect(androidAppDownloadURL, iOSAppDownloadURL) {
        var userAgent = navigator.userAgent || navigator.vendor || window.opera;
        if (/android/i.test(userAgent)) {
            window.location = androidAppDownloadURL;
        }

        if (/iPad|iPhone|iPod/.test(userAgent) && !window.MSStream) {
            window.location = iOSAppDownloadURL;
        }
        if (!((/android/i.test(userAgent)) ||(/iPad|iPhone|iPod/.test(userAgent))){
            //redirect to custom landing page
        }
}
```

</div>

</div>

***Implementation***


![[49307549932-image-20240116-110335.png]]



***Reference***

luz-pulic-api-adapter

- Main page when the api /eletter/qr-code is called: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="cfc5f5cf-da49-4196-abe2-6ddbcbd11bb3" macro-name="view-file"><a href="../_attachments/49307549932-eletter-qr.xhtml" class="confluence-embedded-file" data-nice-type="null" data-file-src="/wiki/download/attachments/49307549932/eletter-qr.xhtml?version=1&amp;modificationDate=1775630513416&amp;cacheVersion=1&amp;api=v2" data-mime-type="application/xhtml+xml" data-has-thumbnail="true">

![[49307549932-eletter-qr.xhtml]]

</a></span>

- Logic check user agent: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="55a05362-f2a5-460d-896f-f22ec2f6f1ff" macro-name="view-file"><a href="../_attachments/49307549932-eletter-qr.js" class="confluence-embedded-file" data-nice-type="JavaScript File" data-file-src="/wiki/download/attachments/49307549932/eletter-qr.js?version=1&amp;modificationDate=1775630513181&amp;cacheVersion=1&amp;api=v2" data-mime-type="text/javascript" data-has-thumbnail="true">

![[49307549932-eletter-qr.js]]

</a></span>

- Define url for Appstore/Google Play Store: <span class="confluence-embedded-file-wrapper conf-macro output-inline" hasbody="false" macro-id="56794625-fb07-4fb8-8206-fd65ee1844a6" macro-name="view-file"><a href="../_attachments/49307549932-RedirectionService.java" class="confluence-embedded-file" data-nice-type="Java Source File" data-file-src="/wiki/download/attachments/49307549932/RedirectionService.java?version=1&amp;modificationDate=1775630513180&amp;cacheVersion=1&amp;api=v2" data-mime-type="binary/octet-stream" data-has-thumbnail="true">

![[49307549932-RedirectionService.java]]

</a></span>

***Conclusion***

- *How can the QR-Code process after scanning the QR-Code work (process flow)? (Not the QR-Code itself, but: what happens when the QR-Code link (i.e. public API) is called)*

Answer: The flow is demonstrated in Figure 1

- *How can we forward to the corresponding app store of the client device* (custom landing page (in the <a href="http://epost.ch/" class="external-link" rel="nofollow">http://epost.ch</a> domain) if it’s not a mobile device)?

Answer: We check the user agent of client request, then decide the destination we should redirect to.

- *What new implementations do we need for it (public API endpoint, …)*

Answer:

\- We can refer the implementation of ***eletter QR code scanning*** which is implemented in ***luz-public-api-adapter.***

\- We need to add one more redirection destination to ePost web page to our implementation.

- *In which module is this implemented? Is a new one required? (Architecture)*

Answer: ***luz-public-api-adapter***

- The QR-Code generator created in the Hackathon seems to have different “levels”. The lowest level would only be the forwarding to the correct app store (without any verification) → can we reuse it for our case?

Answer: We will use the existing endpoint and modify it to add one more option to redirect to custom landing page((in the <a href="http://epost.ch/" class="external-link" rel="nofollow">http://epost.ch</a> domain)) if it’s not a mobile device.
