---
layout: post
title: Project cantaloop - Infrastructure - hardening VM
hidden: true
date: 2025-11-20 08:00:00 +0200
categories: [linux,hetzner]
tags: [linux,datacenter]
---

# 🌍 Project cantaloop - Infrastructure - hardening

*Documenting steps when hardening the new VM. Goal is to block incomming traffic and allow outgoing traffic. Incomming traffic allowed on port 80, 442 and 22*

### Todo list

> ✅ Setup firewall.<br>
> ✅ Adjust ssh security<br>
> ✅ Setup brute-force protect<br>
> ✅ Generate security report with SSL Labs<br>

### Setting up firewall

I am using ufw (Uncomplicated Firewal) on my linux server<br>

Set rules<br>
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```
Checking status before enable says nothing, instead I can verify setup by looking at rules files<br>
```bash
sudo ufw status verbose
 
sudo cat /etc/ufw/user.rules
sudo cat /etc/ufw/user6.rules
```
Enable firewall<br>
```bash
sudo ufw enable
```
Checking status now has info:<br>
```bash
sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443                        ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
443 (v6)                   ALLOW IN    Anywhere (v6)
```
Verify by exit current ssh session and start new ssh session<br>

### Disable direct root ssh login

Edit config /etc/ssh/sshd_config<br>
```bash
sudo vi /etc/ssh/sshd_config
#edit and remove # from line
PermitRootLogin prohibit-password

sudo systemctl restart ssh
```
### Install brute-force protect

```bash
sudo apt install fail2ban -y
local configuration file
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```
Check status:<br>
```bash
sudo systemctl status fail2ban
[sudo] password for tom:
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/usr/lib/systemd/system/fail2ban.service; enabled; preset: enabled)
     Active: active (running) since Fri 2025-11-21 12:18:03 UTC; 2 days ago
       Docs: man:fail2ban(1)
   Main PID: 2527 (fail2ban-server)
      Tasks: 5 (limit: 9255)
     Memory: 33.6M (peak: 36.8M)
        CPU: 3min 56.327s
     CGroup: /system.slice/fail2ban.service
             └─2527 /usr/bin/python3 /usr/bin/fail2ban-server -xf start

Nov 21 12:18:03 cantaloop systemd[1]: Started fail2ban.service - Fail2Ban Service.
Nov 21 12:18:03 cantaloop fail2ban-server[2527]: 2025-11-21 12:18:03,614 fail2ban.configreader   [2527]: WARN>
Nov 21 12:18:03 cantaloop fail2ban-server[2527]: Server ready
```

### 💡 Run security report
> 🔭 To verify SSL certificate settings, domain settings and more<br>

Use SSL Labs (https://www.ssllabs.com/ssltest/)<br>

![All looks fine](/assets/images/security-report.png)

---