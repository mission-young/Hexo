title: windows 批处理解压缩文件
author: 远方
date: 2020-08-06 21:47:17
tags:
---
今天在黑翼实习的过程中，廖师兄提到接下来数据处理要放到linux下的原因是数据解压的考虑。观察数据压缩的格式为7z后缀，而windows其实是有7z压缩软件的，相比能够通过命令行的形式来实现解压缩，从而结合脚本完成批量解压。

数据处理用的语言为python，最初的想法是调用python的逻辑来实现解压，后来觉得又没有太大的必要。因而考虑用bat脚本来实现。

7z的使用说明如下：
### 简介
7z，全称7-Zip， 是一款开源软件。是目前公认的压缩比例最大的压缩解压软件。

- 主页：http://www.7-zip.org/
- 中文主页：http://7z.sparanoid.com/
- 命令行版本下载：http://7z.sparanoid.com/download.html
- 主要特征：
	- 全新的LZMA算法加大了7z格式的压缩比
	- 支持格式：
		* 压缩 / 解压缩：7z, XZ, BZIP2, GZIP, TAR, ZIP
		* 仅解压缩：ARJ, CAB, CHM, CPIO, DEB, DMG, FAT, HFS, ISO, LZH, LZMA, MBR, MSI, NSIS, NTFS, RAR, RPM, UDF, VHD, WIM, XAR, Z

### 退出代码
- 0 ： 正常，没有错误；
- 1 ： 警告，没有致命的错误，例如某些文件正在被使用，没有被压缩；
- 2 ： 致命错误；
- 7 ： 命令行错误；
- 8 ： 没有足够的内存；
- 255 ： 用户停止了操作；

### 使用语法
7z <命令行> [<选项>...] <基本档案名称> [<参数变量>...]

- 在方括号内的表达式(“[” 和 “]”之间的字符)是可选的。
- 在书名号内的表达式(“<” 和 “>”之间的字符)是必须替换的表达式(而且要去掉括号)。

7-Zip 支持和 Windows 相类似的通配符：
- “*”可以使用星号代替零个或多个字符。 
- “?”可以用问号代替名称中的单个字符。

如果只用*，7-Zip 会将其视为任何扩展名的全部文件。

### 命令及实例

1.  a 添加文件到压缩档案。
	-	7z a archive1.zip subdir\ ：增加subdir文件夹下的所有的文件和子文件夹到archive1.zip中，archived1.zip中的文件名包含subdir\前缀。
	- 7z a archive2.zip .\subdir\* ：增加subdir文件夹下的所有的文件和子文件夹到archive1.zip中，archived2.zip中的文件名不包含subdir\前缀。
	- cd /D c:\dir1\ 
	- 7z a c:\archive3.zip dir2\dir3\ ：archiive3.zip中的文件名将包含dir2\dir3\前缀，但是不包含c:\dir1前缀。
	- 7z a Files.7z *.txt -r ： 增加当前文件夹及其子文件夹下的所有的txt文件到Files.7z中。

2.  b 测试 CPU 运行速度及检查内存错误。

3. d 从压缩档案删除文件。
	- 7z d archive.zip *.bak -r ：从archive.zip中删除所有的bak文件。

4.  e 从压缩档案中释放文件到当前目录中。或者到指定的输出文件夹。输出文件夹设置可以通过 -o (设置输出文件夹) 选项来更改。此命令会将所有被释放的文件放置到一个文件夹。如果您想使用完整路径释放文件，您必须使用 x (完整路径释放) 命令。 
	- 7z e archive.zip ：从压缩档案 archive.zip 中释放所有文件到当前文件夹。
	- 7z e archive.zip -oc:\soft *.cpp ：从压缩档案 archive.zip 中释放 *.cpp 文件到 c:\soft 文件夹。

5.  l 列出压缩档案内容。
	- 7z l archiv.zip ：列出压缩档案 archive.zip 的内容。

6. t 测试压缩档案文件的完整性。
	- 7z t archive.zip *.doc ：在压缩档案 archive.zip 中测试 *.doc 文件的完整性。

