title: Linux 删除某个名字的进程
author: 远方
tags:
  - Linux
categories:
  - 技术
date: 2019-09-19 10:00:00
---
linux kill 掉所有匹配到名字的进程
如，要 kill 掉 swoole 相关的进程

ps aux | grep swoole |  awk '{print $2}' | xargs kill -9

ps 列出所有进程，

参数：

a - 显示现行终端机下的所有进程，包括其他用户的进程；

u - 以用户为主的进程状态 ；

x - 通常与 a 这个参数一起使用，可列出较完整信息。

grep 过滤掉不包含 "swoole" 的行

awk '{print $2}'    获取进程 ID (PID， Process Identification)，我们想 kill 掉某一个进程的时候需要通过 PID 指定特定进程

xargs  将标准输入数据转换成命令行参数，xargs能够处理管道或者stdin并将其转换成特定命令的命令参数。

也就是将管道传递过来的每一个 PID 作为 kill -9 的参数

