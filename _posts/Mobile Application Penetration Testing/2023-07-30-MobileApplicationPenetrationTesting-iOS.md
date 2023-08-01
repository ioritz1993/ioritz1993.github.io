---
title: Mobile Application Penetration Testing - iOS
author: ioritz_elisa 
date: 2023-07-30 0:0:00 +0000 
categories: [Notes, Mobile Application Penetration Testing, iOS] 
tags: [Automated Analysis, Jailbreak, Manual Static Analysis] 
pin: true
---


## Basic info

It is more challenging to install applications that are not from the official app store (App Store). You would typically need Xcode or jailbreak the device, or use similar methods. In addition, many iPhones have hardware that provides security.

iOS is based on UNIX (Linux). 

iOS has two partitions and you can't see the files even if you create a shell (OS partition and user partition).

The applications are in .iPA format which is similar to APK and some interesting folders are:

/Payload/Application.app: is the application itself
/Payload/iTunesMetadata.plist: info about the app
/Payload/Application.app/Info.plist: important info (similar to Manifest.xml on Android)



## Interesting Tools

> xcode

The iOS apps are usually programmed with xcode so you can ask for the project to analyze it.
You can see the code, permissions, info, open a mobile simulator, etc.

To do dynamic analysis you sometimes need a developer account.
https://developer.apple.com/programs/


> Anytrains

You can use https://www.imobie.com/anytrans/ to connect to your mobile and download apps, do analysis, backup etc. You need Apple ID and password.


> Correlium

[https://www.corellium.com/](https://www.corellium.com/)


> Appetize

[https://appetize.io/](https://appetize.io/)



## Automated Analysis

> MobSF

Install it on MAC and insert the 'ipa' file and it will be scanned.

You have to break SSL pinning on iSO for dynamic analysis. It is more difficult than Android. Usually you need jailbreak the phone and tools like killchain.

SSL Kill Switch Documentation: [https://github.com/nabla-c0d3/ssl-kill-switch2](https://github.com/nabla-c0d3/ssl-kill-switch2)
This is needed to intercept all traffic.

You can use Objection to break it: 
https://www.secjuice.com/objection-frida-guide/ 


Download Objection on MAC:

* You must to view your mac's developer IDs:

```
security find-identify
```

* You must to choose one:

```
objection pathipa - -source <path> -c <idDesarrollador>
```

This is how Frida gets inside the app.

Then a new APP is created and this must be installed on the mobile that is connected to the order and you can put commands like those mentioned in the Android section.



## Jailbreak

It is highly recommended to jailbreake the phone to be able to analyze it, but it is very dangerous because you can make the phone unusable. 

Jailbreaking Link (use with caution): [https://checkra.in/](https://checkra.in/)



## Manual Static Analysis

If we don't have the xcode then we have the .ipa file. We clone it and name it payload.zip and then hit extract info. It extracts a lot of things. In the .app click on 'show packages content' and we see more files.


