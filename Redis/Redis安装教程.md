# 源文件安装

在CentOS和Red Hat系统中，首先添加EPEL仓库，然后更新yum源：

```shell
sudo yum install epel-release
sudo yum update
```

然后安装Redis数据库：

```shell
sudo yum -y install redis
```

安装好后启动Redis服务即可：

```shell
sudo systemctl start redis
```

这里同样可以使用redis-cli进入Redis命令行模式操作。

另外，为了可以使Redis能被远程连接，需要修改配置文件，路径为/etc/redis.conf

```shell
vi /etc/redis.conf
```

修改

```shell
bind 127.0.0.1
```

然后重启Redis服务，使用的命令如下：

```shell
sudo systemctl restart redis
```