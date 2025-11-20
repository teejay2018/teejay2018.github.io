---
layout: post
title: Project cantaloop - Infrastructure - Hetzner VM
hidden: true
date: 2025-11-20 08:00:00 +0200
categories: [linux,hetzner]
tags: [linux,datacenter]
---

# 🌍 Project cantaloop - Infrastructure - Hetzner VM

*Documenting steps when onboarding a VM in Hetzner cloud.*

### Onboarding Hetzner VM

> ✅ Sign up with account, added 2FA via Google Authenticator.<br>
> ✅ Account and password stored in wallet.<br>
> ✅ Smallest server requested - CX33 - 4 VCPU - 8 GB RAM - 80 GB disk - Max 6.86 EUR pr month.<br>
> ✅ Storage volume 100 GB assigned to VM - Max 5.5 EUR pr month.<br>
> ✅ OS = Ubuntu 24.10<br>
> ✅ Upload ssh key for initial root access, so we can skip emailing password.<br>
> ✅ Location: Falkenstein<br>

### Access with ssh key

Generate new key on my device - WSL Ubuntu user tom<br>
```bash
ssh-keygen -t ed25519
<set passphrase? no just press enter>
```
Copy key from /home/tom/.ssh/id_ed25519.pub<br>

First login and set password for root<br>
root password stored in wallet.<br>
```bash
ssh root@<IP>

passwd root
```

### Initial update linux and setup daily user

```bash
sudo apt update
sudo apt upgrade

#Add user
adduser tom
usermod -aG sudo tom

# While logged in as root on the server:
mkdir -p /home/tom/.ssh
cp /root/.ssh/authorized_keys /home/tom/.ssh

# Set correct ownership and permissions (CRITICAL for SSH key auth to work)
chown -R tom:tom /home/tom/.ssh
chmod 700 /home/tom/.ssh
chmod 600 /home/tom/.ssh/authorized_keys
```
User tom password stored in wallet.<br>
Verify access works with daily user<br>
```bash
ssh tom@<IP>

whoami
sudo whoami
```

### 💡Still to do

> 🔭 disable password login to root (/etc/ssh/sshd_config)<br>
PasswordAuthentication no<br>
PermitRootLogin no<br>
restart service = sudo systemctl restart ssh<br>
> 🔭 Hardening and firewall in next chapter<br>

---