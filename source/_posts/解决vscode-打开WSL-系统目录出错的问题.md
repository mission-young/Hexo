title: 解决vscode 打开WSL 系统目录出错的问题
author: 远方
tags:
  - Magic
  - wsl
  - vscode
categories: []
date: 2020-03-26 07:17:00
---
更新系统至`windows v2004`版本之后，在非`Windows`系统目录(即非`/mnt`)打开`vscode`会提示错误，需要手动前往`\\wsl$\Arch\mnt\c...`之类的目录用`vscode`打开. 在`wsl`系统下输入`code \\wsl$\Arch\mnt\c...`也会同样报错。

解决此问题，只需要进入`Windows`系统目录，再执行`code //wsl$/Arch/home/yfs/...`即可。命令如下：
``` bash
alias code='result=$(pwd | grep "mnt");if [[ $result ]];then code .;else a=`pwd` && cd /mnt/c/ && code "//wsl$/Arch$a"&& cd $a;fi'
```

1. 先判断当前目录是否包含mnt,若包含，则为windows目录环境，直接code即可;
2. 若为wsl系统目录，先记录当前目录，随后进入windows目录，code打开映射的wsl路径，随后返回原目录。

Tips:
```
linux环境下,alias 的优先级高于$PATH.
```