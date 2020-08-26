## jps

jps(Java Virtual Machine Process Status Tool)：用来输出JVM中运行的进程状态信息。

```shell
jps [options] [hostid]  
```

说明：

- 如果不指定hostid就默认为当前主机或服务器。

- 参数：

  ```java
  -q 不输出类名、Jar名和传入main方法的参数  
  -m 输出传入main方法的参数  
  -l 输出main类或Jar的全限名  
  -v 输出传入JVM的参数  
  ```

常用用法：

```shell
[root@cg4 ~]# jps -lvm
11824 sun.tools.jps.Jps -lvm -Dapplication.home=/usr/java/jdk1.8.0_192-amd64 -Xms8m
7860 egova-service-recognition-1.0.0.jar --MYSQL_URL=jdbc:mysql://172.16.0.6:3306/cgdb?useUnicode=true&characterEncoding=utf-8 --SERVER_PORT=8019 --EUREKA_ENABLE=false --MYSQL_USERNAME=root --MYSQL_PASSWORD=ovN!6EYT0F --MODEL_PATH=/egova/recognition/data/model.ser --UNIT_MODEL_PATH=/egova/recognition/data/unitModel.ser --LUCENE_DATA_PATH=/egova/recognition/data/lucene -Xms2g -Xmx4g
1910 org.tanukisoftware.wrapper.WrapperSimpleApp CloudResetPwdUpdateAgent -Dorg.tanukisoftware.wrapper.WrapperSimpleApp.maxStartMainWait=40 -Djava.library.path=../lib -Dwrapper.key=OuRGFsO7J0xlAuvj -Dwrapper.backend=pipe -Dwrapper.disable_console_input=TRUE -Dwrapper.pid=1464 -Dwrapper.version=3.5.26 -Dwrapper.native_library=wrapper -Dwrapper.arch=x86 -Dwrapper.service=TRUE -Dwrapper.cpu.timeout=10 -Dwrapper.jvmid=1
27302 org.apache.catalina.startup.Bootstrap start -Djava.util.logging.config.file=/egova/apache-tomcat-7.0.105/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -XX:PermSize=256M -XX:MaxPermSize=1024m -Xms1024M -Xmx4096M -XX:MaxNewSize=256m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/egova/log -Dfile.encoding=UTF-8 -Dsun.jnu.encoding=UTF-8 -Duser.timezone=GMT+08 -Djava.awt.headless=true -Djdk.tls.ephemeralDHKeySize=2048 -Dorg.apache.catalina.security.SecurityListener.UMASK=0027 -Dignore.endorsed.dirs= -Dcatalina.base=/egova/apache-tomcat-7.0.105 -Dcatalina.home=/egova/apache-tomcat-7.0.105 -Djava.io.tmpdir=/egova/apache-tomcat-7.0.105/temp
```

## jstack

jstack：主要用来查看某个Java进程内的线程堆栈信息

jstack可以定位到线程堆栈，根据堆栈信息我们可以定位到具体代码，所以它在JVM性能调优中使用得非常多。

具体实例可查看[JVM性能调优笔记](./JVM性能调优笔记.md)

## jstat

jstat（JVM统计检测工具）：用于查看各个区内存和GC的情况

语法格式：

```shell
jstat [ generalOption | outputOptions vmid [interval[s|ms] [count]] ]  
```
说明：vmid是Java虚拟机ID，在Linux/Unix系统上一般就是进程ID。interval是采样时间间隔。count是采样数目

实例:

JVM堆内存：
```
堆内存 = 年轻代 + 年老代 + 永久代  
年轻代 = Eden区 + 两个Survivor区（From和To） 
```

各列含义：
```
S0C、S1C、S0U、S1U：Survivor 0/1区容量（Capacity）和使用量（Used）  
EC、EU：Eden区容量和使用量  
OC、OU：年老代容量和使用量  
PC、PU：永久代容量和使用量  
YGC、YGT：年轻代GC次数和GC耗时  
FGC、FGCT：Full GC次数和Full GC耗时  
GCT：GC总耗时  
```

```shell
[root@cg4 ~]# jstat -gc 27302 1s 10
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT   
34816.0 34304.0  0.0    0.0   193024.0 190570.1 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 191201.5 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 191201.5 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 191211.4 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 191211.4 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 191211.4 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 191215.5 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 192029.7 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 192029.7 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
34816.0 34304.0  0.0    0.0   193024.0 192031.7 2738176.0  1476403.5  379968.0 365219.6 41856.0 39348.5   1279   24.363   8      7.091   31.455
```

## jmap

jmap(Java Virtual Machine Memory Map)：用来查看堆内存使用状况，一般结合jhat使用
jhat(Java Heap Analysis Tool)：堆数据分析工具

jmap导出堆内存，然后使用jhat来进行分析。

### 用法
#### 查看进程堆内存使用情况
包括使用的GC算法、堆配置参数和各代中堆内存使用
```shell
[root@cg4 ~]# jmap -heap 27302
Attaching to process ID 27302, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.192-b12

using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 4294967296 (4096.0MB)
   NewSize                  = 268435456 (256.0MB)
   MaxNewSize               = 268435456 (256.0MB)
   OldSize                  = 805306368 (768.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 197656576 (188.5MB)
   used     = 116035984 (110.66053771972656MB)
   free     = 81620592 (77.83946228027344MB)
   58.7058555542316% used
From Space:
   capacity = 35651584 (34.0MB)
   used     = 26958080 (25.709228515625MB)
   free     = 8693504 (8.290771484375MB)
   75.61537798713235% used
To Space:
   capacity = 35127296 (33.5MB)
   used     = 0 (0.0MB)
   free     = 35127296 (33.5MB)
   0.0% used
PS Old Generation
   capacity = 2438987776 (2326.0MB)
   used     = 1945760160 (1855.6214904785156MB)
   free     = 493227616 (470.3785095214844MB)
   79.77736416502647% used
```

#### 查看堆内存中对象的数目
如果带上live则只统计活对象：jmap -histo[:live] pid
```shell
[root@cg4 ~]# jmap -histo:live 27302|more

 num     #instances         #bytes  class name
----------------------------------------------
   1:        149670      293711496  [B
   2:       2342534      216694792  [C
   3:       1619016      142473408  java.lang.reflect.Method
   4:       3783264      121064448  java.util.HashMap$Node
   5:        401153       59971528  [Ljava.util.HashMap$Node;
   6:       2324967       55799208  java.lang.String
   7:       1344178       43013696  java.util.concurrent.ConcurrentHashMap$Node
   8:        816269       32650760  java.util.LinkedHashMap$Entry
   9:        671285       32221680  org.aspectj.weaver.
```
class name为对象类型，说明如下：
```
B  byte  
C  char  
D  double  
F  float  
I  int  
J  long  
Z  boolean  
[  数组，如[I表示int[]  
[L+类名 其他对象  
```
#### dump内存使用情况
dump文件可以通过jhat分析查看，也可以使用VisualVM等工具查看。
dump命令格式：
```shell
jmap -dump:format=b,file=dumpFileName pid
```
实例：
```shell
[root@cg4 ~]# jmap -dump:format=b,file=/egova/dump.dat 27302
Dumping heap to /egova/dump.dat ...
Heap dump file created
```
导出dump文件后使用jhat进行分析：
```shell
[root@cg4 ~]# jhat -J-Xmx512m -port 8888 /egova/dump.dat 
Reading from /egova/dump.dat...
Dump file created Wed Aug 26 10:43:35 GMT+08:00 2020
```
注意：导出的文件可能比较大，加载分析比较耗时，不推荐使用