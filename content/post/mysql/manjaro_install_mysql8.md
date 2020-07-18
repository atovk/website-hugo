---
title: "In manjaro linux to install mysql 8.*"
date: 2020-07-18T14:27:27+08:00
keywords:
- echosence
- 技术分享
- 
description : "eazy install mysql from manjaro"
draft: false
tags: ["manjaro", "mysql"]
categories: ["manjaro", "mysql"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

> in manjaro linux ,install mysql is so eazy!! follow down step by step.

```sh

# arch linux auto install mysql
sudo pacman -S mysql

# random password
sudo mysqld --initialize --user=mysql --basedir=/usr --datadir=/var/lib/mysql

# empty password
sudo mysqld --initialize-insecure --user=mysql --basedir=/usr --datadir=/var/lib/mysql

# auto start service
sudo systemctl enable mysqld.service
sudo systemctl daemon-reload
sudo systemctl start mysqld.service


# login local mysql server
mysql -u root -p

```

## error 

```sh

ignore...

```
