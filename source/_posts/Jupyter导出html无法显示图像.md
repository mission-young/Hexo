title: Jupyter导出html无法显示图像
author: 远方
date: 2020-04-08 13:41:20
tags:
---
此问题是由于root版本变更引起的。

在`root v620`[更新日志](https://root.cern/doc/v620/release-notes.html#release-6.2004)中可以看到关于网络部分做了诸多变更。


# Core Libraries
- Speed-up startup, in particular in case of no or poor network accesibility, by avoiding a network access that was used as input to generate a globally unique ID for the current process.
- This network access is replaced by a passive scan of the network interface. This reduces somewhat the uniqueness of the unique ID as the IP address is no longer guaranteed by the DNS server to be unique. Note that this was already the case when the network access (used to look up the hostname and its IP address) failed.
# Language Bindings
## Jupyter Notebook Integration
- When starting Jupyter server with `root --notebook` arg1 arg2 ..., extra arguments can be provided. All these arguments delivered as is to jupyter executable and can be used for configuration. Like server binding to specific host `root --notebook --ip=hostname`
- Remove `c.NotebookApp.ip = '*' from default jupyter config`. One has to provide ip address for server binding using `root --notebook --ip=<hostaddr>` arguments
- Now Jupyter Notebooks will use JSROOT provided with ROOT installation. This allows to use notebooks without internet connection (offline).
# JavaScript ROOT
- Provide monitoring capabilities for TGeoManager object. Now geomtry with some tracks can be displayed and updated in web browser, using THttpServer monitoring capability like histogram objects.
- JSROOT graphics are now supported in the JupyterLab interface. They are activated in the same way as in the classic Jupyter, i.e. by typing at the beginning of a notebook cell:
```bash
%jsroon on
```
从上述变更中可以解释最近版本为什么突然jupyter无法正常显示图像了。
root官方为了解决弱网络连接下jupyter显示问题，把jupyter的js运行库放在了新版本root安装包中，运行jupyter时会自动调用这个js库。 但是需要留意的是 <font color='red'>仅使用root --notebook这种方式启动jupyter才会生效！使用jupyter-notebook的方式启动并不会生效！！图依旧无法正常显示。</font> 而前面的博客[Jupyter离线配置](/2020/03/25/Jupyter离线配置/)中已经提过，为了解决这个问题，在jupyter的配置文件中可以新增`static`路径来解决，解决了在jupyter运行时的绘图问题。

但这个问题也带来了其他的问题。其一是，无法自由地分享`.ipynb`和对应导出的`.html`。分享的文件在nbviewer中无法显示图像。猜测是root更新之后把原来的链接进行了替换。

为了做验证，新建了一个测试linux环境，编译安装了`root v61902`，发现问题和`v620`一致，下载`v61802`并安装jupyter环境，无需任何配置，输入`jupyter-notebook`即可正常显示图像和导出有图的html文件，`nbviewer`也可正常显示上传到github的`.ipynb`文件。显然，这个版本和最初用的版本一致，在弱网条件下无法正常显示。

把`v620`和`v618`导出的`html`文件做对比，我们把`v620`和`v618`分别定义为`offline`和`online`。两者对应的测试文件为[`
offline.html
`](/attachments/offline.html)和[`online.html`](/attachments/online.html).