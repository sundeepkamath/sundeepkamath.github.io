---
layout: single
title: "Setting up the new Windows Terminal (Preview)"
date: 2019-12-01 18:13:00 +0530
permalink: /posts/setting-up-the-new-windows-terminal/
author_profile: true
comments: true
toc: true
toc_label: "Contents"
categories: 
- Blog
tags:
- Windows Terminal Preview
- Windows 10 1909
- Visual Studio Developer Command Prompt
- Copy Paste Keyboard bindings
---

I've been using the all new **Windows Terminal (Preview)** from a few weeks now, and I definitely find it promising. In case you've not already used it you can download it from [Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) or also from [GitHub](https://github.com/microsoft/terminal/releases).

There were however a couple of things I found not available out of the box in the terminal
* Option to spawn **Visual Studio Developer** command prompt 
* Ability to **copy/paste** text in the CLI using the **default keyboard shortcuts**

However, even though these were not pre-configured in the terminal, Microsoft has provided a configuration file, where you can configure and make these available for your use. 

It's fairly straight forward and this is how you can enable these...

## Integrating the Visual Studio Developer command prompt 
Once you install the terminal on Windows 10, and launch it, you'll find the following default set of CLI's supported.

![Win 10 terminal Default CLI's supported]({{site.url}}/assets/images/blogs/1winterminal_defaultmenuoptions.png)

You can open the configuration file by clicking on the **Settings** option in the menu. This will open up a `profiles.json` file. You can scroll down to the `profiles` section in the file where all the existing CLI's are pre-configured. Just append the following configuration snippet to the existing list of profiles.

{% highlight javascript %}
{
    "commandline": "cmd.exe /k \"%PROGRAMFILES(X86)%\\Microsoft Visual Studio\\2019\\Enterprise\\Common7\\Tools\\VsDevCmd.bat\"",
    "guid": "{26b30263-74e9-4146-b80e-11632e86d42c}",
    "historySize": 9001,
    "name": "VS2019 Dev Prompt",
    "icon": "C:\\terminal\\icon\\vs-48.png"
}
{% endhighlight %}

The path you use in the `commandline` section will depend on the version of Visual Studio you are using. Also the `icon` section will be a custom path where you have a visual studio icon. I downloaded this from the following [site](https://icons8.com/icon/set/visual-studio/all). 

If all goes well, you will see an additional option in the menu for the **Visual Studio Developer** Command prompt with the **custom icon**.

![Win 10 terminal Default CLI's supported]({{site.url}}/assets/images/blogs/2winterminal_vscliwithiconmenu.png)

When you click the new Visual Studio CLI option the CLI should load as follows ...

![Win 10 terminal Default CLI's supported]({{site.url}}/assets/images/blogs/3winterminal_vscli.png)


## Ability to copy/paste text in the CLI using default keyboard shortcuts
As you start using the terminal, you'll notice that common options like **copy/paste** with the default key bindings `Ctrl+C/Ctrl+P` don't work as expected. The **Settings** menu suggests you use `Ctrl+Shift+C` to copy selected text and `Ctrl+Shift+v` to paste the text back from clipboard.

To get this working open the same **Settings** menu option as earlier that will load the `profiles.json` file.

Scroll to the `keybindings` section and append the json configuration below at the end of it.

{% highlight javascript %}
{
    "command" : "copy",
    "keys" : 
    [
        "ctrl+c"
    ]
},
{
    "command" : "paste",
    "keys" : [
        "ctrl+v"
    ]
}
{% endhighlight %}

When this step is done right, 
* **Highlighting/marking/selecting** text on the CLI and pressing `Ctrl+C` will copy the text
* Once copied, pressing `Ctrl+P` will paste the text from clipboard to the target
* If no text is **highlighted/marked/selected** on the CLI, pressing `Ctrl+C` will cancel the command.

Hope this was useful!!