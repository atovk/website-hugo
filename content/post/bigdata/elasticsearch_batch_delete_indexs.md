---
title: "批量删除ES中无效的index / Elasticsearch batch delete indexs"
date: 2019-09-28T14:43:11+08:00
keywords:
- echosence
- 技术分享
- 
description : "批量自动删除 elasticsearch 中大量无效index"
draft: false
tags: ["elasticsearch","delete","index","shell"]
categories: ["elasticsearch","delete","index","shell"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

> 通过elasticSearch 结合shell 批量删除 ES中大量无效index

## 正则获取所有无效index信息到备选删除记录文件中

```sh
# 通过 curl 获取 es 所有index信息，再通过 grep正则过滤出我们要删除的index，awk 只获取index名称 到 index.log 文件中
curl "${server}/_cat/indices" | grep 'real_time_sales_temporary_new_2019092408*' | awk '{print $3}' > index.log

```

## 完整自动脚本如下

> 循环我们过滤出得无效index列表，执行 delete 操作

```sh
#!/bin/bash
# atovk 20190924

# elasticsearch 服务地址
server="http://10.12.11.1:9346"

# 获取 正则查询到的 index real_time_sales_temporary_new_2019092408*
curl "${server}/_cat/indices" | grep 'real_time_sales_temporary_new_2019092408*' | awk '{print $3}' > index.log

# 循环删除相关无效index
cat index.log | while read line
do
    echo "${line}"
    curl -XDELETE "${server}/${line}"
done

```
