---
title: Additional 
author: ioritz_elisa 
date: 2023-07-31 0:0:00 +0000 
categories: [Notes, Additional] 
tags: [Save Changes sensitive files, Install Google Chrome in Linux, Compile GCC] 
pin: false
---


## Save changes in /etc/passwd file

The command ":saveas!" is very useful to save changes in a sensitive files:

```
:saveas! /etc/passwd
```



## Install Google Chrome

Steps to install Google Chrome in Linux:

```
wget  https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

```
sudo dpkg -i google-chrome-stable_current_amd64.deb 
```

Locate path:

```
which google-chrome-stable
```



## Compile GCC

Syntax:

```
gcc <file.c> -o <fileOutput> && chmod +x <fileOutput>
```

Example:

```
gcc <kernel_exploit.c> -o <kernel_exploit> && chmod +x <kernel_exploit>
```

```
./kernel_exploit
```


