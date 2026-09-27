---
title: "View PDF in side the app on Android"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134180714/View+PDF+in+side+the+app+on+Android
space: "GRAVITY"
topic: programming
relevance: 0.786
depth: 3
updated: 2022-06-24
attachments: 1
tags:
  - confluence
  - programming
  - space/gravity
---

# View PDF in side the app on Android

> [!info] Imported from Confluence
> Space **GRAVITY** · updated 2022-06-24 · [open original](https://axonivy.atlassian.net/wiki/spaces/GRAVITY/pages/47134180714/View+PDF+in+side+the+app+on+Android)
> Relevance 0.786 · topic `programming`

## **Situation**

By default WebView could not show the pdf file.

## **Solution**

Using Google Doc to show content of pdf file.

## **Implementation**

Create an interface OnWebViewClientListener.java to handle when pdf file loaded.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="74d36461-a7ab-4ff8-8e99-ba3b3b7af998" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public interface OnWebViewClientListener {
    void handleDocumentLink(String url);
}
```

</div>

</div>

Create a class FFWebViewClient extends WebViewClient and override method shouldOverrideUrlLoading.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="81b1e2df-4e6d-4592-89aa-82ea04df86aa" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class FFWebViewClient extends WebViewClient {

    private OnWebViewClientListener webViewClientListener;

    public FFWebViewClient(OnWebViewClientListener listener) {
        super();
        webViewClientListener = listener;
    }

    @Override
    public boolean shouldOverrideUrlLoading(WebView view, WebResourceRequest request) {
        super.shouldOverrideUrlLoading(view, request);
        String url = request.getUrl().toString();
        //detect pdf file
        if (url.endsWith(".pdf")) {
            webViewClientListener.handleDocumentLink(url);
            return true;
        }
        return false;
    }
}

```

</div>

</div>

Create a fragment have WebView and implement OnWebViewClientListener

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fa3345d6-804d-41fe-bf84-5a9bcc015f45" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class WebViewActivity extends AppCompatActivity implements OnWebViewClientListener {

    @BindView(R.id.wv_main)
    WebView mainWebView;

    @Override
    public View onCreateView(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main_web_view);

        ButterKnife.bind(this);

        ffWebViewClient = new FFWebViewClient(this);
        mainWebView.setWebViewClient(ffWebViewClient);
    }

    @Override
    public void handleDocumentLink(String url) {
       showDialogViewPdf(url);
    }

    private void showDialogViewPdf(String url) {
       DialogViewPdf dialog = new DialogViewPdf(this, url);
       dialog.show();
    }
}
```

</div>

</div>

Layout activity_main_web_view

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a614d389-5ac2-4b8d-8705-b20ae29b68b7" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/colorPrimaryDark"
    tools:context=".fragments.WebViewFragment">

    <WebView
        android:id="@+id/wv_main"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="center"
        android:background="@color/colorWhite"
        android:focusable="true"
        android:focusableInTouchMode="true" />

</FrameLayout>

```

</div>

</div>

  

Create a DialogViewPdf extends Dialog, we will custom action in this file.

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="4763f566-4b79-4136-846d-7ed18eb64c49" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
public class DialogViewPdf extends Dialog {

    private FFWebView dialogWebView;
    private ImageButton btnCancel;
    private String LOG_TAG = DialogViewPdf.class.getSimpleName();
    private String pdfUrl;

    public DialogViewPdf(Context context, String url) {
        super(context, R.style.full_screen_dialog);
        this.pdfUrl = url;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.view_pdf_dialog);
        setupLayout();
    }

    private void setupLayout() {
        btnCancel = findViewById(R.id.btnCancel);
        btnCancel.setOnClickListener(v -> onCancelClick());

        dialogWebView = findViewById(R.id.dialogWebView);
        dialogWebView.getSettings().setBuiltInZoomControls(true);
        dialogWebView.getSettings().setDisplayZoomControls(false);

        String goDocEmbeddedUrl = "https://docs.google.com/viewer?embedded=true&url=";
        String webViewUrl = goDocEmbeddedUrl + pdfUrl;
        dialogWebView.loadUrl(webViewUrl);

        dialogWebView.setWebViewClient(new WebViewClient() {
            @Override
            public void onPageStarted(WebView view, String url, Bitmap favicon) {
                super.onPageStarted(view, url, favicon);
            }

            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                /* this function to remove the google doc toolbar*/
                dialogWebView.loadUrl("javascript:(function() { " + "document.querySelector('[role=\"toolbar\"]').remove();})()");

            }
        });
    }

    private void onCancelClick() {
        this.dismiss();
    }
}
```

</div>

</div>

Layout of view_pdf_dialog 

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fd165716-5c0d-497f-9dbc-9446bcf2367d" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/viewPdfDialog"
    android:layout_width="match_parent"
    android:background="@color/colorBgDialog"
    android:layout_height="match_parent">

    <RelativeLayout
        android:id="@+id/relativeLayout"
        android:layout_width="match_parent"
        android:layout_height="42dp"
        android:background="@color/colorGray"
        android:orientation="horizontal"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <ImageButton
            android:id="@+id/btnCancel"
            android:layout_width="42dp"
            android:layout_height="42dp"
            android:paddingLeft="5dp"
            android:layout_alignParentStart="true"
            android:adjustViewBounds="false"
            android:background="@color/colorTrans"
            android:scaleType="fitCenter"
            app:srcCompat="@drawable/icon_cancel" />
        
    </RelativeLayout>

    <WebView
        android:id="@+id/dialogWebView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/relativeLayout"
        android:background="@color/colorGray" />

    <ProgressBar
        android:id="@+id/pr_dialog_webview"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:layout_gravity="center"
        android:indeterminateTint="@color/colorPrimary" />

</RelativeLayout>
```

</div>

</div>

## Result


![[47134180714-Screenshot_20190128-1024232222.png]]
