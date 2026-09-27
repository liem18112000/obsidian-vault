---
title: "Convert SSL Certificate to various format"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38196094377/Convert+SSL+Certificate+to+various+format
space: "Helios"
topic: security
relevance: 0.794
depth: 2.9
updated: 2019-06-28
attachments: 0
tags:
  - confluence
  - security
  - space/helios
---

# Convert SSL Certificate to various format

> [!info] Imported from Confluence
> Space **Helios** · updated 2019-06-28 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/38196094377/Convert+SSL+Certificate+to+various+format)
> Relevance 0.794 · topic `security`

<u>**Reference link**</u>: <a href="https://www.ryadel.com/en/openssl-convert-ssl-certificates-pem-crt-cer-pfx-p12-linux-windows/" class="external-link" rel="nofollow">https://www.ryadel.com/en/openssl-convert-ssl-certificates-pem-crt-cer-pfx-p12-linux-windows/</a>

  

# <span class="entry-title-primary">OpenSSL – How to convert SSL Certificates to various formats – PEM CRT CER PFX P12 & more</span><span class="entry-subtitle">How to use the OpenSSL tool to convert a SSL certificate and private key on various formats (PEM, CRT, CER, PFX, P12, P7B, P7C extensions & more) on Windows and Linux platforms</span>

  

In this post, part of our <a href="https://www.ryadel.com/en/?q=ssl" class="external-link" rel="nofollow" style="text-decoration: none;">“how to manage SSL certificates on Windows and Linux systems”</a> series, we’ll show how to convert an SSL certificate into the most common formats defined on X.509 standards: the **PEM** format and the **PKCS#12** format, also known as **PFX**. The conversion process will be accomplished through the use of **OpenSSL**, a free tool available for Linux and Windows platforms.

Before entering the console commands of OpenSSL we recommend taking a look to our<a href="https://www.ryadel.com/en/ssl-certificates-standards-formats-extensions-cer-crt-key-pfx-pem-p7b-p7c-pfx-p12/" class="external-link" rel="nofollow" style="text-decoration: none;"><span> </span>overview of<span> </span><strong>X.509</strong>standard and most popular SSL Certificates file formats</a>– **CER**, **CRT**, **PEM**, **DER**, **P7B**, **PFX**, **P12** and so on.

## Installing OpenSSL

The first thing to do is to make sure your system has **OpenSSL** installed: this is a tool that provides an open source implementation of SSL and TLS protocols and that can be used to convert the certificate files into the most popular **X.509 v3** based formats.

## OpenSSL on Linux

If you’re using Linux, you can install OpenSSL with the following **YUM** console command:

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span> <span class="crayon-e" style="color: black;">yum </span><span class="crayon-e" style="color: black;">install </span><span class="crayon-v" style="color: black;">openssl</span> |

</div>

If your distribution is based on **APT** instead of **YUM**, you can use the following command instead:

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">apt</span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">get </span><span class="crayon-e" style="color: black;">install </span><span class="crayon-v" style="color: black;">openssl</span> |

</div>

  

### OpenSSL on Windows

If you’re using Windows, you can install one of the many **OpenSSL** open-source implementations: the one we can recommend is <a href="https://slproweb.com/products/Win32OpenSSL.html" class="external-link" rel="nofollow" style="text-decoration: none;">Win32 OpenSSL by Shining Light Production</a>, available as a *light* or *full* version, both compiled in x86 (32-bit) and x64 (64-bit) modes . You can install any of these versions, as long as your system support them.  
**IMPORTANT:** OpenSSL for Windows requires the <a href="http://www.microsoft.com/downloads/details.aspx?familyid=9B2DA534-3E03-4391-8A4D-074B9F2BC1BF" class="external-link" rel="nofollow" style="text-decoration: none;">Visual C++ 2008 Redistributables</a> runtime in order to work.

OpenSSL is basically a console application, meaning that we’ll use it from the command-line: after the installation process completes, it’s important to check that the installation folder (**C:\Program Files\OpenSSL-Win64\bin** for the 64-bit version) has been added to the system PATH (**Control Panel** \> **System**\> **Advanced **\> **Environment Variables**): if it’s not the case, we strongly recommend to manually add it, so that you can avoid typing the complete path of the executable everytime you’ll need to launch the tool.

Once **OpenSSL** will be installed, we’ll be able to use it to convert our SSL Certificates in various formats.

## From PEM (pem, cer, crt) to PKCS#12 (p12, pfx)

This is the console command that we can use to convert a  **PEM** certificate file (**.pem**, **.cer**or **.crt** extensions), together with its private key (**.key** extension), in a single **PKCS#12** file (**.p12** and **.pfx**extensions):

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs12</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">export</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.crt</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">inkey </span><span class="crayon-v" style="color: black;">privatekey</span><span class="crayon-e" style="color: black;">.key</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pfx</span> |

