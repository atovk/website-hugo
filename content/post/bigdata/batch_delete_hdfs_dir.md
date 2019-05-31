---
title: "批量删除HDFS上无效的临时文件/ Batch delete hdfs dir"
date: 2019-05-31T23:46:56+08:00
draft: false
tags: ["bigdata", "HDFS", "linux", "shell"]
categories: ["bigdata", "HDFS", "linux", "shell"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

# 批量删除HDFS上无效的临时文件
    需求：当前日期下的临时目录不删除（可能存在正在运行的临时文件），通过日期过滤dir将非当前日期的dir进行删除

## 第一版
    由于hdfs删除效率太低，删除一个文件需要 1-2S 完全无法忍受 PB的数据等待删除

```shell

for line in $(hdfs dfs -ls /user/wangping/_sqoop/ |grep -v `date +%Y-%m-%d`|awk -F ' /' '{print $2}')
do
    dir=/${line}
    echo "drop sqoop hdfs tmp dir: ${dir}"
    `hdfs dfs -rm -r ${dir}`
done

```

## 第二版
    中间想一次性加载所有目录一次删除，发现我发遍历初所有待删除目录（太鸡儿多了）
    通过批量执行删除脚本，一千为批次的进行删除执行时间依旧一次删除时间为1-3S左右，效率提高不是一个数量级

```shell

dirs=""
size=0
for line in $(hdfs dfs -ls /user/wangping/_sqoop/ |grep -v `date +%Y-%m-%d`|awk -F ' /' '{print $2}')
do
    let "size++"
    dir=/${line}
    echo "drop sqoop hdfs tmp dir: ${size} ${dir}"
    dirs=${dirs}" "${dir}
    #echo ${size}
    if [[ `expr ${size} % 1000` == 0 ]]; then
        echo "dirs:" ${dirs}
        hdfs dfs -rm -r ${dirs}
        dirs=""
    fi
#    `hdfs dfs -rm -r ${dir}`
done

echo ${dirs}
hdfs dfs -rm -r ${dirs}

```

## 到此 批量删除的工作告一段落
涨知识：
    1，shell 中的计算变量
    2，shell 中的自增变量