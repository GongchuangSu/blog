## 下载和安装

官网下载地址：https://skywalking.apache.org/zh/downloads/

## 部署Skywalking

```shell
# apache-skywalking-apm-bin/bin/startup.sh
./startup.sh
```

## 启动服务

```shell
# 启动Eureka服务
java -javaagent:/Users/su/Documents/Skywalking/apache-skywalking-apm-bin/agent/skywalking-agent.jar=agent.service_name=Eureka -jar ./egova-eureka7001/target/egova-eureka7001-0.0.1-SNAPSHOT.jar

# 启动多媒体服务
java -javaagent:/Users/su/Documents/Skywalking/apache-skywalking-apm-bin/agent/skywalking-agent.jar=agent.service_name=ServiceMedia -jar ./egova-service-media/target/egova-service-media-1.0.0.jar
```

## 停止Skywalking服务脚本

```shell
OAP_NAME=OAPServerStartUp
WEBAPP_NAME=skywalking-webapp.jar

tpid=`ps -ef|grep $OAP_NAME |grep -v grep|grep -v kill|awk '{print $2}'`
if [ ${tpid} ]; then
    echo 'Stop Process '$OAP_NAME'...'
    kill -15 $tpid
fi

tpid=`ps -ef|grep $WEBAPP_NAME |grep -v grep|grep -v kill|awk '{print $2}'`
if [ ${tpid} ]; then
    echo 'Stop Process '$WEBAPP_NAME'...'
    kill -15 $tpid
fi
```

# 参考资源

1. https://segmentfault.com/a/1190000020192440