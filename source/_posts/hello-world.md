title: 应用Hexo搭建Github个人博客
tags:
  - Hexo
  - Blog
categories:
  - 技术
author: 远方
date: 2019-08-02 22:38:00
---
## 搭建Hexo环境
访问Hexo主页
<iframe src="https://hexo.io/zh-cn/" width="100%" Height="800">   </iframe>
<!--more-->
执行上述页面命令：
```bash
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```
即可完成hexo的安装。
### 安装插件：
<iframe src="https://jaredforsyth.com/hexo-admin/" width="100%" Height="800">   </iframe>
执行页面命令：
```bash
npm install --save hexo-admin
```
进入[设置界面](http://localhost:4000/admin/#/settings)：
![upload successful](/images/pasted-1.png)
勾选显示行号以及拼写检查。
随后在blog根目录添加脚本`hexo-publish.sh`：
```bash
#!/usr/bin/env sh
# hexo clean
hexo g
hexo d
```
随后执行`chmod +x hexo-publish.sh`，之后在`_config.yml`中添加
```yml
admin:
  deployCommand: './hexo-publish.sh'
```
修改`_config.yml`配置
```yml
deploy:
  type: git
  repository: git@github.com:mission-young/mission-young.github.io.git
  branch: master
```
至此，即可完成hexo的在线编辑及部署Github。
### 配置主题
本博客采用了[ARIA主题](https://github.com/AlynxZhou/hexo-theme-aria/blob/master/README.zh_CN.md)。
按照该作者Github配置完成主题配置选项。