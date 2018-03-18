---
layout: single
title: "Hosting an Angular Web application in IIS server"
date: 2018-03-18 22:31:00 +0530
permalink: /posts/hosting-angular-application-in-iis-server/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- Angular
- Node
- Angular CLI
- IIS
- Visual Studio Code
---

ASP.Net Core provides a number of project templates out of the box, one of which is **ASP.Net Core with Angular**. You can use this to bootstrap your **Angular SPA** from within **.Net core** application and host it within IIS. However, many prefer using **Angular CLI** to develop an Angular application due to its simplicity and power and as part of this post I'll be focusing on hosting this in IIS. In case you are new to Angular, that's fine too as I'll be covering the basic steps required to create an Angular application.

We'll approach this in 4 steps
1. Install Node and Angular CLI 
2. Create a simple Angular SPA (hello-world application)
3. Host this Angular application locally from console
4. Host this Angular application in IIS

## 1. Install Node and Angular CLI 
We'll first create a **hello world** Angular application. You'll need to have **nodejs** installed on your machine. Check if node is installed on your machine using 

{% highlight powershell %}
node --version
{% endhighlight %}

In case you don't have node installed on your machine, you may download the latest version from [here](https://nodejs.org/en/). Install the **LTS** version preferably. At the time of writing this blog the latest **LTS** version of Node was `v8.10.0`
The node installation also comes with a package manager called **npm (Node Package Manager)**. We'll use this to install other 3rd party libraries.

We'll first install **Angular CLI** using npm. 

{% highlight powershell %}
npm install -g @angular/cli
{% endhighlight %}

This will install Angular CLI, to verify the same we can use ...

{% highlight powershell %}
PS C:\dev\AngularInIIS> ng --version

Angular CLI: 1.7.3
Node: 8.10.0
OS: win32 x64
Angular:
...
PS C:\dev\AngularInIIS>
{% endhighlight %}

This will show the installed version of **Angular CLI**, **Node** and a few other libraries. We can now use this Angular CLI to create an Angular application.

## 2. Create a simple Angular SPA
We'll create a hello world application using Angular CLI. 
It will 
1. Create a new Angular project
2. Generate some boiler plate code
3. Create deployable packages

{% highlight powershell %}
ng new hello-world
{% endhighlight %}

This will create the new project and install the dependent packages using npm.

## 3. Host this Angular application locally from console
Now that the basic application is ready we'll host the application from console using `ng serve` ....

{% highlight powershell %}
PS C:\dev\AngularInIIS\hello-world> ng serve
** NG Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **
Date: 2018-03-18T16:03:02.250Z
Hash: a99bdded108353b66e5f
Time: 7041ms
chunk {inline} inline.bundle.js (inline) 3.85 kB [entry] [rendered]
chunk {main} main.bundle.js (main) 18 kB [initial] [rendered]
chunk {polyfills} polyfills.bundle.js (polyfills) 549 kB [initial] [rendered]
chunk {styles} styles.bundle.js (styles) 41.5 kB [initial] [rendered]
chunk {vendor} vendor.bundle.js (vendor) 7.42 MB [initial] [rendered]

webpack: Compiled successfully.
{% endhighlight %}

You can now open the browser and browse the url `http://localhost:4200/`. You'll find the hello world page as follows ...

![Console hosted Angular app]({{site.url}}/assets/images/blogs/Console_Hosted.jpg)

## 4. Host this Angular application in IIS
Hosting the Angular application in IIS will be done through the following steps...
1. Use Angular CLI to create a build artifact
1. Create a Web application in IIS
2. Install URL rewrite module in IIS
3. Add web.config with a URL rewrite rule
4. Use Angular CLI to create a production package

### 1. Use Angular CLI to create a build artifact
We can use Angular CLI to build the angular project and create a build artifact as follows ...

{% highlight powershell %}
ng build
{% endhighlight %}

Once the build completes successfully it publishes all the build artifacts to the dist folder.

