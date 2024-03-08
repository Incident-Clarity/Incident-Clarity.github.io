+++
title = 'Laptop Battery'
date = 2023-10-25T09:23:30+05:30
draft = false
author = "aayan"
showauthor = false
tags = ["Linux"]
categories = ["Linux", "Laptop", "Battery"]
+++

> 25th October, 2023

## Laptop battery tool 

In the early days, when I switched from Windows to Linux on my laptop, I wasn't able to manually monitor my laptop fan profiles for temperature issues.

So, I found these tools on the AUR: `chillspot` and `slimbookbattery`

Since I had just recently started using **Hyprland** and `chillspot` required root privileges, I was getting an error.

```
(chillspot:176865): Gtk-CRITICAL **: 00:00:23.393: gtk_icon_theme_get_for_display: assertion 'GDK_IS_DISPLAY (display)' failed

(chillspot:176865): Gtk-CRITICAL **: 00:00:23.393: gtk_icon_theme_add_resource_path: assertion 'GTK_IS_ICON_THEME (self)' failed

(chillspot:176865): Gtk-CRITICAL **: 00:00:23.394: gtk_icon_theme_get_for_display: assertion 'GDK_IS_DISPLAY (display)' failed

(chillspot:176865): Gtk-CRITICAL **: 00:00:23.394: gtk_icon_theme_add_resource_path: assertion 'GTK_IS_ICON_THEME (self)' failed
RuntimeError: Gtk couldn't be initialized. Use Gtk.init_check() if you want to handle this case.
```

My guess is that it was due to `chillspot` bring an **x11** application, it won't run on **Wayland**.

## My solution:


`sudo -E chillspot`


The `-E` (or `--preserve-env`) option preserves the user's environment when running the command. This means that if your user session has access to an X11 display, using `sudo -E` might pass these environment variables (such as `DISPLAY` and `XAUTHORITY`) to the chillspot application running as root, potentially solving the issue.


### Conclusion

While `sudo -E chillspot` might temporarily solve the problem by ensuring environment variables are preserved, it's not the most secure or recommended solution.

---

At the end, it worked out for my usecase.