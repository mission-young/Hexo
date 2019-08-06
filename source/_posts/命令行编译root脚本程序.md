title: 命令行编译root脚本程序
author: 远方
tags:
  - root
  - g++
  - 编译
categories:
  - 技术
  - 科研
date: 2019-08-06 09:14:00
---
root可以直接运行cpp脚本，或者解释运行，或者编译运行。但都需要借助root编译器。这样一是速度比较慢，二是当需要执行大量任务时，难以实现并行。

因而产生了将脚本进行完整编译，使之能够脱离root单独运行。较大的项目用Makefile管理比较方便，对于较小的项目，使用单文件即可完成。
```bash
g++ -o source source.cc `root-config --cflags --libs`
```
其中`root-config --cflags --libs` 用来生成链接命令
```bash
root-config --cflags --libs
```
结果为
```bash
-pthread -std=c++11 -m64 -I$ROOTSYS/include -L$ROOTSYS/lib -lCore -lImt -lRIO -lNet -lHist -lGraf -lGraf3d -lGpad -lTree -lTreePlayer -lRint -lPostscript -lMatrix -lPhysics -lMathCore -lThread -lMultiProc -pthread -lm -ldl -rdynamic
```