![Dist folder]({{site.url}}/assets/images/blogs/DistFolder.jpg)

We'll be hosting the build artifacts from this folder in IIS in the subsequent steps.

### 2. Create a Web application in IIS

Create a new **web site** or **web application** or **virtual directory** in IIS to host the Angular application. Anyone of these will do. For the sake of this walkthrough we'll create a new website `SPA` and create new web application `HelloWorld` under it.

![SPA website]({{site.url}}/assets/images/blogs/WebSiteCreation.jpg)

![Hello World web application]({{site.url}}/assets/images/blogs/WebApplicationCreation.jpg)

![Hierarchy in IIS]({{site.url}}/assets/images/blogs/IISHierarchy.jpg)

### 3. Install URL rewrite module in IIS

This step is required to support **deep-linking**. **Deep-linking** is the capability for the user to navigate directly to a page by typing the route into the address bar instead of using the **Angular routing**. Deep-linking causes a problem for IIS because the URL that the user attempts to access is not known to the server and therefore the user receives a `404` response. The solution is for the server to always return the root of the application, even if the user requests a path within the application.

Install the ***URL rewrite module*** from this link - `https://www.iis.net/downloads/microsoft/url-rewrite`

After installing you should see a new icon in IIS Manager.

![URL rewrite module in IIS]({{site.url}}/assets/images/blogs/IISURLRewriteModule.jpg)

### 4. Add web.config with a URL rewrite rule
We'll now add a `web.config` file in which we'll have the **URL rewrite rule**. All requests to this web application that are not for files or folders should be redirected to the **root** of the application. For an **web application** or **virtual directory** under the **default web site**, the URL should be set to the alias, (e.g. `/MyApp/`). For a **web site** at the **root of the server**, the URL should be set to `/`.

In our case since we are using the web application `HelloWorld` we'll set the url `/HelloWorld/`

{% highlight xml %}
<configuration>
<system.webServer>
  <rewrite>
    <rules>
      <rule name="Redirect all requests" stopProcessing="true">
        <match url=".*" />
        <conditions logicalGrouping="MatchAll">
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
        </conditions>
        <action type="Rewrite" url="/HelloWorld/" />
        <!--<action type="Rewrite" url="/" />-->
        </rule>
    </rules>
  </rewrite>
</system.webServer>
</configuration>
{% endhighlight %}

We'll create this `web.config` in `src` folder and to ensure that this file gets copied to `dist` folder each time a build is generated we'll also make an entry in the `assets` section in `angular-cli.json`.

![Web config file entry in assets section]({{site.url}}/assets/images/blogs/WebConfigInAssetsSection.jpg)

We can now build the angular project and verify that the web.config file is indeed published in the `dist` folder.
Once confirmed, there is one more step pending. We need to ensure that the base href value in index.html has the value `/` if we host the application directly in the website or `/MyApp/` if it is hosted in the web application under the web site. In our case this value should be `/HelloWorld/`

![Base href in index.html]({{site.url}}/assets/images/blogs/BaseHrefIndex.jpg)

We can update the above manually, or during creation of the build using the parameter `--base-href`

{% highlight powershell %}
ng build --base-href "/HelloWorld/"
{% endhighlight %}

With this we can now browse the url `http://localhost/HelloWorld/` from the browser. This should load our hello world page.

![Base href in index.html]({{site.url}}/assets/images/blogs/IIS_Hosted.jpg)


Hope this was useful!

### References
* [Deploying Your Angular Application To Azure Using Visual Studio Team Services (VSTS)](https://blogs.msdn.microsoft.com/wael-kdouh/2017/09/11/deploying-your-angular-application-to-azure-using-visual-studio-team-services-vsts/)
* [Tips for Running an Angular app in IIS](https://blogs.msdn.microsoft.com/premier_developer/2017/06/14/tips-for-running-an-angular-app-in-iis/)
* [Deployment of Angular 2 App on IIS](https://www.c-sharpcorner.com/forums/deployment-of-angular-2-app-on-iis)

































