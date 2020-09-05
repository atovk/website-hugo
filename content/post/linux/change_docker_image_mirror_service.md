---
title: "Change docker image mirror service"
date: 2020-07-24T23:29:45+08:00
keywords:
- echosence
- 技术分享
- 
description : "for china mainland docker user change mirror service config"
draft: false
tags: ["docker"]
categories: ["docker"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

> this change for china mainland docker users.

```sh

# create docker user config dir
sudo mkdir -p /etc/docker

# touch docker config file
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://1nj0zren.mirror.aliyuncs.com",
        "https://docker.mirrors.ustc.edu.cn",
        "http://f1361db2.m.daocloud.io",
        "https://registry.docker-cn.com"
    ]
}
EOF

# reload and restart docker
sudo systemctl daemon-reload
sudo systemctl restart docker

```
