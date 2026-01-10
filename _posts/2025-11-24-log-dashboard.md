---
layout: post
title: Project cantaloop - Dashboard
hidden: true
date: 2025-11-24 08:00:00 +0200
categories: [linux,software]
tags: [linux,datacenter]
---

# 🌍 Project cantaloop - Dashboard for logging web traffic

*After core setup we now add a Dashboard to visualize web traffic. This will be based on Elastic stack, previously called ELK stack. It will be using Filebeats, Elasticserach and Kibana. Reason for all this is to demonstrate a useful application build with best practice modern open source tools*

### Steps

> ✅ port 5601 setup - allow on select IP.<br>
> ✅ use official repo<br>
> ✅ elasticsearch<br>
> ✅ kibana<br>


### Allow port 5600

```bash
sudo ufw allow from YOUR_HOME_IP_ADDRESS to any port 5601 proto tcp
sudo ufw status
```

### Use official repo

```bash

```

### Install Elasticsearch

```bash

```
### Install Kibana



### 💡Still to do

> 🔭 next Filebeats<br>
<br>

> Prototype finished - Elastic, Kibana, Filebeat installed and configured and first sample Dashboard is running.<br>

---