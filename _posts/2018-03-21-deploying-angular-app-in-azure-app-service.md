---
layout: single
title: "Deploying an Angular Web Application in Azure App service from VSTS"
date: 2018-03-21 17:57:00 +0530
permalink: /posts/deploying-an-angular-web-application-in-azure-app-service-from-vsts/
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
- Visual Studio Code
- VSTS
- npm
---

In my earlier blog, we saw how we could create an **Angular web** app using **Angular CLI** and host the same in **IIS**. In this article we'll take it forward and figure out how the same can be deployed to a **App service** in Azure. If you haven't checked out that article I would suggest you do so and follow all steps till step number 3 which involves ***Host this Angular application locally from console***.

We'll carry this out in 5 steps...
1. Create a App service in Azure
2. Check in our code to VSTS
3. Create a build definition to build the Angular app
4. Create a release definition to deploy the Angular app onto Azure App service
5. Verify the deployment

## 1. Create a App service in Azure
Let's first create an App service in Azure whereby we can deploy the Angular application.

![Create App Service step 1]({{site.url}}/assets/images/blogs/1CreateAppService.jpg)

![Create App Service step 1]({{site.url}}/assets/images/blogs/2CreateWebApp.jpg)

## 2. Check in our code to VSTS
While the App service gets created, lets checkin the angular app code created in the earlier article into **VSTS (git repository)**.

![Checked in code]({{site.url}}/assets/images/blogs/3CheckedInCode.jpg)

## 3. Create a build definition to build the Angular app
Lets now create a build definition to build the above angular app code.
While creating a new build definition, select an **empty process** template.

![Select build definition template]({{site.url}}/assets/images/blogs/4BuildDefinitionTemplate.jpg)

Next, add a **npm intall** task to install packages for the Angular application.

![npm task to install node packages]({{site.url}}/assets/images/blogs/5BuildDefinitionBuildNPM.jpg)

Add another **npm task** to build the application and create the `dist` folder. 

![npm task to build the app]({{site.url}}/assets/images/blogs/55BuildDefinitionNpmRun.jpg)

We'll now add a **Publish artifact** task which will generate the `dist` artifact which can be provided as an input to our release definition.

![Publish build artifact]({{site.url}}/assets/images/blogs/6BuildDefinitionPublishArtifact.jpg)

That's all. With this we have a build definition ready to build our angular application. You may now spawn a build.

## 4. Create a release definition to deploy the Angular app onto Azure App service
Now that we have a build created from our build definition, lets create a release definition to deploy the same to the App service we created earlier.
Select the Azure App service deployment template.

![Select release template]({{site.url}}/assets/images/blogs/7ReleaseTemplate.jpg)

Select the build artifact from the earlier build.

![Select build artifact]({{site.url}}/assets/images/blogs/8ReleaseBuildArtifactSelection.jpg)

Select the App Service created earlier in Azure in the deployment task.

![Configure deployment tasks]({{site.url}}/assets/images/blogs/9ReleaseConfigureDeploymentTasks.jpg)

Now that the release definition is created, deploy to Azure.

## 5. Verify the deployment
Post successful deployment we can now try opening our web app in browser...

![Verify successful deployment]({{site.url}}/assets/images/blogs/10VerifySuccessfulDeployment.jpg)


Hope this helps!