---
layout: post
title: Fail2ban - protect your VPS
hidden: false
date: 2025-11-24 08:00:00 +0200
categories: [linux,software]
tags: [linux,datacenter,security,operation]
---

# Fail2ban - protect against hackers on port 22 (ssh)

## Install

## Review

After running for a few month, I wanted to check logs for status and see if any improvements are needed

Logfiles are in /var/log

```bash
ls -l /var/log/aut*
-rw-r----- 1 syslog adm  4746706 Feb 25 13:35 /var/log/auth.log
-rw-r----- 1 syslog adm 10151310 Feb 22 00:00 /var/log/auth.log.1
-rw-r----- 1 syslog adm  1032524 Feb 15 00:00 /var/log/auth.log.2.gz
-rw-r----- 1 syslog adm   746200 Feb  8 00:00 /var/log/auth.log.3.gz
-rw-r----- 1 syslog adm   633942 Feb  1 00:00 /var/log/auth.log.4.gz
```
Check last 100 entries and search for "Failed password" in current log

```bash
sudo tail -n 100 /var/log/auth.log | grep sshd
sudo grep "Failed password" /var/log/auth.log
```

Get top 10 unique IP adress with "Failed password"

```bash
sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | head -n 10
    312 176.120.22.13
    268 2.57.121.25
    228 2.57.121.112
    171 213.209.159.159
    150 2.57.122.208
    107 185.156.73.233
     92 91.202.233.33
     86 2.57.122.96
     84 129.212.185.118
     81 178.128.19.97
```
Get status for banned and failed

```bash
sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 4
|  |- Total failed:     26816
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 0
   |- Total banned:     3287
   `- Banned IP list:
```
Check number of "Ban " and sample 2 hours to investigate close

```bash
sudo grep "Ban " /var/log/fail2ban.log | wc -l
1412

sudo grep "Ban " /var/log/fail2ban.log
2026-02-25 10:04:02,663 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.208
2026-02-25 10:04:20,676 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.96
2026-02-25 10:07:36,776 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 10:08:08,793 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 80.94.92.186
2026-02-25 10:14:00,121 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.47
2026-02-25 10:17:46,861 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 10:20:35,343 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.208
2026-02-25 10:22:00,579 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.96
2026-02-25 10:22:30,598 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.13
2026-02-25 10:25:39,298 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 80.94.92.186
2026-02-25 10:28:02,575 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 10:28:14,586 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.121.25
2026-02-25 10:35:16,000 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.47
2026-02-25 10:35:54,636 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 196.11.86.134
2026-02-25 10:36:55,264 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.121.112
2026-02-25 10:36:58,474 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.208
2026-02-25 10:38:17,744 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 10:39:44,384 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.96
2026-02-25 10:43:09,667 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 80.94.92.186
2026-02-25 10:43:28,284 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 91.202.233.33
2026-02-25 10:48:30,075 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 10:52:47,593 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.13
2026-02-25 10:56:00,898 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.47
2026-02-25 10:57:20,934 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.96
2026-02-25 10:58:47,389 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 11:00:44,639 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 80.94.92.186
2026-02-25 11:00:53,252 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.208
2026-02-25 11:02:32,496 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 91.202.233.33
2026-02-25 11:05:29,190 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 161.132.37.26
2026-02-25 11:08:58,329 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 11:17:16,370 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.208
2026-02-25 11:19:12,436 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 45.148.10.157
2026-02-25 11:19:13,644 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 11:21:22,298 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 91.202.233.33
2026-02-25 11:22:49,538 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 206.189.77.61
2026-02-25 11:23:07,552 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.13
2026-02-25 11:29:29,538 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 11:33:32,679 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.122.208
2026-02-25 11:39:44,623 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 209.38.84.1
2026-02-25 11:41:06,857 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.121.25
2026-02-25 11:49:46,264 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 2.57.121.112
2026-02-25 11:53:29,557 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.13
2026-02-25 11:58:27,664 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 176.120.22.47
2026-02-25 11:58:50,879 fail2ban.actions        [830450]: NOTICE  [sshd] Ban 91.202.233.33
```

2.57.122.208 and 209.38.84.1 show that someone are being banned for short time but knows and tries again.

## Adjust

In /etc/fail2ban/jail.local

for sshd:
Increase bantime to 1d (was not set)
Set findtime to 1h (wasnot set)
Set maxretry to 3 (was not set)

for recidive:
Set maxretry to 5 (was not set)

```bash

[sshd]
enabled = true
port    = 22
bantime  = 1d
findtime = 1h
maxretry = 3

[recidive]
enabled  = true
logpath  = /var/log/fail2ban.log
action   = iptables-allports[name=recidive]
bantime  = 1w      
findtime = 1d      
maxretry = 5   

```

Restart service

```bash
sudo systemctl restart fail2ban
```

Check status after change (bantime and general)

```bash
sudo fail2ban-client get sshd bantime
86400

sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 3
|  |- Total failed:     9
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 2
   |- Total banned:     4
   `- Banned IP list:   176.120.22.13 176.120.22.47
```
Tip ban one individual IP adress this was
```bash
sudo fail2ban-client set sshd unbanip [IP-ADRESSE]
```

## Revisit day after