7. u 在压缩档案文件中使用较新的文件替换掉较旧的文件。
	- 7z u archive.zip *.doc ：在压缩档案 archive.zip 中更新 *.doc 文件。

8. x 在当前目录中，使用完整路径从压缩档案中释放文件.或者到指定的输出文件夹。
	-7z x archive.zip ：从压缩档案 archive.zip 中释放所有文件到当前文件夹。
	-7z x archive.zip -oc:\soft *.cpp ：从压缩档案 archive.zip 中释放 *.cpp 文件到 c:\soft 文件夹。

### 更多的选项

1.  --在命令行中使“--”后的选项开关“-”都失效。这样就允许在命令行中使用文件名以“-”开头的文件。
	- 7z t -- -ArchiveName.7z ：测试 -ArchiveName.7z 压缩档案.

2.  -i 指定压缩时附加文件或一类文件。此选项可附件添加多个类型。
	-  i[<recurse_type>]<file_ref> 其中<recurse_type>为可以为r[- | 0]（具体的-r选项见后面-r），<file_ref>可以为@{listfile} | !{wildcard}。
	- 7z a -tzip src.zip *.txt -ir!DIR1\*.cpp ：从当前目录中添加 *.txt 文件，和 DIR1 目录及其子目录中的 *.cpp 文件到 src.zip 压缩档案。

3. -x 指定某一文件或某一类文件从操作中排除。此选项可同时排除多个类型。
	- x[<recurse_type>]<file_ref> 其中<recurse_type>为可以为r[- | 0]（具体的-r选项见后面-r），<file_ref>可以为@{listfile} | !{wildcard}。
	- 7z a -tzip archive.zip *.txt -x!temp.* ：添加除 temp.* 文件之外的所有 *.txt 文件到压缩档案 archive.zip。

4. -o 指定释放文件的输出文件夹。此选项只能和释放命令配合使用。
	- 7z x archive.zip -oc:\Doc ：从 archive.zip 压缩档案释放所有文件到 c:\Doc 文件夹。

5. -r 递归子目录选项。
6. -r 开启递归子目录。对于 e (释放)、l (列表)、t (测试)、x (完整路径释放) 这些在压缩档案中操作的命令， 会默认使用此选项。 
7. -r- 关闭递归子目录。对于 a (添加)、d (删除)、u (更新) 等所有需扫描磁盘文件的命令，会默认使用此选项。 
8. -r0 开启递归子目录。但只应用于通配符。 
	- 7z l archive.zip -r- *.doc ：列出在 archive.zip 压缩档案中根目录下的 *.doc 文件。 
	- 7z a -tzip archive.zip -r src\*.cpp src\*.h ：将 src 目录及其子目录中的 *.cpp 及 *.h 文件添加到 archive.zip 压缩档案。

9. -t 指定压缩档案格式。指定压缩档案格式。它们可以是：zip、7z、rar、cab、gzip、bzip2、tar 或其它格式。而 默认值是 7z 格式。 
	-  7z a -tzip archive.zip *.txt ：使用 zip 格式从当前目录中添加所有 *.txt 文件到压缩档案 archive.zip。

10. -y 使 7-Zip 执行命令时的大多数提示失效。您可以使用此选项来阻止在 e (释放) 和 x (完整路径释放) 命令中文件覆盖时的提示。
	- 7z x src.zip -y ：从 src.zip 释放所有文件。所有的覆盖提示将被阻止且所有相同文件名的文件将被覆盖。

11. -v指定分卷大小。
	- {Size}[b | k | m | g] 
	- 指定分卷大小，可以使用字节、KB(1 KB＝1024 字节)，MB(1 MB = 1024 KB)或 GB(1 GB = 1024 MB)。如果您只指定了 {Size}，7-zip 将把它视为字。 
	-7z a a.7z *.txt -v10k -v15k -v2m ： 创建 a.7z 分卷压缩档案。第一个分卷为 10 KB，第二个为 15 KB，剩下全部为 2 MB。

12. -p 指定密码。
	- 7z x archive.zip -psecret ：将设有密码“secret”的压缩档案 archive.zip 中所有文件释放。

