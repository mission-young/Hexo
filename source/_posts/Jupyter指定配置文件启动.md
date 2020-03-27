title: Jupyter指定配置文件启动
author: 远方
tags: []
categories: []
date: 2020-03-27 13:32:00
---
`jupyter`默认配置文件为`~/.jupyter/jupyter_notebook_config.py`
若有多种配置需求如端口，启动路径，密码，是否允许远程访问，则可以将`jupyter_notebook_config.py`复制一份进行修改，随后重命名或者放置到其他目录。为了方便管理，可以修改名字，如`jupyter_notebook_config_remote.py`，启动jupyter命令为
`jupyter-notebook --config='~/.jupyter/jupyter_notebook_config_remote.py'`. 而默认的配置依旧可以使用`jupyter-notebook`启动.