```bash
sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 1
|  |- Total failed:     377
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 53
   |- Total banned:     55
   `- Banned IP list:   176.120.22.13 176.120.22.47 193.32.162.13 91.224.92.78 195.178.110.15 2.57.122.208 193.24.211.202 91.202.233.33 206.189.147.34 45.148.10.147 164.92.136.119 45.148.10.151 213.209.159.159 45.148.10.152 46.225.126.58 2.57.121.25 45.148.10.141 45.87.249.170 45.148.10.157 170.64.136.185 213.209.159.158 2.57.121.112 172.183.91.3 45.148.10.121 217.100.210.76 80.94.92.171 92.222.229.15 91.224.92.54 91.224.92.190 91.224.92.108 152.42.187.233 46.101.147.180 50.6.228.52 106.12.45.125 64.227.181.122 45.148.10.192 147.182.194.60 92.118.39.72 20.61.126.211 188.166.227.168 170.64.213.76 92.118.39.76 170.64.183.63 163.177.76.18 157.245.46.52 34.84.162.177 156.227.233.148 45.148.10.240 92.118.39.56 160.250.133.177 103.124.107.26 2.57.122.238 115.190.112.50

sudo grep "Ban " /var/log/fail2ban.log
2026-02-26 00:21:13,678 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 188.166.227.168
2026-02-26 00:48:33,485 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 170.64.213.76
2026-02-26 01:31:20,379 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 92.118.39.76
2026-02-26 03:10:32,616 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 170.64.183.63
2026-02-26 03:13:36,685 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 163.177.76.18
2026-02-26 03:33:26,290 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 157.245.46.52
2026-02-26 03:38:27,598 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 34.84.162.177
2026-02-26 04:23:54,723 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 156.227.233.148
2026-02-26 05:00:27,464 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 45.148.10.240
2026-02-26 06:30:38,476 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 92.118.39.56
2026-02-26 07:21:00,704 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 160.250.133.177
2026-02-26 07:46:37,220 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 103.124.107.26
2026-02-26 09:14:16,193 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 2.57.122.238
2026-02-26 12:28:32,181 fail2ban.actions        [1469421]: NOTICE  [sshd] Ban 115.190.112.50

sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | head -n 10
    316 176.120.22.13
    270 2.57.121.25
    234 2.57.121.112
    171 213.209.159.159
    152 2.57.122.208
    107 185.156.73.233
     94 91.202.233.33
     86 2.57.122.96
     85 193.32.162.13
     84 129.212.185.118

sudo grep "Failed password" /var/log/auth.log
2026-02-26T08:17:30.182717+01:00 cantaloop sshd[1527387]: Failed password for root from 117.89.88.244 port 55672 ssh2
2026-02-26T08:45:54.537875+01:00 cantaloop sshd[1528958]: Failed password for root from 45.129.231.10 port 59330 ssh2
2026-02-26T08:52:38.520650+01:00 cantaloop sshd[1529297]: Failed password for root from 142.93.221.138 port 34074 ssh2
2026-02-26T08:56:38.706725+01:00 cantaloop sshd[1529501]: Failed password for invalid user cantaloop from 103.247.20.83 port 57366 ssh2
2026-02-26T09:10:51.307468+01:00 cantaloop sshd[1530282]: Failed password for invalid user sol from 2.57.122.238 port 47772 ssh2
2026-02-26T09:14:18.060868+01:00 cantaloop sshd[1530458]: Failed password for invalid user solana from 2.57.122.238 port 42544 ssh2
2026-02-26T09:23:14.634770+01:00 cantaloop sshd[1530921]: Failed password for root from 46.225.87.94 port 43824 ssh2
2026-02-26T09:44:20.822436+01:00 cantaloop sshd[1532030]: Failed password for root from 82.196.100.252 port 41466 ssh2
2026-02-26T09:47:37.255734+01:00 cantaloop sshd[1532208]: Failed password for invalid user admin from 45.129.231.10 port 50654 ssh2
2026-02-26T10:08:04.418439+01:00 cantaloop sshd[1533349]: Failed password for root from 142.93.221.138 port 40410 ssh2
2026-02-26T10:23:55.535154+01:00 cantaloop sshd[1534195]: Failed password for invalid user cantaloop from 193.104.58.61 port 57698 ssh2
2026-02-26T10:33:44.729839+01:00 cantaloop sshd[1534702]: Failed password for root from 220.95.232.134 port 41342 ssh2
2026-02-26T10:39:33.064674+01:00 cantaloop sshd[1535041]: Failed password for root from 117.89.88.244 port 33688 ssh2
2026-02-26T10:49:49.357273+01:00 cantaloop sshd[1535569]: Failed password for root from 45.129.231.10 port 60102 ssh2
2026-02-26T10:53:09.719128+01:00 cantaloop sshd[1535730]: Failed password for invalid user admin from 34.140.124.68 port 28434 ssh2
2026-02-26T11:05:53.576075+01:00 cantaloop sshd[1536401]: Failed password for root from 5.223.61.89 port 14574 ssh2
2026-02-26T11:39:05.566376+01:00 cantaloop sshd[1538505]: Failed password for root from 142.93.221.138 port 29906 ssh2
2026-02-26T11:51:10.754919+01:00 cantaloop sshd[1539120]: Failed password for invalid user cantaloop from 193.104.58.60 port 39550 ssh2
2026-02-26T11:52:44.283867+01:00 cantaloop sshd[1539200]: Failed password for invalid user admin from 45.129.231.10 port 38268 ssh2
2026-02-26T12:28:20.041131+01:00 cantaloop sshd[1541033]: Failed password for root from 115.190.112.50 port 60650 ssh2
2026-02-26T12:28:23.533135+01:00 cantaloop sshd[1541035]: Failed password for root from 115.190.112.50 port 33808 ssh2
2026-02-26T12:28:31.470610+01:00 cantaloop sshd[1541057]: Failed password for root from 115.190.112.50 port 34300 ssh2

```