</div>

If you also have an intermediate certificates file (for example, **CAcert.crt**) , you can add it to the “bundle” using the** -certfile** command parameter in the following way:

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs12</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">export</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.crt</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">inkey </span><span class="crayon-v" style="color: black;">privatekey</span><span class="crayon-e" style="color: black;">.key</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pfx</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">certfile </span><span class="crayon-v" style="color: black;">CAcert</span><span class="crayon-e" style="color: black;">.cr</span> |

</div>

  

## From PKCS#12 to PEM

If you need to “extract” a **PEM** certificate (**.pem**, **.cer** or **.crt**) and/or its private key (**.key**)from a single **PKCS#12** file (**.p12** or **.pfx**), you need to issue two commands.

The first one is to extract the certificate:

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs12</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pfx</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">nokey</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.crt</span> |

</div>

And a second one would be to retrieve the private key:

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs12</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pfx</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">privatekey</span><span class="crayon-e" style="color: black;">.key</span> |

</div>

**IMPORTANT**: the *private key *obtained with the above command will be in encrypted format: to convert it in RSA format, you’ll need to input a third command:

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">rsa</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pfx</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">privatekey_rsa</span><span class="crayon-e" style="color: black;">.key</span> |

</div>

Needless to say, since **PKCS#12** is a password-protected format, in order to execute all the above commands you’ll be prompted for the password that has been used when creating the **.pfx** file.

## From DER (.der, cer) to PEM

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">x509</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">inform </span><span class="crayon-v" style="color: black;">der</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.cer</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pem</span> |

</div>

## From PEM to DER

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">x509</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">outform </span><span class="crayon-v" style="color: black;">der</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pem</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.der</span> |

</div>

## From PEM to PKCS#7 (.p7b, .p7c)

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">crl2pkcs7</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">nocrl</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">certfile </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pem</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.p7b</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">certfile </span><span class="crayon-v" style="color: black;">CAcert</span><span class="crayon-e" style="color: black;">.cer</span> |

</div>

## From PKCS#7 to PEM

<div>

|  |  |
|----|----|
| 1 | <span class="crayon-o">\></span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs7</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">print_certs</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.p7b</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificate</span><span class="crayon-e" style="color: black;">.pem</span> |

</div>

## From PKCS#7 to PFX

<div>

<table style="">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>1<br />
2</p></td>
<td><p><span class="crayon-o">&gt;</span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs7</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">print_certs</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificatename</span><span class="crayon-e" style="color: black;">.p7b</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificatename</span><span class="crayon-e" style="color: black;">.cer</span><br />
<span class="crayon-o">&gt;</span><span class="crayon-h"> </span><span class="crayon-e" style="color: black;">openssl </span><span class="crayon-v" style="color: black;">pkcs12</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-v" style="color: black;">export</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-st">in</span><span class="crayon-h"> </span><span class="crayon-v" style="color: black;">certificatename</span><span class="crayon-e" style="color: black;">.cer</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">inkey </span><span class="crayon-v" style="color: black;">privateKey</span><span class="crayon-e" style="color: black;">.key</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">out </span><span class="crayon-v" style="color: black;">certificatename</span><span class="crayon-e" style="color: black;">.pfx</span><span class="crayon-h"> </span><span class="crayon-o">-</span><span class="crayon-e" style="color: black;">certfile </span><span class="crayon-v" style="color: black;">cacert</span><span class="crayon-e" style="color: black;">.cer</span></p></td>
</tr>
</tbody>
</table>

</div>

## Online SSL Converters

If you can’t (or don’t want to) install **OpenSSL**, you can convert your SSL Certificates using one of these web-based online tools:

- <a href="https://www.sslshopper.com/ssl-converter.html" class="external-link" rel="nofollow" style="text-decoration: none;"><strong>SSL Certificates Converter Tool</strong></a> by <a href="http://SSLShopper.com" class="external-link" rel="nofollow">SSLShopper.com</a>
- **<a href="https://decoder.link/converter/" class="external-link" rel="nofollow" style="text-decoration: none;">SSL Converter</a>** by NameCheap

Both of them work really well and can convert most, if not all, the format detailed above: at the same time, you need to seriously think about the security implications that come with uploading your SSL Certificates (and possibly their private keys) to a third-party service. As trustable and secure those two site have been as of today, we still don’t recommend such move.

## Conclusions

That’s it, at least for the time being: we hope that these commands will be helpful to those developers and system administrators who need to convert SSL certificates in the various formats required by their applications.

See you next time!
