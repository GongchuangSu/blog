## 配置

修改kafka中的sever.properties文件下的属性

```shell
broker.id=0
log.dir=/usr/local/kafka/kafka-logs
#配置zookeeper管理kafka的路径
zookeeper.connect=localhost:2181
#配置kafka的监听端口
listeners=PLAINTEXT://:9092
#把kafka的地址端口注册给zookeeper，如果是远程访问要改成外网IP
advertised.listeners=PLAINTEXT://192.168.213.145:9092
```

## 后台启动

- 方式一

  ```shell
  ./kafka-server-start.sh -daemon ../config/server.properties
  ```

- 方式二

  ```shell
  nohup ./kafka-server-start.sh ../config/server.properties 1>/dev/null 2>&1 &
  ```

# 参考链接

- [Centos安装Kafka](https://www.cnblogs.com/linjiqin/p/13196347.html)

