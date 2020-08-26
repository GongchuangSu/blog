## Tomcat目录层次结构

- bin：存放启动和关闭Tomcat的脚本文件
  - catalina.sh，用于启动和关闭tomcat服务器
  - configtest.sh，用于检查配置文件
  - startup.sh，启动Tomcat脚本
  - shutdown.sh，关闭Tomcat脚本
  - setclasspath.sh，catalina.sh依赖这个脚本来设置一些运行时变量
- conf：存放Tomcat服务器的各种配置文件
  - server.xml：Tomcat 的全局配置文件
  - web.xml：为不同的Tomcat配置的web应用设置缺省值的文件
  - tomcat-users.xml：Tomcat用户认证的配置文件
- lib: 存放Tomcat服务器支撑的jar包
- logs: 存放Tomcat的日志文件
- temp: 存放Tomcat运行时产生的临时文件
- webapps：Tomcat的主要Web发布目录，即供外界访问的web资源的存放目录
- work: Tomcat的工作目录

> 说明：
>
> 1. 脚本version.sh、startup.sh、shutdown.sh、configtest.sh都是对catalina.sh的包装，内容大同小异，差异在于功能介绍和调用catalina.sh时的参数不同。

### catalina.sh文件配置详解

用文本编辑器打开catalina.sh，首先可以看到一些环境变量的说明，基于功能可以做如下分类：

- 指定tomcat和应用的启动路径
  - CATALINA_HOME，指向tomcat的安装路径，或者脚本catalina.sh所在位置的父目录。
  - CATALINA_BASE，可选项，如无定义，则与CATALINA_HOME取值相同。用于指向用户自定义的tomcat路径，tomcat启动时将以CATALINA_BASE变量值来计算配置文件、日志文件等的路径。
- Java运行时路径，tomcat运行依赖系统已安装好的Java运行时软件，因此如下两个变量中至少定义一个，并且变量值指向合法路径。
  - JAVA_HOME，指定JDK的安装路径。
  - JRE_HOME，指定JRE的安装路径。取值原则，如用户未指定JRE_HOME，则取JAVA_HOME的值；如JRE_HOME和JAVA_HOME都有定义，则优先取JRE_HOME的定义。
- Java运行时参数
  - JAVA_OPTS，顾名思义，用来定义传递Java运行时的参数，对全部命令行选项都会生效，因此不建议在本变量中定义堆大小、GC参数、JMX参数等。
  - CATALINA_OPTS，用来定义传递给Java运行时的参数，仅在命令行选项为run/start/debug时有效，可以用来传递堆大小、GC参数、JMX参数等。
  - CATALINA_TMPDIR，用来定义java.io.tmpdir变量的值，默认值为$CATALINA_BASE/temp。
  - JAVA_ENDORSED_DIRS，以分号相隔的目录列表，用来替换非JCP提供的jar实现，默认值为$CATALINA_HOME/endorsed。
- 远程调试参数。当开发环境与应用运行环境不一致，或者开发环境下出于各种原因不适合调试应用时，这时即可借助远程调试来解决问题。JPDA的介绍可参考阅读资料。
  - JPDA_TRANSPORT，默认值dt_socket。
  - JPDA_ADDRESS，默认值localhost:8000。
  - JPDA_SUSPEND，默认值n
  - JPDA_OPTS，默认值为-agentlib:jdwp=transport=$JPDA_TRANSPORT,address=$JPDA_ADDRESS,server=y,suspend=$JPDA_SUSPEND
- 其它
  - CATALINA_OUT，使用全路径方式指定stdout和stderr重定向之后的文件名，默认值为$CATALINA_BASE/logs/catalina.out。
  - CATALINA_PID，本选项在*nix下有效，指定pid文件的存储路径，默认值为空，如用户指定，则启动、停止时都会从变量指定的文件中提取进程ID。
  - LOGGING_CONFIG，定义tomcat使用的日志配置文件的路径，默认值为$CATALINA_BASE/conf/logging.properties。
  - LOGGING_MANAGER，定义tomcat使用的日志管理器。

如何定义catalina.sh提供的变量？

- 方法一，修改catalina.sh，在脚本的开始位置为前述变量赋值。
- 方法二，提供自定义的启动、停止脚本，在自定义的脚本中为前述变量赋值。
- 方法三，使用tomcat开发人员预留的扩展方法，在$CATALINA_BASE/bin/setenv.sh中为前述变量赋值。

### tomcat用户管理权限

配置文件地址：`conf/tomcat-users.xml`

```xml
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <user username="tomcat" password="tomcat" roles="tomcat"/>
  <user username="both" password="tomcat" roles="tomcat,role1"/>
  <user username="role1" password="tomcat" roles="role1"/>
  <user username="root" password="root" roles="tomcat,role1,manager-gui,manager-script,manager-jmx,manager-status"/>
```

### 如何在线调整日志级别

```java
PropertyConfigurator.configure(configFileName)
```

