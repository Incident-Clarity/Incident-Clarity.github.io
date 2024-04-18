---
title: GlazeWM Guide
date: 2024-04-18T17:21:20+05:30
url: /glazeWM-windows/
categories:
  - Windows
tags:
  - Blog
  - Guide
  - Open-Source
  - Windows
draft: false
author: aayan
showauthor: false
showDateOnlyInArticle: false
showDateUpdated: true
showHeadingAnchors: false
showPagination: false
showReadingTime: true
showTableOfContents: true
showTaxonomies: true
showWordCount: true
sharingLinks: false
showEdit: false
showViews: false
showLikes: false
showSummary: true
summary: Tiling window manager for Windows
---
<!--more-->

Recently, I came across this Tiling window manager for Windows called ***GlazeWM***. Written in C#, it uses the Windows API under the hood for positioning windows. 

You can checkout the project here: https://github.com/glzr-io/glazewm.

Features: 
- Multi-monitor support
- Customizable bar window
- Customizable rules for specific windows
- Simple YAML configuration
- Easy installation

The GlazeWM project is much better than the old [SackWM](https://losttech.software/stack.html). 

Also, <kbd>[Fancy Zones](https://learn.microsoft.com/en-us/windows/powertoys/fancyzones)</kbd> from PowerToys does more or less the same thing as StackWM. 
## Installation
---

### Winget (Recommended)
---
```sh
winget install GlazeWM
```

Winget installs portable packages in `%LOCALAPPDATA%\Microsoft\Winget\Packages\` by default. This can be overridden with the flag `--location \path\to\folder`.
### Scoop
---

The [Extras](https://github.com/ScoopInstaller/Extras) bucket.

```sh
scoop bucket add extras # Ensure bucket is added first
scoop install glazewm
```

### GitHub Releases
---

Download the latest `.exe` from the [releases page](https://github.com/glzr-io/glazewm/releases/https://github.com/glzr-io/glazewm/releases/).

Example: [GlazeWM_x64_2.1.1.exe](https://github.com/glzr-io/glazewm/releases/download/v2.1.1/GlazeWM_x64_2.1.1.exe)

## Configuration 
---

We can customize the GlazeWM using the `config.yaml` file which is present at this path.
```
C:\Users\{username}\.glaze-wm\config.yaml
```

If config.yaml is not at that location, use the [default config](https://github.com/glzr-io/GlazeWM/blob/master/GlazeWM.App/Resources/sample-config.yaml).

### Keybindings
---

Customize the keybinds using the `keybindings` property in the config file. Keybinding is the use of one or more keys to run one or more commands when pressed.

The full list of keys that can be used for keybindings is [here](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.keys?view=windowsdesktop-5.0#fields).

By default, the <kbd>Alt</kbd> key is the mod key for keybindings. The Windows key is hard to remap.


```yaml
keybindings:
  # Command to run.
  - command: "focus workspace 1"

    # Key combination to trigger the keybinding.
    binding: "Alt+1"

  # To run multiple commands in a sequence, use the `commands` property (eg. to move a window to a
  # workspace + focus workspace).
  - commands: ["move to workspace 1", "focus workspace 1"]
    binding: "Alt+Shift+1"

  - command: "focus left"
    # To have multiple key combinations that can trigger a command, use the `bindings` property.
    bindings: ["Alt+H", "Alt+Left"]
```


### Official Keybindings
---

Keybindings with <kbd>Alt</kbd> pressed:

![Alt key pressed - with key bindings](https://user-images.githubusercontent.com/34844898/194635035-152ed4a6-e5a1-4878-8863-f62391e7d703.png)

Keybindings with <kbd>Alt</kbd>+<kbd>Shift</kbd> pressed:

![Alt+shift key pressed - with key bindings](https://user-images.githubusercontent.com/34844898/194635089-d5ed152b-1527-43e8-a69c-4e154b97a207.png)

Apart from the `Alt+Shift+E` binding for exiting GlazeWM, it's also possible to safely exit via the system tray icon.


### Custom Keybindings
---

```yaml
keybindings:
  - command: "exec 'C:\\Program Files\\Thorium\\Application\\thorium.exe'"
    bindings: ["Alt+B"]
  - command: "exec wt" # Windows terminal
    bindings: ["Alt+T"]
  - command: "exec explorer"
    bindings: ["Alt+E"]
```

### Custom Gaps like i3
---

```yaml
gaps:
  # Gap between adjacent windows.
  inner_gap: "2px"

  # Gap between windows and the screen edge.
  outer_gap: "2px"
```


### My Personal Config
---

There are not a lot of changes to my configuration file. But check it out here: [file](https://raw.githubusercontent.com/Incident-Clarity/glazewm/main/config.yaml)


## Final Thoughts 
---

**To-Do's**
- I will be customizing both the WM and the bar much more in the next few days.
- Finding ways (if possible) such that only GlazeWM runs, not the Microsoft Desktop Window Manager (DWM). This will be to reduce the system usage of DWM.
- Making a script for Windows tools and driver installation in a new Windows system. And adding the installation and config file of GlazeWM within it.

**Additional learning**: 
- HTML keyboard input `<kbd>key</kbd>` Tag
	- <kbd>Alt</kbd> + <kbd>Tab</kbd>
	- [Web-Reference](https://www.w3schools.com/tags/tag_kbd.asp)
