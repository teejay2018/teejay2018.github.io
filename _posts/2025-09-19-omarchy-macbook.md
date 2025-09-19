---
layout: post
title: Omarchy on intel Macbook Pro
date: 2025-09-19 08:00:00 +0200
categories: [linux,omarchy,macbook]
tags: [omarchy,linux]
---

# Bring old Macbook Pro from 2017 with intel CPU alive and install Omarchy

Omarchy is a super interesting project which is basically streamlining a linux for typical developer

### Steps

Prepare bootable USB with Omarchy ISO V3.0.1
Download from iso.omarchy.org

Ventoy - used to create bootable USB with ISO on Windows 11 laptop


Boot Macbook holding key option - select Boot from USB
Select the ISO if you have more on USB
Select Normal boot

Seems stuck shortly after showing Omarchy logo - no install started

Cancel this and repeat - but select Grub boot 

This seem to work, install steps begin.

After completion, reboot and remove USB.
Entering the password at disk encryption point now works.

No Wifi network is configured during install, instead you do this as first thing
- tab down to the wanted wifi
- select by pressing space
- enter password 

When Wifi is connected click the update to have latest Omarchy via internet

### Shortcuts to begin with

| What                             | Shortcut                          |
|----------------------------------|-----------------------------------|
| Terminal                         | Super + Enter                    |
| Browser                          | Super + B                        |
| Tile jump left or right          | Super + Arrow key left or right  |
| Tile move left or right          | Super + Shift + left or right    |
| Tile split horzontal or vertical | Super + J                        |
| btop                             | Super + T                        |
| Close Window                     | Super + W                        |
| ChatGPT                          | Super + A                        |
| X                                | Super + X                        |
| Launcher                         | Super + Space                    |
| Omarchy Menu                     | Super + Alt + Space              |
| Jump other Space                 |                                  |
| Jump other Screen                | Super + Shift + Tab              |

  
----------------------------------------------
  

Macbook keyboard - need to know  

ALT = Option  
Super = Command  
Ctrl = Control  

