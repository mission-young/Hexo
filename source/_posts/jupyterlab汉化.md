title: jupyterlab汉化
author: 远方
date: 2021-03-08 18:06:33
tags:
---
mac安装jupyterlab，使用`pip3 install jupyterlab`实际上安装的还是jupyter，并不起作用，仍然需要一系列操作来完成。可以使用`brew install jupyterlab`的方式完成安装。

但此时`jupyterlab`和`python3`是什么关系呢？

实际上，`jupyterlab`依旧是引用的`python3`,而非独立安装了一份python，且两者的`pip`库通用。`jupyterlab`的库的位置在`/usr/local/Cellar/jupyterlab/3.0.9/libexec/lib/python3.9/site-packages`，而`python3`的库位置在`/usr/local/lib/python3.9/site-packages`。对于在`jupyterlab`网页界面安装的插件，会同步安装，但是通过系统`pip3`安装的，则只会安装到`python3`所在的库位置，并不会同步，且不会作用于`jupyterlab`。

比如这里我们所需要的汉化插件，`jupyterlab-language-pack-zh-CN`，若只采用默认安装的方式，并不能汉化成功，而同步安装在`jupyterlab`库的位置之后，汉化正常。