---
title: "Docker rootless"
date: 2020-07-18T23:56:24+08:00
keywords:
- echosence
- 技术分享
- 
description : "rootless docker"
draft: false
tags: ["docker", "rootless"]
categories: ["docker"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---
[docker office rootless doc](https://docs.docker.com/engine/security/rootless/)

[rootless principle](https://medium.com/@tonistiigi/experimenting-with-rootless-docker-416c9ad8c0d6)


```sh

# using office rootless shell script
curl -sSL https://get.docker.com/rootless | sh

# generally will be notice create `/etc/subuid` & `/etc/subgid`

sudo touch /etc/subuid
sudo touch /etc/subgid


# edit touch file
echo "`whoami`:`id -u`:65536" >> /etc/subuid
echo "`whoami`:`id -u`:65536" >> /etc/subgid

# reusing office rootless shell script
curl -sSL https://get.docker.com/rootless | sh

# --user opt service
systemctl --user (start|stop|restart) docker

# success to rootless docker
docker ps

```