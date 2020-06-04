## 修改PATH环境变量

### 临时修改，关闭连接失效

```shell
export PATH=/usr/local/bin:$PATH
```

> 有效期限：临时改变，只能在当前的终端窗口中有效，当前窗口关闭后就会恢复原有的path配置
> 用户局限：仅对当前用户

### 永久修改当前用户

```shell
vim ~/.bashrc 
// 在最后一行添上：
export PATH=/usr/local/bin:$PATH
// 关闭保存，执行以下命令生效
source ~/.bashrc
```

> 有效期限：永久有效
> 用户局限：仅对当前用户

### 全局修改

```shell
vim /etc/profile
//在最后一行添上：
export PATH=/usr/local/bin:$PATH
// 关闭保存，执行以下命令生效
source ~/.bashrc
```

> 有效期限：永久有效
> 用户局限：对所有用户

## 设置静态IP

> 网络配置的配置文件在/etc/sysconfig/network-scripts/下，文件名前缀为ifcfg-后面跟的就是网卡的名称

- 先查看配置文件是否与网卡名一致

```shell
[root@localhost ~]# ls /etc/sysconfig/network-scripts/ifcfg-*
/etc/sysconfig/network-scripts/ifcfg-enp0s3
/etc/sysconfig/network-scripts/ifcfg-lo
```

![image-20200229181013343](assets/image-20200229181013343.png)

- 若上述名称不一致，需调整配置文件名

```shell
[root@localhost network-scripts]# mv ifcfg-enp0s3 ifcfg-enp0s8
```

- 修改配置文件

```shell
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static # 改为 static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8 # 改为实际网卡名称
UUID=b8adac9e-996d-4b3c-bd45-9176f4ac05d7
DEVICE=enp0s8 # 改为实际网卡名称
ONBOOT=yes    # 改为自启动
IPADDR=192.168.1.110  # 设置静态IP地址
NETMASK=255.255.255.0 # 子网掩码
GATEWAY=192.168.1.1   # 网关或路由地址
```

- 重启服务

```shell
service network restart
```

## 修改主机名

```shell
[root@fastdfs-200 ~]# hostnamectl set-hostname fastDFS-200
# 需重启
[root@fastdfs-200 ~]# hostnamectl status
   Static hostname: fastdfs-200
   Pretty hostname: fastDFS-200
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 42cbcdd91ee544bf8a9a61ecbc1f5f61
           Boot ID: e606de4a3148428cbcfa3a6bf7749d74
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-862.el7.x86_64
      Architecture: x86-64
```

##查看目录的树形结构

```shell
# 安装tree
yum intall tree
# 以树状图显示所有文件，子文件夹显示到第 N 层
tree -L N
```

## SSH无密码登录

比如服务器A与服务器B之间ssh需要无密码登陆，可按照以下步骤进行配置：

- 在服务器A和B上生成各自的SSH密钥和公钥

  ```ssh
  # 一路Enter即可
  # 生成的密钥和公钥保存在~/.ssh目录下
  ssh-keygen -t rsa
  ```

- 将各自SSH公钥上传至对方服务器上

  ```ssh
  # 在服务器A上执行
  ssh-copy-id root@服务器B用户名
  # 在服务器B上执行
  ssh-copy-id root@服务器A用户名
  ```

  > 输入远程用户的密码后，SSH公钥就会自动上传了。SSH公钥保存在远程Linux服务器的**.ssh/authorized_keys**文件中

参考：[SSH无密码登录：只需两个简单步骤 (Linux)](https://www.linuxdashen.com/ssh-key%EF%BC%9A%E4%B8%A4%E4%B8%AA%E7%AE%80%E5%8D%95%E6%AD%A5%E9%AA%A4%E5%AE%9E%E7%8E%B0ssh%E6%97%A0%E5%AF%86%E7%A0%81%E7%99%BB%E5%BD%95)

## 设置系统时间为中国时区并启用NTP同步

```shell
yum install ntp //安装ntp服务
systemctl enable ntpd //开机启动服务
systemctl start ntpd //启动服务
timedatectl set-timezone Asia/Shanghai //更改时区
timedatectl set-ntp yes //启用ntp同步
ntpq -p //同步时间
```

参考资料：[CentOS 7 时间, 日期设置 (含时间同步）](https://www.cnblogs.com/tangxiaosheng/p/4986375.html)

## /etc/resolv.conf文件简析

**/etc/resolv.conf**是**DNS客户机配置文件**，用于设置DNS服务器的IP地址及DNS域名，还包含了**主机的域名搜索顺序**。

该文件是由**域名解析器**（resolver，一个根据主机名解析IP地址的库）使用的配置文件。它的格式很简单，每行以一个关键字开头，后接一个或多个由空格隔开的参数。

```shell
nameserver    //定义DNS服务器的IP地址
domain       //定义本地域名
search        //定义域名的搜索列表
sortlist        //对返回的域名进行排序
```

