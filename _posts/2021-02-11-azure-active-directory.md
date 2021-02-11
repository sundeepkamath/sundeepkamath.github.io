---
layout: single
title: "Azure Active Directory"
date: 2021-02-11 21:00:00 +0530
permalink: /posts/azure-active-directory/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- azure
- active directory
- IAM
- identity
- access
- cloud
---

In this post I'll talk about what Azure Active Directory is and the critical role it plays w.r.t identity and access management in the Azure space.

![Post Banner]({{site.url}}/assets/images/blogs/active_directory/1_ActiveDirectoryHeader.png)

## What is Azure Active Directory?
> **Azure Active Directory (AAD)** is Microsoft’s enterprise cloud-based ***identity and access management (IAM)*** solution. Its primary purpose is to provide **authentication** and **authorization** for applications in the cloud.

## How is AAD different from Windows Active Directory?
**Windows Active Directory** first release in Windows 2000 server, live on-premise in servers called **Domain Controllers (DC)**. Each DC contains a catalog of users and computers that are authorized to access resources on the network. Users authenticate to DCs via **Kerberos** or **NTLM** authentication.

## How AAD differs from Windows AD?
Both **AAD** and **Windows AD** are fundamentally different systems that exist in an interconnected enterprise environment. The only commonality between them is that both are IAM systems.

||AAD | Windows AD |  
||--------|--------|  
|Communication|REST APIs|LDAP (Lightweight Directory Access Protocol)|  
|Authentication|Cloud based protocols (OpenID/OAuth, SAML etc.)|Kerberos, NTLM|  
|Network Organization|Flat structure of users and groups|Organizational units, domains and forests|  
|Management|Admins organize users into groups|Admins or data owners assign users to groups|
|Devices|Mobile device management|No mobile device management|   

## When to use AAD over Windows AD and vice-versa?
If you are starting a ***brand new*** organization from scratch, Azure AD could meet all of your needs, especially if you plan on using an entirely cloud-based infrastructure.  

If you are already running ***an established enterprise network***, you most likely already have Windows AD, and you might be planning to add AAD to manage your cloud infrastructure (Hybrid scenario).  

In case of ***hybrid scenario*** it makes sense to use **Azure AD Connect**, which syncs data between the on-premise DCs and the cloud. Azure AD Connect also provides password hash synchronization, pass-through authentication, federation, and health monitoring. Those features allow your users to have the same user id and password, on-premise and in the cloud.

## What is the difference between SSO (Single Sign On) and Federation
**SSO** allows a single authentication credential to access different systems within a *single organization*, while, a **federated identity management** system provides single access to *multiple systems across different enterprises*.

## What is the difference between AAD B2B and AAD B2C?
**B2B** and **B2C** are different features provided by the same AAD instance.

* **AAD B2B (Business-to-Business)**  
This allows a company to ***invite members from other organizations*** to share application access. When you invite a user to your application, they will get access using their Azure AD account. No need to create another account for them. No need for a new password. They sign on to your app with their credentials. You are still in control of your application. You decide if it requires multi-factor authentication. You choose who has access.

* **AAD B2C (Business-to-Consumer)**
The purpose of Azure AD B2C is to allow organizations to build a cloud identity directory for their customers. It’s built to ***allow anyone to sign up*** as a service user with their email or social media provider like Facebook, Google or LinkedIn. 
It is an identity repository in the cloud that allows your users to sign up for your applications with an email address and password (no restrictions on the email domain) or social media logins. The service itself handles all the processes like sign-up, sign-in, password reset and so on. You don’t have to worry about it.

Hope this was useful!!

## References
* [Microsoft docs](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis)
* [Varonis - Jeff Petters](https://www.varonis.com/blog/azure-active-directory/)
* [Predicagroup - Tomasz Onyszko](https://www.predicagroup.com/blog/azure-ad-b2b-b2c-puzzled-out/)  
* [SSO vs Federation - Stuart Kwan](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/what-is-single-sign-on)
