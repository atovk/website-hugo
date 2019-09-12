---
title: "MAC 无法打开应用程序 / Fix mac application can‘t open"
date: 2019-09-13T01:42:46+08:00
draft: false
tags: ["MacOS","error"]
categories: ["MacOS"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

## Mac OS-[xxx.app已损坏,打不开.你应该将它移到废纸篓]

> 出现这个问题的解决方法：

- 修改系统配置：

```text
    系统偏好设置... -> 安全性与隐私 -> 修改为任何来源
```

- 如果没有这个选项的话（macOS Sierra 10.12）,打开终端，执行:

```sh

    # 此命令为 关闭苹果的Gatekeeper, 其实是有被攻击的风险的
    # 参考维基百科 https://en.wikipedia.org/wiki/Gatekeeper_(macOS)

    sudo spctl --master-disable
```
