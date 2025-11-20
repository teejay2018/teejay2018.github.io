---
layout: post
title: Project cantaloop - Infrastructure - hardening VM
hidden: true
date: 2025-11-20 08:00:00 +0200
categories: [linux,hetzner]
tags: [linux,datacenter]
---

# 🌍 Project cantaloop - Infrastructure - hardening

*Documenting steps when hardening the new VM.*

### Todo list

> ✅ Initialize firewall.<br>
> ✅ deny all.<br>
> ✅ allow ssh, https, http.<br>

### Steps

Initialize<br>
```bash
firewall
```
Configure the UFW Firewall<br>
Allow Necessary Ports (Critical Step)<br>
Enable the Firewall<br>
Check Status<br>
Disable Direct Root SSH Login<br>
Install Fail2Ban (Brute-Force Protection)<br>

### 💡Still to do

> 🔭 more<br>

---