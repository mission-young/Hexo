title: Hexo设置端口和ip
author: 远方
date: 2020-04-01 10:19:12
tags:
---
在bash中输入
```bash
hexo s -i -p 8000
```
`-i` 可以开放外部网路访问，若去除该选项，则只能本机`localhost`访问.
`-p 8000`可以用来设置端口，默认端口为4000.