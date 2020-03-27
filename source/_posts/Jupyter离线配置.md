title: Jupyter离线配置
author: 远方
tags:
  - Jupyter
categories:
  - 科研
date: 2020-03-25 23:32:00
---
此文档不是用于离线安装jupyter，而是解决jupyter因网络问题无法正常显示图像的问题。

修改文件：`~ /.jupyter/jupyter_notebook_config.py`
定位到 `c.NotebookApp.extra_static_paths` 这一行，修改为 
`c.NotebookApp.extra_static_paths = ['/usr/share/root/js']`
其中`'/usr/share/root/js' `根据root安装的目录来确定.
![upload successful](/images/pasted-2.png)
可以通过`find`命令来确定该路径.