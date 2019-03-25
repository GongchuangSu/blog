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
- 运行镜像并指定端口
```shell
docker run -d -p 8888:8080 jpress
```
- 进入容器
```shell
docker exec -it df3db18d4f64 bash
```