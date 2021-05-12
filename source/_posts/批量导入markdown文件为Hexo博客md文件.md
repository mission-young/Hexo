title: 批量导入markdown文件为Hexo博客md文件
author: 远方
date: 2020-04-11 23:26:56
tags:
---
相比于普通的markdown文件，hexo的markdown文件有文件头数据，用来设定标题，创建日期，类别等信息，有时候需要批量将普通markdown文件添加头部文件数据，可以通过以下脚本实现。
```bash
for file in */*
do
    name=`echo ${file%.*}`
    sed -i "1i\title: `echo ${name##*/}`" $file
    sed -i "2i\author: 远方" $file
    sed -i "3i\tags:" $file
    sed -i "4i\  - LeetCode" $file
    sed -i "5i\  - 算法" $file
    sed -i "6i\categories:" $file
    sed -i "7i\  - LeetCode破局攻略" $file
    sed -i "8i\date: 2016-01-01 19:20:00" $file
    sed -i "9i\---" $file
done
```
其中
```bash 
name=`echo ${file%.*}`
```
截取`.*`前的字符串，表现为去后缀名，
```bash
sed -i "1i\title: `echo ${name##*/}`" $file
```
从左往右截取`*/`后的数据，`##`标识截取到最后一个满足条件的，表现为去除中间路径，保留文件名。

- [参考链接](https://www.cnblogs.com/kiko2014551511/p/11531558.html)