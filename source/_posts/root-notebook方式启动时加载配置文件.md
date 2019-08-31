title: root --notebook方式启动时加载配置文件
author: 远方
tags:
  - jupyter
  - 配置
categories:
  - 技术
date: 2019-08-31 14:48:00
---
jupyter-notebook方式启动notebook时，会调用`~/.jupyter/jupyter_notebook_config.py`. 但通过`root --notebook`启动notebook时，则不会自动找到该路径。需要把配置文件拷到当前目录，随后执行`root --notebook`命令。