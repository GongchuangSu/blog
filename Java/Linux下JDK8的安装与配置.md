- 下载JDK8安装包

  ```shell
  wget https://download.oracle.com/otn/java/jdk/8u231-b11/5b13a193868b4bf28bcb45c792fce896/jdk-8u231-linux-x64.tar.gz
  ```

- 解压至指定目录

  ```shell
  tar -zxf jdk-8u231-linux-x64.tar.gz -C /usr/local
  ```

- 修改目录名称

  ```shell
  mv /usr/local/jdk1.8.0._231 /usr/local/jdk8
  ```

- 设置JDK环境变量

  ```shell
  # 进入配置文件
  vi /etc/profile
  # 配置环境变量
  export JAVA_HOME=/usr/local/jdk8
  export JRE_HOME=${JAVA_HOME}/jre
  export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
  export  PATH=${JAVA_HOME}/bin:$PATH
  # 保存退出并重新加载此配置文件
  source /etc/profile
  ```

- 验证是否安装成功

  ```shell
  [root@localhost ~]# java -version
  java version "1.8.0_231"
  Java(TM) SE Runtime Environment (build 1.8.0_231-b11)
  Java HotSpot(TM) 64-Bit Server VM (build 25.231-b11, mixed mode)
  ```

  