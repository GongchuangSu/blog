# 常用命令
- 查看当前哪些容器正在运行
```shell l
supersu@supersu:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
844c376a4b14        jpress              "catalina.sh run"   25 minutes ago      Up 25 minutes       0.0.0.0:8888->8080/tcp   jolly_beaver
```
- 查看所有容器
```shell
supersu@supersu:~$ docker ps -a
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                      PORTS                    NAMES
c675743c1dbc        jpress                        "catalina.sh run"        24 hours ago        Up 24 hours                 0.0.0.0:8888->8080/tcp   stupefied_yalow
df3db18d4f64        jpress                        "catalina.sh run"        32 hours ago        Exited (0) 24 hours ago                              trusting_montalcini
844c376a4b14        6a2947234d0c                  "catalina.sh run"        33 hours ago        Exited (143) 33 hours ago                            jolly_beaver
```
- 删除容器
```shell
supersu@supersu:~$ docker rm [CONTAINERID]
```

- 启动/停止/重启容器

```
docker start container_name/container_id
docker stop container_name/container_id
docker restart container_name/container_id
```

- 搜索镜像

```
docker search httpd
```

- 查看镜像
```shell
supersu@supersu:~$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
jpress                         latest              6a2947234d0c        25 minutes ago      352MB
hello-world                    latest              4ab4c602aa5e        2 months ago        1.84kB
hub.c.163.com/library/tomcat   latest              72d2be374029        15 months ago       292MB
hub.c.163.com/library/nginx    latest              46102226f2fd        19 months ago       109MB
```
- 下载镜像
```shell
docker pull hub.c.163.com/library/tomcat:latest
```
- 删除镜像
```shell
docker image rm [imageName]
docker rmi b39c68b7af30
```
- 创建并编辑Dockerfile文件
```shell
vi Dockerfile
```
内容如下：
```shell
from hub.c.163.com/library/tomcat

MAINTAINER SUSU xxx@163.com

COPY jpress.war /usr/local/tomcat/webapps

```
- 创建image镜像
```shell
supersu@supersu:~/Document/Docker$ docker build -t jpress:latest .
Sending build context to Docker daemon  59.19MB
Step 1/3 : from hub.c.163.com/library/tomcat
 ---> 72d2be374029
Step 2/3 : MAINTAINER SUSU xxx@163.com
 ---> Using cache
 ---> 2c7858e7ab0f
Step 3/3 : COPY jpress.war /usr/local/tomcat/webapps
 ---> Using cache
 ---> d6d73534106d
Successfully built d6d73534106d
Successfully tagged jpress:latest
```
- 查看日志

```shell
docker logs container_name/container_id -f
```

- 运行镜像并指定端口
```shell
docker run -d -p 8888:8080 jpress
```

- 查看容器IP

```shell
docker inspect 容器ID | grep IPAddress 
```

- 进入容器
```shell
docker exec -it df3db18d4f64 bash
```

- 退出容器
```shell
exit
```


## 安装Portainer

```shell
$ docker volume create portainer_data
$ docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

## 安装Mysql

### Mysql 5.7

```shell
docker run --name sgc-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=egova -d mysql:5.7 --character-set-server=utf8  --collation-server=utf8_general_ci --lower_case_table_names=1
```

### Mysql 8.0

```shell
docker run --name sgc-mysql-8.0 -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=egova -e TZ="Asia/Shanghai" --restart always mysql:8.0 --lower_case_table_names=1
```

### 时区设置问题

```shell
set time_zone='+08:00'; 
set global time_zone='+08:00';
# my.cnf文件配置以下内容
[mysqld]
...
default-time_zone='+8:00'
...
```

## 安装Redis

```shell
docker run -d -p 6379:6379 -e TZ="Asia/Shanghai" --name sgc-redis --restart always  redis:3.2
```

## 安装Cetus

```shell
# 拉取镜像
docker pull ledetech/cetus 
# 运行
docker run -d -P -it -p 3309:3309 --name sgc-cetus --restart always ledetech/cetus
```



## Dockerfile for Tomcat

### 多媒体服务

#### 定义Dockerfile

```shell
# docker file for HttpFileServer from supersu
# VERSION 0.0.1
# Author: supersu
# 基础镜像
FROM tomcat:7-jre8
# 作者
MAINTAINER supersu <sgc0515@gmail.com>

# 定义工作目录
ENV WORK_PATH /egova
# 定义tomcat配置目录
ENV TOMCAT_PATH /usr/local/tomcat/conf
# 定义要替换的server.xml文件名
ENV SERVER_CONF_FILE_NAME server.xml
# 删除原文件server.xml
RUN rm $TOMCAT_PATH/$SERVER_CONF_FILE_NAME \
    && mkdir -p $TOMCAT_PATH/Catalina/localhost
# 复制文件server.xml(修改8080端口为8089)
COPY  ./$SERVER_CONF_FILE_NAME $TOMCAT_PATH/
COPY  ./HttpFileServer.xml ./MediaRoot.xml $TOMCAT_PATH/Catalina/localhost/

WORKDIR $WORK_PATH
COPY ./oneinstall-web-HttpFileServer.tar.gz $WORK_PATH/ 
COPY ./MediaRoot.tar.gz $WORK_PATH/ 
RUN tar -zxf $WORK_PATH/MediaRoot.tar.gz \
    && tar -zxf $WORK_PATH/oneinstall-web-HttpFileServer.tar.gz \
    && rm $WORK_PATH/oneinstall-web-HttpFileServer.tar.gz \
    && rm $WORK_PATH/MediaRoot.tar.gz
EXPOSE 8089
```
#### 制作镜像
```shell
docker build -t supersu/media-tomcat:v0.1 .
docker run --name mediaServer -it -p 8089:8089 supersu/media-tomcat:v0.1 /bin/bash
```

# 容器中安装Vim

```shell

mv /etc/apt/sources.list /etc/apt/sources.list.bak
    echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib" >/etc/apt/sources.list
    echo "deb http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list
    echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib" >>/etc/apt/sources.list
    echo "deb-src http://mirrors.163.com/debian/ jessie-proposed-updates main non-free contrib" >>/etc/apt/sources.list
    #更新安装源
    apt-get update
    
apt-get install -y vim
```

