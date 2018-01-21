---
layout: single
title: "Opening Microsoft Edge from the Run dialog"
date: 2017-03-15 09:02:00 +0530
permalink: /posts/opening-microsoft-edge-from-the-run-dialog/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Tricks and Tips
tags:
- Browser
- Microsoft Edge
- Shortcut
---

Are you someone who prefers using keyboard shortcuts to open up applications? I always tend to use them wherever possible. The latest browser from Microsoft's stable - **Edge** has been released from sometime now (exclusively on Windows 10). I found it to be light weight and decent enough for browsing. 

### The problem:
One quirk that I find is that you can't trigger it directly from the *Run dialog* in windows (by default), like the way one could open Internet Explorer by typing `iexplore` from the *Run dialog*. 

However, there is a slight tweak that can be performed to make this possible. With this tweak you could type `edge` in the *Run dialog* and open up Microsoft Edge.

### Steps to make this possible:
* Type `shell:Appsfolder` in the *Run dialog* and press enter. This would open up the Applications folder.
* Search for **Microsoft Edge** application in this folder.
* Right click on this **Microsoft Edge** application and create a shortcut to it. Name this shortcut as **edge**
* Copy this shortcut and place it in the **System32** folder i.e. (`C:\Windows\System32`)

That's all!! We are good to go now...
Open the *Run dialog* and type edge in it to open up Microsoft Edge browser.

Hope this was useful!