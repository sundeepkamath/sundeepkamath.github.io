---
layout: single
title: "Using Web Applications as Native Desktop Applications in Windows"
date: 2018-09-07 22:07:00 +0530
permalink: /posts/using-web-applications-as-native-desktop-applications-in-windows/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- Nativefier
- Web Applications
- Native Applications
- Desktop Applications
---

Ever felt the need to install some useful web applications as **native desktop applications** in **Windows**? Well, I've always been on the lookout for desktop version of some well known online applications like **TweetDeck** for instance. While the web version of it is quite good, I prefer to use the desktop version and since there is none available (atleast officially from TweetDeck), I decided to look out online for options.

I came across this **command line tool** called **Nativefier**, which is written in **JavaScript** and uses [Electron](https://github.com/electron/electron) to make native applications for Windows, Mac and Linux. It gives you an app like experience, and can preserve configuration and settings.

## How to Install Nativefier ?
* Install the latest version of **Node.js** from [here](https://nodejs.org/en/.). I would recommed you install the latest **LTS** version.
* Verify node is installed using 
{% highlight powershell %}
C:\Users\sundeep>node -v
v8.11.4
{% endhighlight %}

* Install Nativefier
{% highlight powershell %}
npm install -g nativefier
{% endhighlight %}
* Verify Nativefier is installed using 
{% highlight powershell %}
C:\Users\sundeep>nativefier -v
v7.6.7
{% endhighlight %}

## Install the desktop version of a Web site
* In my case, I will try to install the desktop version of **TweetDeck** website.
{% highlight powershell %}
nativefier --name “TweetDeck” “https://tweetdeck.twitter.com/”
{% endhighlight %}

This will download a bunch of files from the **TweetDeck website** and will also download the **Electron app**, if it is not already installed. The generated native application would be created in a folder with the application name, in this case `TweetDeck`.
You can open the folder, locate the **executable** and execute it. This will open up the application as a **desktop client**.

That's it!! You can go ahead and create a desktop shortcut to this executable. The great part is that all the applications generated using **Nativefier** are ***portable***. You can easily carry them around with all their configuration. This makes sure you have your data everywhere and prevents you from logging again and again. You could read up more on **Nativefier** [here](https://github.com/jiahaog/nativefier).

Hope this was useful!!