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

When Wifi is connected you click the update to have latest Omarchy via internet

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

Super = Command  
ALT = Option  
Ctrl = Control  

----------------------------------------------
Web Apps

Tried creating a few home made web apps

First my own blog = https://blog.cantaloop.dk<br>
3 step process under Install, Web Apps<br> 
- give it name (Toms blog),<br>
- URL<br>
- and URL for icon (PNG) (This you can find at dashboardicons for example)

This worked flawless and then I wanted to do same with Office (https://office.com) including login with my work account.<br>
After this I did create Outlook as well as Web App - using the URL https://outlook.office.com/mail/<br>

---------------------------------------------

I noticed that sound was not working out of the box after Omarchy install. Within the sound setup window under configuration I could only see HDA Intel PCH with off and only unavailable choices<br>

Searching the discussion part of https://github.com/basecamp/omarchy gave me this article = https://github.com/basecamp/omarchy/discussions/714<br>
Following instructions in README.md under https://github.com/davidjo/snd_hda_macbookpro I installed Cyrrus driver

```bash
pacman -S gcc linux-headers make patch wget
```

```bash
git clone https://github.com/davidjo/snd_hda_macbookpro.git
cd snd_hda_macbookpro/
#run the following command as root or with sudo
./install.cirrus.driver.sh
reboot
```
After reboot go to sound setup window<br>
Here you now find a list of available choices under HDA Intel PCH<br>
I choose the first - Analog Stereo Duplex - which worked fine with the build in speakers<br>
Build in microphone also work.

--------------
Question - how is Apple keys for brigtness and sound working?


