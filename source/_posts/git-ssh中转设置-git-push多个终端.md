title: git ssh中转设置 / git push多个终端
author: 远方
tags:
  - ssh
  - git
  - 配置
categories:
  - 技术
date: 2019-09-13 16:08:00
---
实验室服务器尽管已经设置代理上网，但是访问git的时候，只能通过http clone项目，因而不满于实际开发需求。因而需要换种方式实现。
上网搜索，发现大多数方案要求借助于代理服务。在非管理员条件下想要实现代理服务并不容易，而且系统已经内置了代理，两者也容易发生冲突。
借助先前git内网服务搭建的经验，采用中转的方式来实现。
### 在server端初始化git仓库
- project: testcode
- path: /data/d2/RIBLL2017NaMgAlSi/gitbackup
- code: `git init --bare testcode`
	-  这一步会自动在gitbackup目录下创建testcode目录
- edit: 在testcode目录下创建`.gitignore`文件
``` bash
# Prerequisites
*.d
# Compiled Object files
*.slo
*.lo
*.o
*.obj
# Precompiled Headers
*.gch
*.pch
# Compiled Dynamic libraries
*.so
*.dylib
*.dll
# Fortran module files
*.mod
*.smod
# Compiled Static libraries
*.lai
*.la
*.a
*.lib
# Executables
*.exe
*.out
*.app
# root file
*.root
```
### 在server端自己的工作目录下clone testcode项目

- code: `git clone /data/d2/RIBLL2017NaMgAlSi/gitbackup/testcode`
- 随后即可进入testcode工作目录创建自己的项目文件。正常add，commit和push
### 在个人电脑端clone testcode项目
- code: `git clone ssh://wuchenguang@*:2727//data/d2/RIBLL2017NaMgAlSi/gitbackup/testcode`
### 在个人电脑端配置
- push到github(执行一次) `git remote set-url --add --push origin git@github.com:mission-young/testcode.git`
- push到server(执行一次) `git remote set-url --add --push origin ssh://wuchenguang@*:2727//data/d2/RIBLL2017NaMgAlSi/gitbackup/testcode`
- 同步push(需要时执行) `git push`

### .bashrc 设置备份
- server
```bash
# git --init bare project
export gitserver=/data/d2/RIBLL2017NaMgAlSi/gitbackup
# eg. git clone $gitpath/code 用于clone server项目
```
- pc
```bash
export gitserver=/data/d2/RIBLL2017NaMgAlSi/gitbackup
export gitpath="ssh://wuchenguang@*:2727/$gitserver"
# eg. git clone $gitpath/code 用于clone server项目
export pushgithub="remote set-url --add --push origin git@github.com:mission-young"
# eg. git $pushgithub/code 用于设置push到github
export pushserver="remote set-url --add --push origin ssh://wuchenguang@*:2727/$gitserver"
# eg. git $pushserver/code 用于设置push到server
# 搞定上面几步之后，git push 可以同时上传到github与server
```



