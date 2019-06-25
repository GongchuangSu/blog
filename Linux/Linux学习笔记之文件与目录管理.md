# 目录相关操作
## 几个特殊的目录
- `·`：代表此层目录     
- `..`：代表上一层目录
- `-`：代表前一个工作目录
- `~`：代表“目前用户身份”所在的主文件夹
- `~user1`：代表`user1`这个用户的主文件夹

## 几个常见的处理目录命令
- `cd`：切换目录
- `pwd`：(Print Working Directory)显示当前目录
- `mkdir`：新建一个新的目录
- `rmdir`：删除一个空的目录

```
[root@cg102 ~]# pwd [-P]
参数：
-P：显示出当前的实际路径，而非使用链接（link）路径

[root@cg102 ~]# mkdir [-mp] 目录名称
参数：
-m：配置目录的权限，无视默认权限（umask）
-p：直接将所需要的目录递归创建起来
```

## 几个常见的文件操作命令
- `cp`：复制文件或目录
- `rm`：删除文件或目录
- `mv`：移动文件与目录、或更名

```
[root@cg102 ~]# cp [-air] 源文件 目标文件
[root@cg102 ~]# cp [-air] 源文件1 源文件2 源文件3 目标目录
参数：
-a：连同文件属性一同复制，相当于-pdr
-i：覆盖既有文件之前先询问用户
-r：递归处理，将指定目录下的所有文件与子目录一并处理
-v：详细显示命令执行的操作
-p：保留源文件或目录的属性

[root@cg102 ~]# rm [-fir] 文件或目录
参数：
-f：强制删除文件或目录
-i：删除已有文件或目录之前先询问用户
-r：递归处理，将指定目录下的所有文件与子目录一并处理

[root@cg102 ~]# mv [-fiu] source destination
[root@cg102 ~]# mv [-fiu] source1 source2 source3 destination
参数：
-f：强制删除文件或目录
-i：删除已有文件或目录之前先询问用户
-u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作
```

## 获取文件名与目录名称
```
[root@cg102 sysconfig]# basename /etc/sysconfig/network
network
[root@cg102 sysconfig]# dirname /etc/sysconfig/network
/etc/sysconfig
```

## 几个常见的查阅文件内容的命令
- `cat`：由第一行开始显示文件内容
- `tac`：由最后一行开始显示文件内容
- `nl`：显示的时候顺便输出行号
- `more`：一页一页的显示文件内容
- `less`：与`more`类似，但比`more`更好用，可以往前翻页
- `head`：只看头几行
- `tail`：只看结尾几行
  - 常用参数`-f`文件内容更新后，显示信息同步更新
- `od`：以二进制的方式读取文件内容
- `wc`：统计文件内容信息
  - `-l`：只显示列数
  - `-w`：只显示字数

## 通配符

- 定义：`shell`内建的符号
- 用途：操作多个相似（有简单规律）的文件
- 常用通配符
  - `*`：匹配任何字符串
  - `?`：匹配1个字符串
  - `[xyz]`：匹配xyz任意一个字符
  - `[a-z]`：匹配一个范围
  - `[!xyz]`或`[^xyz]`：不匹配

## tar命令用法

**tar命令**可以为linux的文件和目录创建档案，常用于文件的压缩和解压。

```shell
## 打包
[root@localhost ~]# tar -cf /tmp/etc-backup.tar /etc
## 打包并使用gzip压缩
[root@localhost ~]# tar -zcf /tmp/etc-backup.tar.gz /etc
## 打包并使用bzip2压缩
[root@localhost ~]# tar -jcf /tmp/etc-backup.tar.bzip2 /etc

## 解包
[root@localhost ~]# tar -xf /tmp/etc-backup.tar -C /temp/
## 使用gzip解压
[root@localhost ~]# tar -zxf /tmp/etc-backup.tar -C /temp/
## 使用bzip2解压
[root@localhost ~]# tar -jxf /tmp/etc-backup.tar -C /temp/
```

参数说明：

- `c`：建立新的备份文件（打包）
- `x`：从备份文件中还原文件（解包）
- `f`：指定备份文件
- `t`：列出备份文件里的内容（查看）
- `z`：通过gzip指令处理备份文件
- `j`：通过bzip2指令处理备份文件