13. -ao 指定在释放期间如何覆盖硬盘上现有的同名文件。
	- 语法：-ao[a | s | u ]
	- -aoa 直接覆盖现有文件，而没有任何提示。 
	- -aos 跳过现有文件，其不会被覆盖。 
	- -aou 如果相同文件名的文件以存在，将自动重命名被释放的文件。举个例子，文件 file.txt 将被自动重命名为 file_1.txt。 
	- -aot 如果相同文件名的文件以存在，将自动重命名现有的文件。举个例子，文件 file.txt 将被自动重命名为 file_1.txt。 
	- 7z x test.zip -aoa ：从压缩档案 test.zip 中释放所有文件并却不做提示直接覆盖现有文件。

14. -an 不解析命令行中的 archive_name 区域。此选项必须和 -i (附加文件) 开关一起使用。比如您为压缩档案使用列表文件，您就需要指定 -ai 选项，所以您需要禁止解析命令行中的 archive_name 区域。
实例见后面的-ai和-ax中。

15. -ai 指定附加文件，包括压缩档案文件名及通配符。此选项可同时附加多个类型。
	- ai[<recurse_type>]<file_ref> 其中<recurse_type>为可以为r[- | 0]（具体的-r选项见后面-r），<file_ref>可以为@{listfile} | !{wildcard}。
	- 7z t -an -air!*.7z ： 在当前目录及子目录下测试 *.7z 压缩档案。

17. -ax 指定必须从操作中排除的压缩档案。此选项可同时排除多个类型。
	- ax[<recurse_type>]<file_ref> 其中<recurse_type>为可以为r[- | 0]（具体的-r选项见后面-r），<file_ref>可以为@{listfile} | !{wildcard}。
	- 7z t -an -ai!*.7z -ax!a*.7z ：测试除 a*.7z 之外的 *.7z 压缩档案。

更多的不常用的选项，可以查看帮助。例如：-m设置压缩算法；-scs 设置要压缩的文件的列表文件的字符集；-seml通过电子邮件发送压缩档；-sfx创建自释放档；-si从标准输入读入数据，-so从输出到标准输出；-slp设置大内存模式；-slt显示技术信息；-ssc设置区分大小写；-ssw压缩正在写入的文件；-u更新选项。

`7z -e` 后面跟的是相对路径，`7z -x`后面跟的是绝对路径。

下面是bat的脚本
```bat
@echo off
REM %cd% 终端当前目录
REM %%~dp0 为脚本所在目录
REM %%用在脚本中
REM %s用在命令行中
REM 7z x 后面跟绝对路径
REM 7z e 后面跟相对路径
REM 7z -aos 表示若文件已存在，跳过解压
REM 两行cd 源于 7z x -aos %%s -o%%~dps 在for循环中无法正常执行，用反复进退目录的方式曲线救国。这么做效率会下降，以后再修正。
REM /R 表示递归搜索

set work_path="%cd%"
echo work_path=%work_path%
for /R %work_path% %%s in (*.7z.001) do (
    cd %%~dps
    7z x -aos %%s
    cd %work_path%
)
```

- REM 表示bat脚本的注释
- `7z x -aos` 表示 解压到当前目录
- %%s表示与`*.7z.001`匹配的文件全路径

```log
%I 引用I
%~I         - expands %I removing any surrounding quotes (")
%~fI        - expands %I to a fully qualified path name
%~dI        - expands %I to a drive letter only
%~pI        - expands %I to a path only
%~nI        - expands %I to a file name only
%~xI        - expands %I to a file extension only
%~sI        - expanded path contains short names only
%~aI        - expands %I to file attributes of file
%~tI        - expands %I to date/time of file
%~zI        - expands %I to size of file

The modifiers can be combined to get compound results:
%~dpI       - expands %I to a drive letter and path only
%~nxI       - expands %I to a file name and extension only
%~fsI       - expands %I to a full path name with short names only
```
在上面，给出了cmd下提取路径或文件名的一些操作，如果用在脚本中，`%`改为`%%`即可。其中`I`表示变量，在这里，我们用的是`S`. 

