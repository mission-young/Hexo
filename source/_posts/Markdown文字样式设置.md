title: Markdown文字样式设置
author: 远方
date: 2019-08-04 11:51:40
tags:
---
## 修改markdown的字体、大小、颜色
```markdown
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=red>我是红色</font>
<font color=#008000>我是绿色</font>
<font color=Blue>我是蓝色</font>
<font size=5>我是尺寸</font>
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>
```
效果如下
- <font face="黑体">我是黑体字</font>
- <font face="微软雅黑">我是微软雅黑</font>
- <font face="STCAIYUN">我是华文彩云</font>
- <font color=red>我是红色</font>
- <font color=#008000>我是绿色</font>
- <font color=Blue>我是蓝色</font>
- <font size=5>我是尺寸</font>
- <font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>
## 为文字添加背景色
由于 style 标签和标签的 style 属性不被支持，所以这里只能是借助 table, tr, td 等表格标签的 bgcolor 属性来实现背景色。故这里对于文字背景色的设置，只是将那一整行看作一个表格，更改了那个格子的背景色（bgcolor）
```markdown
<table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>
```
<table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>
