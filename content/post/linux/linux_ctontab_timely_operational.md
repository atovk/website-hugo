---
title: "linux下crontab定时在mysql中生成明日需要使用的空表 / Timely operational on linux crontab"
date: 2019-09-27T23:11:55+08:00
keywords:
- echosence
- 技术分享
- 
description : "使用crontab定时操作 某些任务: mysql定时新建空表"
draft: false
tags: ["linux","crontab","mysql"]
categories: ["linux","crontab","mysql"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

> 通过定时crontab 调用远程执行sql新建表结构

## create_table.sh

```shell

#!/bin/bash

# 生成明天的日期
tomorrow=$(date -d "+1 days" "+%Y%m%d")
echo next date: ${tomorrow}

# sql文件脚本中的日期替换为明日的日期
sed -i "s/\([0-9]\{8\}\)/${tomorrow}/g" /home/crontab/create_table.sql

echo `cat /home/crontab/create_table.sql | head -n 1`

# 通过执行mysql执行文件中的sql 脚本创建以命题啊日期结尾的表名
mysql -uroot -p123456 -h10.0.56.20 -Dpangu</home/wangjun/crontab/create_table.sql

```

## 加入到 crontab 中每日生成下一日的空表

> 别忘了给脚本加执行权限

```sh

# 每天一点执行
0 1 * * * sh /home/wangjun/crontab/create_table.sh

# 创建 crontab 任务
crontab /home/wangjun/crontab/crontest.cron

# 查看crontab 任务
crontab -l

```