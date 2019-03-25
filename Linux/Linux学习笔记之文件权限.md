# Linux文件权限
## 查看文件权限和属性
```
[root@cg102 ~]# ls -al
总用量 2605064
dr-xr-x---.  8 root root      4096 1月  29 11:09 .
dr-xr-xr-x. 19 root root       252 1月  28 13:59 ..
-rw-------.  1 root root      1494 1月  28 12:56 anaconda-ks.cfg
-rw-------.  1 root root     13745 1月  29 17:40 .bash_history
-rw-r--r--.  1 root root        18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root       176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root       186 1月  28 13:54 .bashrc
drwx------.  3 root root        17 1月  28 13:57 .cache
-rw-r--r--.  1 root root      3954 1月  28 13:48 check.sh
-rw-r--r--.  1 root root       100 12月 29 2013 .cshrc
-rw-r--r--.  1 root root      6139 1月  28 13:48 dl.sh
```
> 说明：第一列代表文件的类型和权限。其中第一个字符代表文件类型；234代表文件拥有者的权限；567代表文件所属用户组的权限；890代表其他人对此文件的权限。

## 改变文件属性与权限
- chgrp：改变文件所属用户组
- chown：改变文件所有者
- chmod：改变文件权限

### chgrp
```
[root@cg102 ~]# chgrp [-R] grpname dirname/filename ...
```
> 说明：
> 1. -R：进行递归的持续更改，也即连同子目录下的所有文件、目录
> 2. 要改变的组名(grpname)必须要在`/etc/group`文件中存在才行，否则会报`invalid group name`错误

### chown
```
# 只改变所有者
[root@cg102 ~]# chown [-R] username dirname/filename ...
# 改变所有者和用户组
[root@cg102 ~]# chown [-R] username:grpname dirname/filename ...
[root@cg102 ~]# chown [-R] username.grpname dirname/filename ...
# 只改变用户组
[root@cg102 ~]# chown [-R] .grpname dirname/filename ...
```
> 说明：要改变的用户名(username)必须要在`/etc/passwd`文件中存在

### chmod
权限的设置方法有两种：数字类型改变文件权限和符号类型改变文件权限。

#### 数字类型改变文件权限
Linux文件的基本权限有九个，分别是owner、group、others三种身份各有自己的read、write、execute权限。可以使用数字代表各个权限：
- r:4
- w:2
- x:1

当权限为`-rwxrwx---`，分数则是：
- owner=`rwx`=4+2+1=7
- group=`rwx`=4+2+1=7
- others=`---`=0+0+0=0

```
[root@cg102 ~]# chmod [-R] xyz dirname/filename
```
> 说明：xyz就是三种身份分别对应的分数

#### 符号类型改变文件权限

| 命令  |       身份       |                操作                 |    权限     | 文件或目录 |
| :---: | :--------------: | :---------------------------------: | :---------: | :--------: |
| chmod | u<br>g<br>o<br>a | +（加入）<br>-（除去）<br>=（设置） | r<br>w<br>x | 文件或目录 |

> 说明：u代表user、g代表group、o代表others、a代表全部


```
[root@cg102 ~]# chmod u=rwx,go=rx .bashrc
# 注意：`u=rwx,go=rx`是连在一起的，中间不能存在任何空格
[root@cg102 ~]# chmod a-x .bashrc
```

## 目录与文件的权限意义

|        类别        | r                          | w                            | x                      |
| :----------------: | :------------------------- | :--------------------------- | :--------------------- |
| 权限对文件的重要性 | 可读取文件的实际内容       | 可以编辑文件**内容**         | 具有可被系统执行的权限 |
| 权限对目录的重要性 | 具有读取目录结构列表的权限 | 具有更改该目录结构列表的权限 | 是否有权限进入该目录   |

> 说明：
> 1. 当对一个**文件**具有`w`权限时，表示具有写入、编辑文件内容的权限，但并不具备删除该文件本身的权限
> 2. 当对一个**目录**具有`w`权限时，表示具有更改该目录结构列表的权限，即以下权限：
> - 新建新的文件与目录
> - 删除已经存在的文件与目录
> - 将已存在的文件或目录进行重命名