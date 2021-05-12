title: macOS更改sshd服务器端口
author: 远方
tags:
  - sshd
  - 配置文件
categories:
  - 技术
  - ''
date: 2019-08-18 15:11:00
---
macOS ssh server默认端口为22，而学校网络政策一天三变，突然间无法在校外连通ssh。因而需要更改macOS默认端口。
macOS更改sshd服务端口的方式不同于linux更改`/etc/ssh/sshd_config`的方式。需要更改`/System/Library/LaunchDaemons/ssh.plist`文件。
其中
```xml
<key>SockServiceName</key>
<string>ssh</string>
```
ssh代表的就是默认的22端口，将ssh修改为合适的端口就可以了。
比如
```xml
<key>SockServiceName</key>
<string>67</string>
```
在设置界面重新关闭和开启ssh服务即可。