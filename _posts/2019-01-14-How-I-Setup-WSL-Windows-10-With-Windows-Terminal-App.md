---
layout: post
title: Creating an OpenCV 4 Project in Visual Studio 2017 & 2019
category: General
tags: [Windows, Windows 10, Windows Terminal, WSL, ZSH]
excerpt_separator: <!--more-->
---

I’ve had a long history of not being a fan of the Windows operating system. While Windows 7 was a decent release of the OS, version eight was an absolute dumpster fire and the early builds of Windows 10 weren’t much better. The more recent builds of Windows 10, however, have certainly started to win me over, especially when Microsoft surprised everyone by coming out with the <a href="https://docs.microsoft.com/en-us/windows/wsl/install-win10" target="_blank">Windows Subsystem for Linux</a>.

The default setup for WSL is anything but perfect but it’s possible to get a really nice looking, highly functional setup with just a little bit of work. Let’s take a look at how I have everything set up.

![_config.yml]({{ site.baseurl }}/images/windows-terminal/WIndowsConsole.png)

<!--more-->

# Windows Terminal

The first thing we want to do is get rid of the God awful ConHost terminal that WSL uses by default. Luckily, Microsoft is working on rolling out a new Windows Terminal application that is much more modern. It’s still marked as a preview application, but it has been working great for me. You can download it from the <a href="https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab" target="_blank">Microsoft App Store</a>.

![_config.yml]({{ site.baseurl }}/images/windows-terminal/SplitPanes.png)

The new terminal app allows you to fully customize it, supports tabs and even allows you to split the view into multiple panes without needing a tool like Tmux.

# Windows Terminal Settings

As I said before, the new Terminal application is fully customizable. You adjust the settings via a json file, which makes everything extremely easy to tweak and backup. Rather than diving into all of the options, I’ll just share my personal configuration here. There’s a whole lot that you can play with in here!

```json
// To view the default settings, hold "alt" while clicking on the "Settings" button.
// For documentation on these settings, see: https://aka.ms/terminal-documentation
{
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "defaultProfile": "{2c4de342-38b7-51cf-b940-2309a097f518}",
    "showTerminalTitleInTitlebar" : true,
    "profiles":
    [
    {
            "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
            "hidden": false,
            "name": "Ubuntu",
            "source": "Windows.Terminal.Wsl",
            "colorscheme": "One Half Dark",
            "alwaysShowTabs": true,
            "fontFace": "Ubuntu Mono derivative Powerline",
            "fontSize" : 11,
            "historySize": 9001,
            "padding": "15, 9, 15, 9",
            "useAcrylic": false
        },
        {
            // Make changes here to the powershell.exe profile
            "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
            "name": "Windows PowerShell",
            "commandline": "powershell.exe",
            "hidden": false,
            "colorscheme": "Solarized Dark",
            "fontSize": 10
        },
        {
            // Make changes here to the cmd.exe profile
            "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
            "name": "cmd",
            "commandline": "cmd.exe",
            "hidden": false,
            "fontSize": 10,
            "colorscheme": "Campbell"
        },
        {
            "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
            "hidden": true,
            "name": "Azure Cloud Shell",
            "source": "Windows.Terminal.Azure"
        }
    ],
    // Add custom color schemes to this array
    "schemes": [],
    // Add any keybinding overrides to this array.
    // To unbind a default keybinding, set the command to "unbound"
    "keybindings": [
        {
            "command": "closeTab",
            "keys": [
                "ctrl+w"
            ]
        },
        {
            "command": "newTab",
            "keys": [
                "ctrl+t"
            ]
        },
        {
            "command": "newTabProfile0",
            "keys": [
                "ctrl+shift+1"
            ]
        },
        {
            "command": "newTabProfile1",
            "keys": [
                "ctrl+shift+2"
            ]
        },
        {
            "command": "newTabProfile2",
            "keys": [
                "ctrl+shift+3"
            ]
        },
        {
            "command": "nextTab",
            "keys": [
                "ctrl+tab"
            ]
        },
        {
            "command": "prevTab",
            "keys": [
                "ctrl+shift+tab"
            ]
        },
        {
            "command" : "switchToTab0",
            "keys" : 
            [
                "alt+1"
            ]
        },
        {
            "command" : "switchToTab1",
            "keys" : 
            [
                "alt+2"
            ]
        },
        {
            "command" : "switchToTab2",
            "keys" : 
            [
                "alt+3"
            ]
        },
        {
            "command" : "switchToTab3",
            "keys" : 
            [
                "alt+4"
            ]
        },
        {
            "command" : "switchToTab4",
            "keys" : 
            [
                "alt+5"
            ]
        },
        {
            "command" : "switchToTab5",
            "keys" : 
            [
                "alt+6"
            ]
        },
        {
            "command" : "switchToTab6",
            "keys" : 
            [
                "alt+7"
            ]
        },
        {
            "command" : "switchToTab7",
            "keys" : 
            [
                "alt+8"
            ]
        },
        {
            "command" : "switchToTab8",
            "keys" : 
            [
                "alt+9"
            ]
        }
    ]
}
```

# ZSH And Oh My ZSH

My shell of choice is ZSH with <a href="https://ohmyz.sh/" target="_blank">Oh My ZSH</a> installed to help make ZSH a bit nicer. To get started, you need to install some base dependencies:

```
sudo apt install zsh git
```

Next up we need to install Oh My ZSH, which can be done by simply running the following command:

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Finally, we can set ZSH as the default shell in WSL. To do this, edit your **~/.bashrc** file to include the following at the top:

```
if test -t 1; then
    exec zsh
fi
```

# Configuring The ZSH Theme

My personal theme of choice for ZSH is agnoster, so I change the ***ZSH_THEME*** variable in my **~/.zshrc** file to use *agnoster*. When you do this, you’ll notice that there are some issues caused by the fonts used by the theme. You can fix this by installing the power line fonts, which you can get by cloning the git repository and running the install script in a PowerShell with admin privileges:

```
git clone https://github.com/powerline/fonts.git
.\install.ps1
```

**NOTE:** You’ll notice in the Windows Terminal config that I included at the start of this post that I have it set to use a powerline font for the Ubuntu session.

# Fixing Directory Colors

By default, using ls will result in some really ugly colors that makes the text extremely hard to read. I fixed this by installing a Solarized color theme for the directory color outputs. First, we need to obtain the color scheme:

```
curl https://raw.githubusercontent.com/seebi/dircolors-solarized/master/dircolors.ansi-dark --output ~/.dircolors
```

Next, add the following to the **~/.zshrc** file:

```
# Set directory colors for ls
eval `dircolors ~/.dircolors`
```