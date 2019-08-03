title: GitHub管理Hexo源文件
author: 远方
tags:
  - Blog
  - Hexo
categories:
  - 技术
date: 2019-08-03 10:36:00
---
hexo发布网站到Github之后，可以直接通过访问Git个人主页。但`hexo d`部署的方式并不包含博客的源码，对于博客的迁移、更新、维护、多终端编辑并不友好。

在网上看到诸多同时管理hexo博客源文件和发布版本的教程，看似很优雅，但分支的切换、管理比较繁琐，同时一个严重的问题是，源代码的权限想要设为私有，而发布版本为公有。因而决定重新建立一个repo来管理源代码项目。
<!--more-->
在Github新疆项目Hexo，权限设置为私有。在Hexo目录输入：
```
git init 
git add .
git commit -m "add"
git remote add origin git@github.com:mission-young/Hexo.git
git push -u origin master
```
查看Hexo目录下`.gitignore`文件:
```git
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```
已经自动忽略public目录。由此实现：源代码私有，部署至Hexo项目；发布网站共有，部署至mission-young.github.io。
同时编辑部署脚本`hexo-publish.sh`:
```bash
#!/usr/bin/env sh
# hexo clean
hexo g
hexo d
git add .
git commit -m "update"
git push  
```
从而实现一键同步Hexo项目并发布。