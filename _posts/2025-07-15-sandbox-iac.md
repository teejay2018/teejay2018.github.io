---
layout: post
title: Setup for sandbox vm
date: 2025-07-15 08:00:00 +0200
categories: [linux,iac,portable]
tags: [terraform,ansible,azure]
---

# Setup for sandbox vm

Repository on GitHub [my-sandbox-iac](https://github.com/teejay2018/my-sandbox-iac)

Local development from WSL Ubuntu 24.4 on Windows Laptop (/home/tom/my-sandbox-iac)

### Install Terraform on Ubuntu

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

#terraform version
#Terraform v1.12.2
#on linux_amd64
```
### Install Ansible on Ubuntu

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

#ansible --version
#ansible [core 2.18.6]
```

### Setup SSH

To use SSH for Ansible we need to generate keys with ssh-keygen on Ubuntu and the use the public key to configure SSH access on the VM.
Enter passphrase or leave it blank if you want passwordless SSH

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible_key

ls -al ~/.ssh
total 16
drwx------  2 tom tom 4096 Jul 15 15:37 .
drwxr-x--- 18 tom tom 4096 Jul 15 15:37 ..
-rw-------  1 tom tom 3389 Jul 15 15:37 ansible_key
-rw-r--r--  1 tom tom  745 Jul 15 15:37 ansible_key.pub
```

This generates two files ansible_key (private key) and ansible_key.pub (public key)
Go to Azure linux VM and under Settings -> Networking -> SSH keys, select Upload SSH key and choose the public key (ansible_key.pub)


