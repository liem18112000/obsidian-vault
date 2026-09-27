---
title: "Intel Security cordova plugin"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38180333816/Intel+Security+cordova+plugin
space: "Helios"
topic: security
relevance: 0.721
depth: 2.8
updated: 2018-11-02
attachments: 0
tags:
  - confluence
  - security
  - space/helios
---

# Intel Security cordova plugin

> [!info] Imported from Confluence
> Space **Helios** · updated 2018-11-02 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38180333816/Intel+Security+cordova+plugin)
> Relevance 0.721 · topic `security`

Following the plugin from IONIC: <a href="https://ionicframework.com/docs/native/intel-security/" class="external-link" rel="nofollow">https://ionicframework.com/docs/native/intel-security/</a> (deprecated and removed from GitHub)

We have cloned and editted due to the update to Android Codova 7 of IONIC, we edited the plugin.xml file of Android group to copy and push the source code to the right position for Android Project in AndroidStudio structure.

Plugin is place on: luz_myklara/native/com-intel-security-cordova-plugin

To install this plugin run this cmd:

**ionic cordova plugin add ./native/com-intel-security-cordova-plugin --force**

To save data

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b585d675-ae55-4911-bd76-5caf140c9051" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
this.intelSecurity.storage.write({ id: key, instanceID: instanceID })
          .then(data => {
            console.log('Save successfully ' + data);
          }).catch(error => {
            console.log(error);
          });
```

</div>

</div>

  

To read data

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3b3e15d9-9f83-465d-ab89-dd2cfec40557" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
this.intelSecurity.storage.read({ id: key })
      .then(() => {
        console.log('Read successfully');
      }).catch((error) => {
        console.log('Read fail');
      });
```

</div>

</div>

  

To delete data

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9eef8c96-9e49-4e7b-93d2-3ff0b8292c09" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
this.intelSecurity.storage.delete({ id: key })
      .then(() => {
        console.log('Delete successfully');
      }).catch((error) => {
        console.log('Delete fail');
      });
```

</div>

</div>
