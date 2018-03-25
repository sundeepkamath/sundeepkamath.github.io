---
layout: single
title: "Automate Udemy course downloads"
date: 2018-03-25 22:11:00 +0530
permalink: /posts/automate-udemy-course-downloads/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- Python
- Udemy
---

**Udemy** as we know is an online learning provider with quality video courses on vast verticals including programming, marketing, data science etc.
I've been using this learning portal from a long time and have been recently looking at ways to download the personal **paid** video courses for **offline** viewing.

While Udemy does provide options to download the videos on mobile its a chore really copying the same onto the desktop/laptop where I really prefer watching them. Considering this the plan was to find a automated way of downloading the videos online or build a script to achieve the same.

It seems there is already a python package available to do this job. I'll provide the steps here so that you can use the same for your use.

## Install Python on your machine
Download the latest version from [here](https://www.python.org/downloads/release/python-364/).
At the time of writing this post the latest version was 3.6.4

## Install the python module youtube_dl
You may install this using ...
{% highlight python %}
python -m youtube_dl -u <udemy username> -p <udemy password> https://www.udemy.com/<URL of course content page>
{% endhighlight %}

**Note:**  
The URL of the course content page means, go to the udemy course you need to download in your browser and click on the **Course-Content** tab.
Copy the URL from the address bar.

This will download all the videos on to disk.

Hope this helps!!

### References
* [ASP.NET Core Web API help pages with Swagger / Open API](https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger)
* [Python download](https://www.python.org/)



