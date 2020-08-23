title: Hexo忽略渲染某些文件
author: 远方
date: 2020-04-10 10:07:10
tags:
---
hexo运行时会渲染source目录下所有文件，如果需要跳过某些文件，需要在hexo根目录下的`_config.yml`文件中修改配置：
```yml
skip_render: 
```
为
```yml
skip_render: "attachments/*"
```
如果需要跳过某些文件（如html），可以更改为
```yml
skip_render: "attachments/*.html"
```
这里提一下渲染`html`与否的区别：
1. 渲染会增加hexo启动的时常
2. 渲染后html会内嵌入hexo框架中，成为其子界面
3. 非渲染的html会跳转到html文件，和原来的html界面一样。