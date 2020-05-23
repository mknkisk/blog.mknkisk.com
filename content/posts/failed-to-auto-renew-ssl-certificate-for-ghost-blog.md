---
title: '[SOLVED] Failed to auto renew ssl certificate for ghost blog'
date: 2017-11-27T01:06:36+09:00
draft: false
---

ghost cli v1.0.0 -> 1.3.0

Ref -> [Upgrading Ghost-CLI](https://docs.ghost.org/v1/docs/upgrade#section-upgrading-from-1-1-3)

```
$ npm upgrade ghost-cli -g

$ ghost version
Ghost-CLI version: 1.3.0
Ghost Version (at /var/www/ghost): 1.6.2

$ ghost migrate
✔ Checking for available migrations
Running sudo command: mkdir -p /etc/letsencrypt
Running sudo command: ./acme.sh --install --home /etc/letsencrypt
Running sudo command: /etc/letsencrypt/acme.sh --issue --home /etc/letsencrypt --domain blog.mknkisk.com --webroot /var/www/ghost/system/nginx-root --reloadcmd "nginx -s reload" --accountemail xxx@example.com
Running sudo command: nginx -s reload
Running sudo command: /root/.acme.sh/acme.sh --remove --home /root/.acme.sh --domain blog.mknkisk.com
✔ Migrating SSL certs

$ crontab -l
11 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null
```
