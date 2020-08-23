title: Jupyter修改外观配置
author: 远方
date: 2020-04-22 11:19:41
tags:
---
jupyter默认的宽度比较小，当将浏览器全屏的时候，仅有一半的页面宽度能够得以利用，画图的时候展示的范围比较小。为了解决这个问题，只需要在设置主题时修改配置即可。
```bash
 jt -t grade3 -cellw 90% -T
```
- `jt`为主题配置命令
- `-t grade3` 为使用`grade3`主题
- `-cellw 90%` 为设置单元格宽度为页面宽度的90%
- `-T` 为显示工具栏。

效果如下图：

![upload successful](/images/pasted-3.png)

<font color='red'>备注：若使图片的宽度也变大，需要在设定TCanvas修改默认的宽度。</font>