---
title: "linux 命令行终端快捷键操作 / Linux teminal option"
date: 2019-09-13T02:20:08+08:00
keywords:
- linux
- 命令行终端快捷键
- 效率
description : "linux 终端命令行操作快捷键"
draft: false
tags: ["linux", "terminal", "hot key"]
categories: ["linux", "terminal", "快捷键"]
author: "atovk"
license: "CC BY-SA 4.0"
contentCopyright: '<a rel="license noopener" href="https://creativecommons.org/licenses/by-sa/4.0" target="_blank">CC BY-SA 4.0</a>'

---

## 基础快捷键

| 快捷键 | 功能 |
| --- | --- |
`Ctrl + a` | 跳到本行的行首
`Ctrl + e` | 则跳到页尾
`Ctrl + u` | 删除当前光标前面的文字
`ctrl + k` | 删除当前光标后面的文字
`Ctrl + w` </br> `Alt + d` | w删除光标前面的单词的字符,d则删除后面的字符
`Alt + Backsapce` | 删除当前光标后面的单词;误删使用`Ctrl + y`进行恢复, `Ctrl + L`进行清屏操作
`ctrl + a` | 光标移到行首
`ctrl + b` | 光标左移一个字母
`ctrl + c` | 杀死当前进程
`ctrl + d` | 退出当前 Shell
`ctrl + e` | 光标移到行尾
`ctrl + h` | 删除光标前一个字符，同 backspace 键相同
`ctrl + k` | 清除光标后至行尾的内容
`ctrl + l` | 清屏，相当于clear
`ctrl + r` | 搜索之前打过的命令.会有一个提示，根据你输入的关键字进行搜索bash的

## history 快捷键

| 快捷键 | 功能 |
| --- | --- |
`ctrl + u` |  清除光标前至行首间的所有内容
`ctrl + w` |  移除光标前的一个单词
`ctrl + t` |  交换光标位置前的两个字符
`ctrl + y` |  粘贴或者恢复上次的删除
`ctrl + d` |  删除光标所在字母; 注意: (`backspace`及`ctrl + h`)是删除光标前的字符.
`ctrl + f` |  光标右移
`ctrl + z`  |  将进程转到后台运行,`fg`命令恢复.比如top -d1 然后`ctrl + z`到后台`fg`重新恢复

## esc组合 快捷键

| 快捷键 | 功能 |
| --- | --- |
`esc + d` |  删除光标后的一个词
`esc + f` |  往右跳一个词
`esc + b` |  往左跳一个词
`esc + t` |  交换光标位置前的两个单词
