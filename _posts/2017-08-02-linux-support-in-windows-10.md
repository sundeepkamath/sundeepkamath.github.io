---
layout: single
title: "Linux support in Windows 10"
date: 2017-08-02 17:34:00 +0530
permalink: /posts/linux-support-in-windows-10/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Operating Systems
tags:
- Linux
- Windows 10
- Linux Subsystem
- Ubuntu
- Suse
- WSL
---

If you've been following the evolution of Windows 10 over the last few months, you must have also checked out the support for Linux in windows. In case you've not, then yes, you've read that right, Windows 10 (64 bit edition) has indeed been trying to support Linux. Microsoft calls this as **Windows Subsystem for Linux** acronymed as **WSL**.

### What exactly is WSL?
If you check **Wikipedia** for **WSL** it states ...

> **Windows Subsystem for Linux (WSL)** is a compatibility layer for running Linux binary executables (in ELF format) natively on Windows 10.  

> **WSL** provides a **Linux-compatible kernel interface** developed by Microsoft (*containing no Linux kernel code*), which can then run a Linux userland on top of it, such as that of Ubuntu, SUSE or Fedora. 

> Such a userland might contain a Bash shell and command language, with native Linux command-line tools (sed, awk, etc.) and programming language interpreters (Ruby, Python, etc.).

This is still under active development and the subsystem still does not run all the linux software. You could try an *hackish* approach to run graphical applications from Linux in Windows by installing an **X11** server.

### Why exactly is WSL useful?
If you are primarily a developer, you must be aware that a lot of software/tools in the open source world are primarily geared towards the **\*nix** operating systems. For developers who use Windows as a OS platform it becomes difficult to use these tools as is, without workarounds.  

Now you might be wondering why not use a Linux VM, why this new system ? True, you could definitely use the Linux VM. However, **WSL**  has some advantages over the Linux VM.  

**WSL** is,
* *Light-weight*, uses fewer resources than a fully virtualized machine.
* Furthermore, it allows developers to use Linux software on Windows machine, as also use Windows apps and Linux tools on the *same set of files*.

### Hooked? Wanna try installing and using it?
The best way to judge a new feature is to try it out. I would suggest you install **WSL** and check its benefits for yourself. This [article](https://docs.microsoft.com/en-us/windows/wsl/install-win10) can be a good starting point to install the subsystem and get started.

I'm definitely finding **WSL** useful, in fact I'll be trying to draw upon one of the uses of **WSL** in my next post. So stay glued!!

Hope this was useful!!

### References
* [Wikipedia - Windows Subsystem for Linux](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux)
* [WSL installation guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

