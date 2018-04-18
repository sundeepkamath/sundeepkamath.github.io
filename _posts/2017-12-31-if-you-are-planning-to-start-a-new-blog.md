---
layout: single
title: "If you are planning to start a new blog ..."
date: 2017-12-31 23:34:00 +0530
permalink: /posts/if-you-are-planning-to-start-a-new-blog/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- Wordpress
- Domain
- Jekyll
- GitHub
- GitHub Pages
- GitHub Repositories
- Markdown
- Disqus
- WSL
---

<p><a href="https://www.codeproject.com/script/Articles/BlogFeedList.aspx?amid=4854419" rel="tag" style="display:none">CodeProject</a></p>

Hi there ! 2017 is nearing end, and well, while we celebrate for the wonderful memories of this year, it's also time to reflect back on the past year and come up with resolutions for the New Year. Personally, I'm happy that I've managed to keep this blog alive. It all started in the year 2015 when I started this blog, and its been glorious 3 years of blogging. If you've been following my blogs, I hope you found them useful all along. 

I'm planning to completely overhaul my blogging platform for the new year. All that would remain same are the published articles, but all else is about to change. Well there are 2 main reasons why I feel this overhaul is necessary.
1. Fresh look provides a fresh perspective and provides a change from monotony
2. (*The main reason*) The new platform will help me reduce maintenance costs of the blog (*better invested in some other pursuits*)

Now, before I share details on the new blogging platform, I would like to share some details on my earlier blogging platform setup.

### Previous blogging platform setup
* Personal blog **domain** hosted on [Bigrock](https://www.bigrock.in/)
* Private **wordpress** hosting for the blog (on [BigRock](https://www.bigrock.in/))

Hosting my blog on **wordpress** was worth it when I had just begun blogging. It had a very good usability, variety of quality blog themes and a plethora of plugins to make it easy to add features to your blog. I loved the **markdown** plugins they provided (which also helped me to migrate my posts to the new platform effortlessly).

In fact, while maintaining this blog I realized that there are certain features that are absolutely crucial if you are serious about blogging.

### Some must have features in a blogging platform
* Good server uptime 
* Fantastic collection of cool themes and nifty plugins
* Markdown support is a bonus
* SEO optimization
* Ability to capture user Comments on the blog posts
* Reasonable purchase and renewal cost to maintain the blog year on year.
* Ability to migrate your content to a new platform, should the need arise in future.

My **wordpress** blogging platform on BigRock provided all of this. However, the annual maintenance cost was about ***5 times*** the web site domain cost. Now, I had 2 options - either think of some ads monetization approach to cover up the costs or think of an alternative to help curb the costs.

Owning a domain is absolutely necessary, and when your existing provider's service is good, it makes sense to stay with them. So this left me with the option to check if there are alternate hosting options. This is when I came across **GitHub Pages** and **Jekyll**.

### What are GitHub Pages and Jekyll?
Lets try to understand both of these and how they are used.

> **GitHub Pages** is a web hosting service offered by GitHub for hosting static web pages. It is integrated with the Jekyll software for static web site and blog generation.

> **Jekyll** is a simple, blog-aware, static site generator perfect for personal, project, or organization sites.

Think of **Jekyll** like a **file based CMS**, without all the complexity. Instead of using databases, Jekyll takes your content, renders **Markdown** and **Liquid templates**, and spits out a complete, static website ready to be served by a web server. 

In a nutshell, 

> **Jekyll** is the ***engine*** behind **GitHub Pages**, which you can use to host sites right from your **GitHub repositories**.

The last part about hosting the web site from your **GitHub Repositories**, is the crux here. It means that as long as you are fine with having your website content in a public repository you can host it for free (no charges). Of course, if you have some sensitive data and plan to keep your website content private you can use GitHub's private repositories. But then you need to pay the appropriate costs.

### Does Jekyll support themes?
Yes, Jekyll does support themes. In fact, there are a number of free and paid Jekyll themes available online. You could check out a few of these [here](https://github.com/jekyll/jekyll/wiki/Themes).

### How to install and deploy using Jekyll?
If you are using windows, you can follow the steps [here](https://jekyllrb.com/docs/windows/) to install Jekyll, and deploy your website using it. Note that Windows is not an officially supported platform for Jekyll. However, there are some tweaks using which you could get it done. On the contrary, it is beneficial if you are using **Windows 10 64-bit OS** with **Windows Subsystem for Linux (WSL)** activated. That way, you can use the bash commandline to install Jekyll. If **WSL** is new to you, you might want to check my [earlier post]({{site.url}}/posts/linux-support-in-windows-10/) that explains it in detail.

### Closing note
If you are creating a new blog from scratch, then this should be pain less. However, if you plan to ***migrate*** your existing blog to this platform, then there could be few challenges. Personally, I was lucky to have used markdown syntax for all posts in my wordpress hosted site. So migration was super easy. However, I did lose out the comments posted on my blog posts. Looking back, there could have been a way to migrate those as well, but I did not wish to invest more time there. 

So before you take the plunge I would suggest you try creating a sample website using this platform, and once you get familiar with it, analyze whether it would be beneficial in your case. 

Hope this was useful!!

### References
* [Jekyll docs](https://jekyllrb.com/)
* [Jekyll GitHub](https://github.com/jekyll/jekyll)
* [GitHub Pages](https://pages.github.com/)
* [Wiki GitHub Pages](https://en.wikipedia.org/wiki/GitHub_Pages)
* [Wiki Jekyll](https://en.wikipedia.org/wiki/Jekyll_(software))
* [GitHub Pages Jekyll Themes](https://github.com/jekyll/jekyll/wiki/Themes)











