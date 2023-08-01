---
title: Steganography 
author: ioritz_elisa 
date: 2023-07-28 0:0:00 +0000 
categories: [Notes, Steganography] 
tags: [Metadata, Steganography, Binaries, Thumbnail] 
pin: true
---


## Metadata

Metadata refers to the data that provides information about other data. In the context of pentesting, metadata often includes details about files, such as creation date, author, software used, and version information, which can be valuable for assessing security risks and conducting reconnaissance during penetration testing.

### Interesting Tools

> Exiftool

Tool for view the metadata of the image or file.

Syntax:

```
exiftool <file>
```

To extract the thumbmail:

```
exiftool -b -ThumbnailImage <image> > >name_thumbnail.jpg>
```

To extract binaries:

```
exiftool -b -RedTRC <image>
```


> FOCA

FOCA (Fingerprinting Organizations with Collected Archives) is a software tool for extracting and analyzing metadata from files to gather information about organizations.

Download: https://github.com/ElevenPaths/FOCA



## Steganography

Steganography is the practice of concealing secret information within a seemingly innocent carrier medium, such as an image, audio, video, or text file. It involves embedding the hidden data in a way that is not easily detectable without prior knowledge of the method used. Steganography is often used for covert communication and information protection in cybersecurity and intelligence operations.

### Interesting Tools

> steghide

Installation:

```
apt-get install steghide
apt-get install stegosuite
```

Command to display information about a file whether it has embedded data or not:

```
steghide info <picture.jpg>
```

Extracts embedded data from a file:

```
steghide extract -sf <picture.jpg>
```


> Foremost

Foremost is a program that recovers files based on their headers , footers and internal data structures , I find it useful when dealing with png images.

It is used to recover deleted files, for example in a USB:

```
foremost -t <fileType> -i <input> -v -o <outputName>
```

* -t: specify file type (examples: all, jpeg, pdf...).
* -i: specify input file.
* -v: verbose mode.
* -o: set output directory.

 If you don’t know your partition, type the following in the terminal:
 
```
df -h
```


> Strings

The command 'strings' extracts readable strings from an image or file:

```
strings <image.jpg>
```


> Exiftool

Extract information from a file:

```
exiftool <image.jpg>
```

Save thumbnail image:

```
exiftool -b -ThumbnailImage <image.jpg> > <nameThumbnail.jpg>
```


> Binwalk

It is a tool for searching binary files, like images and audio files for embedded files and data.

Displays the embedded data in the given file:

```
binwalk <file>
```

Displays and extracts the data from the given file:

```
binwalk -e <file>
```


> Zsteg

It is a tool that can detect hidden data in png and bmp files.

Download it:

```
gem install zsteg
```

Runs all the methods on the given file:

```
zsteg -a <file>
```


> Wavsteg

That tool that can hide data and files in wav files and can also extract data from wav files.

Extracts data from a wav sound file and outputs the data into a new file:

```
python3 WavSteg.py -r -s <soundfile> -o <outputfile>
```


> StegCracker

A tool that bruteforces passwords using steghide.

Syntax:

```
stegcracker <file> <wordlist>
```

Github: https://github.com/Paradoxis/StegCracker



## Vulnerability Exiftool - version 12.37

> Create a image with payload:

* Exploit: [https://github.com/UNICORDev/exploit-CVE-2021-22204](https://github.com/UNICORDev/exploit-CVE-2021-22204)

* Metasploit: unix/fileformat/exiftool_djvu_ant_perl_injection


> This version is vulnerable to RCE in the filename parameter.


