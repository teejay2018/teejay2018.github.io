---
layout: post
title: Project cantaloop - Software stack
hidden: true
date: 2025-11-24 08:00:00 +0200
categories: [linux,software]
tags: [linux,datacenter]
---

# 🌍 Project cantaloop - Software stack

*Documenting steps setting up my server.*

### Steps

> ✅ nginx.<br>
> ✅ mysql<br>
> ✅ python<br>
> ✅ certbot<br>


### Web server Nginx

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install nginx -y
```
Open firewall to web traffic<br>
```bash
sudo ufw allow 'Nginx Full'
```

### Database

```bash
sudo apt install mysql-server -y

sudo mysql_secure_installation # Run this to set a root password and secure the installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1

Skipping password set for root as authentication with auth_socket is used by default.
If you would like to use password authentication instead, this can be done with the "ALTER_USER" command.
See https://dev.mysql.com/doc/refman/8.0/en/alter-user.html#alter-user-password-management for more information.

By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
```

### Python

```bash
sudo apt install python3 python3-pip python3-venv -y
#including venv for project isolated environments
```
### Certbot

Use certbot to automatically handle SSL certificates<br>
Including free LetsEncrypt certificates and renewal via scheduled job every 3 month.<br>

First set DNS correct<br>
cantaloop.dk            A record       46.224.77.102<br>

Check with nslookup<br>

Install and set up certbot<br>

```bash
sudo certbot --nginx -d cantaloop.dk -d www.cantaloop.dk -d private.cantaloop.dk
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Requesting a certificate for cantaloop.dk and 2 more domains

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/cantaloop.dk/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/cantaloop.dk/privkey.pem
This certificate expires on 2026-02-19.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for cantaloop.dk to /etc/nginx/sites-enabled/default
Successfully deployed certificate for www.cantaloop.dk to /etc/nginx/sites-enabled/default
Successfully deployed certificate for private.cantaloop.dk to /etc/nginx/sites-enabled/default
Congratulations! You have successfully enabled HTTPS on https://cantaloop.dk, https://www.cantaloop.dk, and https://private.cantaloop.dk

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

```

### 💡Still to do

> 🔭 test renew dry-run via certbot<br>
sudo certbot renew --dry-run<br>
---