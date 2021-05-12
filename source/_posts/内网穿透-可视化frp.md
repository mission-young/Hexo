title: 内网穿透-可视化frp
author: 远方
date: 2020-09-14 14:57:57
tags:
---
## 软件工具
[Sakura Frp](https://www.natfrp.com/)(Frp二次开发，可视化)
### 流程
1. 注册账号并登陆
2. 每日领取流量

![upload successful](/images/pasted-14.png)

3. 软件下载

![upload successful](/images/pasted-15.png)

推荐使用自动安装脚本，按照提示填入id和秘钥

![upload successful](/images/pasted-16.png)

4. 添加和管理隧道

![upload successful](/images/pasted-17.png)
    
隧道协议分为tcp,udp,http,https,xtcp等，不同协议的区别参见网页右下方。
    
  ![upload successful](/images/pasted-19.png)
  对于我们常用的远程桌面(rdp)，ssh，均可用这种方式。
    	 
 ### 服务器选择
         
![upload successful](/images/pasted-20.png)	
可建站表示支持http隧道服务映射。？M表示带宽，优先使用国内的服务器，速度较快。
        
### TCP隧道设置
        
![upload successful](/images/pasted-21.png)
      
隧道名称和备注随意设置，本机地址留空即可，本地端口即为内网服务器的端口，比如ssh默认22，rdp默认3389，远程端口可以自己设置，符合规则即可，默认留空也可以。

下方的加密传输和压缩数据可打开。
        
### HTTP隧道设置
        
![upload successful](/images/pasted-22.png)
        和TCP协议对比，域名替代了远程端口。需要自己注册域名。绑定自己域名后，可以直接不带端口地访问自己域名。
        
![upload successful](/images/pasted-23.png)

* 备注：选择服务器时需选择 可建站的类型！

* 备注：建站也未必需要http隧道，使用tcp隧道也可以，但需要带端口访问。 [参考链接](https://gitproxy.qianqu.me/wiki/#/panel/faq?id=%e5%ae%9e%e5%90%8d%e8%ae%a4%e8%af%81%e5%88%b0%e5%ba%95%e5%8f%af%e4%bb%a5%e5%81%9a%e4%bb%80%e4%b9%88). 
          
### 配置文件：
使用脚本安装软件后，会提示创建系统服务，按照提示输入token和隧道之后，会自动创建systemd服务。根据自身需要，可以对配置文件进行修改。
       
服务脚本类似如下：

![upload successful](/images/pasted-24.png)

其中被遮挡的部分为个人秘钥，数字部分为隧道列表
    
![upload successful](/images/pasted-25.png)
    
可以看出，355729隧道正常启动，通过yfsonline.top即可访问.
    
对于同一个服务器的不同隧道端口，可以修改配置文件
    
![upload successful](/images/pasted-26.png)
    
对于不同的服务器，比如上面所列，则对于每一个服务器需要额外配置。目前所采用的方式为新建systemd服务(很蠢但有效)，如下：
    
![upload successful](/images/pasted-27.png)
    
随后通过`systemctl enable/restart *.service`的方式即可配置好服务。

## 可能遇到的部分很蠢的问题

1. 服务无法正常启动
检查服务配置文件的隧道ID，里面的id为初次配置时生成的，在反复测试的过程中可能这个隧道已经被你删除了。

2. 按教程配置好之后，依旧无法远程访问ssh服务
sshd端口是否输入正确？ sshd服务有没有开启？

3. 服务配置文件中使用'command1 && command2'开启两个服务后，无法正常访问服务
systemctl如何在一个service中开启两个进程，这个目前还没搞明白，就很蠢地手动创建另一个服务吧！
