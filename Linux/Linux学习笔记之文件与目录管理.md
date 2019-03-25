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
- `od`：以二进制的方式读取文件内容