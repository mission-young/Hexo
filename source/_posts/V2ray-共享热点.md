title: V2ray 共享热点
author: 远方
date: 2020-05-02 21:19:16
tags:
---

最近肺炎爆发, 虽然上班了但是因为公司要节省口罩, 所以大部分时间都在家里待岗, 正好电脑上已经不再用ss, ssr也准备不用了 (越来越不稳定了) 于是尝试了一下共享V2Ray的局域网连接, 很快就成功了, 闲得无事写篇博客记录一下.
准备
1.	电脑1台
2.	安装好V2RayN
详细步骤
设置V2RayN
因为现在懒得自己弄, 所以随便找个机场, 拿到订阅链接后在V2RayN中更新节点并在设置中允许局域网连接, 同时留意一下本地监听端口, 如图显示是10808端口.

![upload successful](/images/pasted-4.png)
打开移动热点
以win10系统为例, 在设置中打开移动热点:
 
![upload successful](/images/pasted-5.png)
然后在网络适配器选项中, 查看已经开启的热点并获取ip地址:
 
![upload successful](/images/pasted-6.png)
连接热点
首先在设置中查看已开启的代理的手动设置项, 可以看到端口为10809.
 
![upload successful](/images/pasted-7.png)
在手机上连接热点, 并手动设置代理服务器ip以及端口, 从上面步骤可知分别为: 192.168.137.1和10809.
 
![upload successful](/images/pasted-8.png)
到这里就大功告成了, 其他类似的共享到局域网的热点都是差不多的步骤, ssr的设置也一样.
