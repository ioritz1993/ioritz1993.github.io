---
title: "FindTrack: An OSINT Automation Tool with Graphical Interface"
author: elisa
date: 2022-11-10 0:0:00 +0000 
categories: [Projects, TFE] 
tags: [FindTrack, OSINT, Graphic Interface, Automation Tool, Investigation] 
pin: true
---

![Image-FindTrack](https://raw.githubusercontent.com/ioritz1993/ioritz1993.github.io/main/assets/img/tfe/image-FindTrack.jpg)

FindTrack is a **Linux** tool designed to **automate OSINT** (Open-Source Intelligence) with a **graphical interface**. I (**Elisa Alises Núñez**) created it as part of my **Master's thesis in Cybersecurity**.

The inspiration for this tool came from the significant problem of the high number of cyberattacks recorded today. Its purpose is to simplify OSINT tasks, allowing anyone with basic knowledge to assess the amount of information that can be extracted about themselves.

By doing so, users can become more aware of the considerable risks we constantly face, often without even realizing it. Even seemingly "insignificant" data, such as an email address or phone number, can lead to social engineering attacks, phishing attempts, or even more serious crimes.

Moreover, FindTrack can also contribute to law enforcement investigations and more.

Using social media and providing information about ourselves is not inherently bad, but it should be done with caution.

You can find a DEMO video of the tool on my profile.

PS: Its usage is completely legal, as it relies on open-source intelligence (OSINT).


View DEMO: [Linkedln Video DEMO - FindTrack](https://es.linkedin.com/posts/elisa-alises-n%C3%BA%C3%B1ez-9a4325248_github-creanyx0findtrack-herramienta-activity-7067543690210066433-fSWH)
Download Tool: https://github.com/Creanyx0/FindTrack


## Features

Currently, FindTrack is in its first functional version, with the following capabilities:
- Retrieve social media profiles based on a known username (nick).
- Retrieve social media profiles based on a person's name and surname.
- Obtain Google search results related to a person's name and surname.
- Obtain Google Images URLs related to a person's name and surname.
- Obtain information about a mobile phone, including existence, location, original company, and time zone.
- Retrieve email addresses related to a domain.
- Extract metadata from a photograph.
- Download images, comments, IDs, etc., from an Instagram account (query by nick).
⚠️ The term "person" can also refer to a company or entity. ⚠️


## Installation

FindTrack does not require tokens, access keys, or configurations. Just follow these steps:

```
git clone https://github.com/Creanyx0/FindTrack.git
cd FindTrack 
sudo su 
chmod +x FindTrack.sh 
./FindTrack.sh
```


## Characteristics

- It is entirely interactive and easy to use.
- Includes loading screens and error windows for better user experience.
- Utilizes indicative colors for ease of understanding.
- Provides a button to display the results file content through a terminal in all result windows.
- Allows manual visualization of the report.


## Programming Languages Used

The backend is created with Bash and Python, while the frontend uses Zenity and Yad.

