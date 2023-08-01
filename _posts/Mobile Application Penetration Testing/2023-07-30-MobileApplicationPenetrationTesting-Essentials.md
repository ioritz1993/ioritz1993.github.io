---
title: Mobile Application Penetration Testing - Essentials
author: ioritz_elisa 
date: 2023-07-30 0:0:00 +0000 
categories: [Notes, Mobile Application Penetration Testing, Essentials Mobile] 
tags: [Intercept Traffic, MASTG, Dump Memory] 
pin: false
---


## Intercepting Traffic with Burpsuite

* Go to phone settings, click in Wi-FIi options and configure the proxy.
* Give a Burp certificate error when browsing with the phone.
* Go to the url where the burp is from the mobile and click on allow in the certificate.
* Then go to settings, general and download the certificate.
* Then enable it and that's it.

> Other option is to use Proxyman.



## OWASP Mobile Application Security Testing Guide (MASTG)

https://github.com/OWASP/owasp-mastg



## Dump Memory

## Fridump

Fridump is a tool used in penetration testing and reverse engineering to **extract and dump sensitive data**, such as credentials and encryption keys, **from the memory of Android and iOS** mobile applications.

> Syntax:

```
fridump [-h] [-o dir] [-u] [-H HOST] [-v] [-r] [-s] [--max-size bytes] process
```

Download: https://github.com/rootbsd/fridump3


