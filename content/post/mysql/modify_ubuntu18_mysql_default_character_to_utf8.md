---
title: "ubuntu18.xx 中修改 mysql 数据库默认字符编码"
date: 2019-01-20T00:52:59+08:00
draft: false
tags: ["mysql", "springboot"]
categories: ["mysql", "springboot"]
author: "atovk"
---

# ubuntu18.xx 中修改 mysql 数据库默认字符编码
 - 今天更换meriadb 为mysql 之后发现 springboot jpa存入数据库中的字符是乱码，看了下问题为mysql字符编码不是 utf-8了

## ubuntu18 默认mysql配置文件地址
 sudo vim /etc/mysql/conf.d/mysql.cnf

## 编辑文件内容
 ```sh
[mysql]
default-character-set=utf8

[client]
default-character-set=utf8

[mysqld]
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8

 ```

## 重启mysql
sudo service mysql restart


## 注意
 - 如果已经存在的库中乱码，手动重设字符编码/删除重建;
