---
layout: post
title: Cantaloop operation - Infrastructure, services, applications
date: 2026-02-12
categories: [vm,cantaloop,hetzner,linux]
tags: [linux,vm,infrastructure, operation]
---

# 🌍 Day to day operation of Cantaloop infrastructure and service.

*Notes for operation, restart, upgrade reconfigure, renew, monitor, anything related*

---
# 🧾 Operation, restart, upgrade, update, monitor, maintenance notes

*Build chapters for each topic**

**Core OS**
**Certbot**
**Nextcloud**
**More*

---

## 1) 🎯 Core OS - Ubuntu Linux

---

## 2) 🧠 Certbot
### 2.1 TLS in use
- cantaloop.dk
- elastic.cantaloop.dk
- llm.cantaloop.dk
- nextcloud.cantaloop.dk

### 2.2 Onboard, Status, Dryrun, Renew, Delete
- create nginx and request TLS via certbot.
-

---

## 3) 🧠 Nextcloud
### 3.1 settings
- Install dir /var/www.nextcloud, subdomain nextcloud.cantaloop.dk, hosted by my nginx
- Its mostly PHP depending on package **php8.3-fpm**
- and a database on my mariadb with name **nextcloud**
- Data is located under **/var/www/nextcloud/data**
- Log files to be seen here **/var/www/nextcloud/data/nextcloud.log**
- Services
  - sudo systemctl status nginx
  - sudo systemctl status php8.3-fpm
  - systemctl status mariadb

### 3.2 Upgrade
- Web based via Admin -> Settings -> Click Open updater
- Click Start update
- Running the /updater
- Can also do from CLI with ``cd /var/www/nextcloud; ./occ upgrade``

---

_Last updated: February 2026_  
_Source: Cantaloop Aps._
