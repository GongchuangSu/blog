1.Webservice
- Webservice是跨平台，跨语言的远程调用技术
- 通信机制实质就是xml数据交换
- 采用了soap协议（简单对象协议）进行通信

---
2.方法名是否可以与类名同名？
```java
public class Solution{
	String str;
	public Solution() {
		str = "hello!";
	}
	
	public void Solution(){
		System.out.println(str);
	}
	
	public static void main(String[] args){
		Solution s = new Solution();
		s.Solution();
	}
}
/**
hello!
*/
```
从以上代码可以看出，方法名可以与类名同名，它与构造函数的唯一区别在于：构造函数没有返回值。

---
3.Java反射机制主要提供了以下功能：
- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时调用任意一个对象的方法

---
4.Java中术语解释
- **容器**：一种服务调用规范框架，J2EE 大量运用了容器和组件技术来构建分层的企业级应用。在 J2EE 规范中，相应的有 WEB Container 和 EJB Container 等
- **WEB容器**：给处于其中的应用程序组件（JSP，SERVLET）提供一个环境，使 JSP，SERVLET 直接跟容器中的环境变量交互，不必关注其它系统问题（从这个角度来说，web 容器应该属于架构上的概念）。web 容器 主要由 WEB 服务器来实现。例如：TOMCAT，WEBLOGIC，WEBSPHERE 等
- **Servlet**（Server Applet）：全称 Java Servlet，未有中文译文。是用 Java 编写的服务器端程序。其主要功能在于交互式地浏览和修改数据，生成动态 Web 内容。狭义的 Servlet 是指 Java 语言实现的一个接口，广义的 Servlet 是指任何实现了这个 Servlet 接口的类，一般情况下，人们将 Servlet 理解为后者
- **EJB容器**：为应用程序服务器中的企业 Bean 提供运行时环境。容器处理应用程序服务器中企业 bean 操作的各个方面，并充当 Bean 中用户编写的业务逻辑与应用程序环境中其他方面之间的中介
- **JNDI**：（Java Naming & Directory Interface）JAVA命名目录服务。主要提供的功能是：提供一个目录系，让其它各地的应用程序在其上面留下自己的索引，从而满足快速查找和定位分布式应用程序的功能
- **JMS**：（Java Message Service）JAVA消息服务。主要实现各个应用程序之间的通讯。包括点对点和广播
- **JTA**：（Java Transaction API）JAVA事务服务。提供各种分布式事务服务。应用程序只需调用其提供的接口即可
- **JAF**：（Java Action FrameWork）JAVA安全认证框架。提供一些安全控制方面的框架。让开发者通过各种部署和自定义实现自己的个性安全控制策略
- **RMI/IIOP**:（Remote Method Invocation /internet对象请求中介协议）他们主要用于通过远程调用服务。例如，远程有一台计算机上运行一个程序，它提供股票分析服务，我们可以在本地计算机上实现对其直接调用。当然这是要通过一定的规范才能在异构的系统之间进行通信。RMI是JAVA特有的

---
5.面向对象的五大基本原则
1. 单一职责原则（Single-Resposibility Principle）：一个类，最好只做一件事，只有一个引起它的变化。单一职责原则可以看做是低耦合、高内聚在面向对象原则上的引申，将职责定义为引起变化的原因，以提高内聚性来减少引起变化的原因
2. 开放封闭原则（Open-Closed principle）：软件实体应该是可扩展的，而不可修改的。也就是，对扩展开放，对修改封闭的
3. 里氏替换原则（Liskov-Substituion Principle）：子类必须能够替换其基类。这一思想体现为对继承机制的约束规范，只有子类能够替换基类时，才能保证系统在运行期内识别子类，这是保证继承复用的基础
4. 依赖倒置原则（Dependecy-Inversion Principle）：依赖于抽象。具体而言就是高层模块不依赖于底层模块，二者都同依赖于抽象；抽象不依赖于具体，具体依赖于抽象
5. 接口隔离原则（Interface-Segregation Principle）：使用多个小的专门的接口，而不要使用一个大的总接口

---
6.if语句比较问题
除boolean外的其他类型都不能使用赋值语句，否则会提示无法转成布尔值。
```java
public class Solution{
	
	public static void main(String[] args){
		boolean flag = false;
		if(flag = true){
			System.out.println("true");
		}else{
			System.out.println("false");
		}
	}
}
/*output
true
*/
```
如果将boolean改为其它类型，比如int型
```java
public class Solution{
	
	public static void main(String[] args){
		int flag = 0;
		if(flag = 1){
			System.out.println("true");
		}else{
			System.out.println("false");
		}
	}
}
/*output
编译错误：Type mismatch: cannot convert from int to boolean
*/
```

---
7.基本Java类型中数据的默认类型
如果没有明确指出，整数型的默认类型为int；带小数的默认为double型

---
8.Java Math的 floor,round和ceil的总结
- floor 向下取整 
- ceil  向上取整 
- round 则是4舍5入的计算

---
9.自动转换按从低到高的顺序转换。不同类型数据间的优先关系如下：
byte,short,char-> int -> long -> float -> double(由低到高)

---
10.ResultSet数组是从1开始遍历

---
11.JDK中的包及其基本功能
- java.awt：包含构成抽象窗口工具集的多个类，用来构建和管理应用程序的图形用户界面
- java.lang：提供java编成语言的程序设计的基础类
- java.io：包含提供多种输出输入功能的类，
- java.net：包含执行与网络有关的类，如URL，SCOKET，SEVERSOCKET，
- java.applet：包含java小应用程序的类
- java.util：包含一些实用性的类

---
12.true,false,null,friendly，sizeof不是java的关键字,但是不能把它们作为java标识符用

---
13.集合线程安全性问题
除了vector、stack、hashtable、enumeration这些之外，其他的都是非线程安全的类和接口

---
14.Java语言中，中文字符所占的字节数取决于字符的编码方式，一般情况下，采用ISO8859-1编码方式时，一个中文字符与一个英文字符一样只占1个字节；采用GB2312或GBK编码方式时，一个中文字符占2个字节；而采用UTF-8编码方式时，一个中文字符会占3个字节

---
15.移位运算符
- <<：表示左移位
- ">>"：表示带符号右移
- ">>>"：表示无符号右移
- 不存在"<<<"运算符

---
16.枚举类中所有的枚举值都是类静态常量，在初始化时会对所有的枚举值对象进行第一次初始化。也就是说，有几个枚举值对象就会调用几次构造方法

---
17.静态方法中没有this指针，故在类方法中不可用this来调用本类的类方法，但可用实例对象调用

---
18.Java语言中的异常处理包括声明异常、抛出异常、捕获异常和处理异常四个环节。
- throw：用于抛出异常。
- throws：关键字可以在方法上声明该方法要抛出的异常，然后在方法内部通过throw抛出异常对象。
- try：用于检测被包住的语句块是否出现异常，如果有异常，则抛出异常，并执行catch语句。
- cacth：用于捕获从try中抛出的异常并作出处理。
- finally：语句块是不管有没有出现异常都要执行的内容

---
19.构造方法不能被static、final、synchronized、abstract、native修饰，但可以被public、private、protected修饰

---
20.Java异常体系
![image](http://note.youdao.com/yws/res/563/WEBRESOURCE82c1745e3e1e406a1089edd098eba235)

---
21.Statement,PreparedStatement,CallableStatement的区别(重要)   
1)功能  
Statement：接口提供了执行语句和获取结果的基本方法   
PreparedStatement：接口添加了处理 IN 参数的方法   
CallableStatement：接口添加了处理 OUT 参数的方法   

2)用法  
Statement :
- 普通的不带参的查询SQL；
- 每次执行SQL语句，都需要编译   

PreparedStatement :
- 继承自Statement
- 可变参数的SQL,编译一次,执行多次,效率高;
- 安全性好，有效防止Sql注入等问题;
- 支持批量更新,批量删除;   

CallableStatement :
- 继承自PreparedStatement,支持带参SQL操作;
- 支持调用存储过程,提供了对输出和输入/输出参数的支持;

---
22.项目中遇到过什么样的问题？又是怎样解决的？
涉及到用户权限管理，该怎样处理？。。。待解决

---
23.Java是编译型语言还是解释型语言？
[Java和JVM运行原理](http://gongchuangsu.com/2016/03/01/Java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/Java%E5%92%8CJVM%E8%BF%90%E8%A1%8C%E5%8E%9F%E7%90%86/)

---
24.Struts2 VS SpringMVC
- struts2框架是类级别的拦截，每次来了请求就创建一个Action，然后调用setter getter方法把request中的数据注入
- struts2实际上是通过setter getter方法与request打交道的 
- struts2中，一个Action对象对应一个request上下文
- spring3 mvc不同，spring3mvc是方法级别的拦截，拦截到方法后根据参数上的注解，把request数据注入进去 
- 在spring3mvc中，一个方法对应一个request上下文 

---
25.控制反转（IoC）与依赖注入（DI）
http://blog.xiaohansong.com/2015/10/21/IoC-and-DI/

---
26.HTTP和HTTPS的主要区别
HTTP是HTTP协议运行在TCP之上，所有传输的内容都是明文，客户端和服务器端都无法验证对方的身份；而HTTPS是HTTP协议运行在SSL/TLS之上，在SSL/TLS运行在TCP之上，所有传输的内容都经过加密。HTTPS与HTTP最大的不同即在HTTP与TCP之间多了一个加密/身份验证层。

---
27.面向过程和面向对象的本质理解
- 面向过程就是分析出解决问题所需的步骤，面向对象则是把构成问题的事物分解成对象，抽象出对象的目的并不在于完成某个步骤，而是描述其再整个解决问题的步骤中的行为。 
- 面向过程的思维方式是分析综合，面向对象的思维方式是构造。

---
28. interrupted() 和isInterrupted()的区别
   interrupted() 和 isInterrupted()都能够用于检测对象的“中断标记”。区别是，interrupted()除了返回中断标记之外，它还会清除中断标记(即将中断标记设为false)；而isInterrupted()仅仅返回中断标记。

---
29.数据完整性可以分为四类。
1、实体完整性，实体完整性的目的是确保数据库中所有实体的唯一性，也就是不应出现完全相同的数据记录。
2、区域完整性，匹配完整性要求数据表中的数据位于某一个特定的允许范围内。
3、参考完整性，是用来维护相关数据表之间数据一致性的手段，通过实现参考完整性，可以避免因一个数据表的记录改变而造成另一个数据表内的数据变成无效值。
4、用户自定义完整性，用户自定义由用户根据实际应用中的需要自行定义。

---
30.
- DML（data manipulation language）是数据操纵语言：它们是UPDATE、INSERT、DELETE/SELECT，就象它的名字一样，这4条命令是用来对数据库里的数据进行操作的语言，增删查改。 
- DDL（data definition language）是数据定义语言：DDL比DML要多，主要的命令有CREATE、ALTER、DROP等，DDL主要是用在定义或改变表（TABLE）的结构，数据类型，表之间的链接和约束等初始化工作上，他们大多在建立表时使用。
- DCL（DataControlLanguage）是数据库控制语言：是用来设置或更改数据库用户或角色权限的语句，包括（grant,deny,revoke等）语句。
- DQL（ Data Query Language）  
  数据查询语言，数据查询语言DQL基本结构是由SELECT子句，FROM子句，WHERE 
  子句组成的查询块： 
  SELECT <字段名表> 
  FROM <表或视图名> 
  WHERE <查询条件>

---
31.与Mysql服务器相互作用的通讯协议包括TCP/IP，Socket，共享内存，命名管道。

---
32.关系数据库中用二维表来表示实体之间的联系

---
33.Ant和Maven

Ant特点：
- 没有一个约定的目录结构
- 必须明确让ant做什么，什么时候做，然后编译，打包
- 没有生命周期，必须定义目标及其实现的任务序列
- 没有集成依赖管理

Maven特点：
- 拥有约定，知道你的代码在哪里，放到哪里去
- 拥有一个生命周期，例如执行 mvn install 就可以自动执行编译，测试，打包等构建过程
- 只需要定义一个pom.xml,然后把源码放到默认的目录，Maven帮你处理其他事情
- 拥有依赖管理，仓库管理

---
34.HTTP中Get与Post的区别
Http定义了与服务器交互的不同方法，最基本的方法有4种，分别是GET，POST，PUT，DELETE。常用的有GET和POST。
- GET请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，参数之间以&相连
- GET方式提交的数据最多只能是1024字节，理论上POST没有限制，可传较大量的数据
- POST的安全性要比GET的安全性高  

总结：Get是向服务器发索取数据的一种请求，而Post是向服务器提交数据的一种请求，在FORM（表单）中，Method默认为"GET"，实质上，GET和POST只是发送机制不同，并不是一个取一个发！

---
35.redirect和forward的区别
- redirect是服务器发给客户端一个状态码为3XX的响应，由客户端负责跳转，所以浏览器地址栏显示的是跳转后的地址。<jsp:forward page = "for2.jsp"/>
- forward又叫转发，是服务器内部的跳转，客户端是不知道的，所以浏览器地址栏显示的是跳转前的地址。 <%response.sendRedirect("for2.jsp");%>

---
36.<%@ include file=""%>与<jsp:include page=""/>区别
- 执行时间上:    
  <%@ include file=”relativeURI”%> 是在翻译阶段执行   
  <jsp:include page=”relativeURI” flush=”true” /> 在请求处理阶段执行。 
- 引入内容的不同:   
  <%@ include file=”relativeURI”%> 引入静态文本(html,jsp),在JSP页面被转化成servlet之前和它融和到一起。   
  <jsp:include page=”relativeURI” flush=”true” /> 引入执行页面或servlet所生成的应答文本。   

【注】：JSP容器负责将jsp页面转化成servlet，并编译这个servlet。这两个步骤就构成了翻译阶段。

---
37.EJB 与 javabean 的区别
- JavaBean是一个组件，而EJB就是一个组建框架
- JavaBean面向的是业务逻辑和表示层的显示，通过编写一个JavaBean，可以将业务逻辑的事件和事务都放在其中，然后通过它的变量属性将所需要的内容在表示层传递显示。
- EJB是部署在服务器上的可执行组建或商业对象。EJB有一个部署描述符，通过这个部署描述符可以对EJB的属性进行描述。EJB不和表示层交互。
- 通常，对于简单的服务器端应用来说，使用JavaBean是很不错的选择。虽然对于复杂的服务器端应用来说，使用JavaBean同样可以达到相同的效果，但这么做，所有底层的实现都必须手工来重新编写。而EJB不必用户关心它的底层操作，而只要关心它的外部实现即可。

---
38.JSP中参数传递的方法
1. 直接在URL请求后添加，超链接：<a herf="index.jsp?a=a&b=b&c=c">name</a>
```
// 在href.jsp中设置参数
<a href="getHerf.jsp?name=abc&password=123">传递参数</a>
// 在getHref.jsp取得参数
String name=request.getParameter("name");  
name=new String(name.getBytes("iso-8859-1"),"gb2312");  
```
2. 通过设置<jsp:param>属性
```
// 在param.jsp中设置参数
<jsp:forward page="getParam.jsp">  
    <jsp:param name="name" value="abc"/>  
    <jsp:param name="password" value="123"/>  
</jsp:forward>  
// 在getParam.jsp取得参数
String name=request.getParameter("name");  
String password=request.getParameter("password");
```
3.设置session和request
request.setAttribute()和request.getAttribute()是配合<jsp:forward>或是include指令来实现的。
session.setAttribute()和session.getAttribute()可以多个页面进行参数传递
```
// 通过显示的把参数放置到session和request中，以达到传递参数的目的
session.setAttribute(name,value);  
request.setAttribute(name,value);
// 取参数
value=(value className)session.getAttribute(name);  
value=(value className)request.getAttribute(name); 
```
4. form表单
```
// form.jsp
<form action="result.jsp" method="get" align="center">  
    姓名：<input type="text" name="name" size="20" value="" maxlength="20"><br/>  
    密码：<input type="password" name="password" size="20" value="" maxlength="20"><br/>
</form>       
// result.jsp
<%  
  request.setCharacterEncoding("GB2312");  
  String name=request.getParameter("name");  
  name=new String(name.getBytes("iso-8859-1"),"GB2312"); 
  String pwd=request.getParameter("password");  
%> 
```

---
39.Tomcat与Servlet是如何工作的
![image](http://note.youdao.com/yws/res/1014/WEBRESOURCE7983ad78d4572e0c40dbb2bc8bc57763)
1. Web Cilent 向Servlet容器(Tomcat) 发出Http请求
2. Servlet容器接收Web Client的请求
3. Servlet 容器创建一个HttpRequest对象，将Web Cilent 请求的信息封装到这个对象中
4. Servlet 容器创建一个HttpResponse对象
5. Servlet 容器调用HttpServlet对象的service方法，把HttpRequest对象与HttpResponse 对象作为参数传递给HttpServlet 对象
6. HttpServlet 调用HttpRequest对象的有关方法，获取Http请求信息
7. HttpServlet 调用HttpResponse对象的有关方法，生成响应数据
8. Servlet容器把HttpServlet的响应结果传给Web Cilent

---
40.Java语言的几个主要特点
- 平台无关性：能运行于不同的平台上
- 安全性：去掉了指针操作，内存由操作系统分配
- 面向对象：利用类使程序更加利于维护
- 分布式：可以使用网络文件和使用本机文件一样容易
- 健壮性：检查程序编译和运行的错

---
41.Java的三大核心机制
- 虚拟机机制：利用虚拟机解释字节码执行java程序实现跨平台
- 垃圾收集机制：自动内存回收
- 代码安全性机制：加载代码后校验代码后再执行代码

---
42.JAVA需要配置的相关环境变量
- JAVA_HOME：JDK安装所在目录，如D:\Program Files\Java\jdk1.7.0_79
- path：用于搜索外部命令，例如编译程序的javac命令，执行程序的java命令。配置：%JAVA_HOME%\bin
- classpath：用于搜索类，即class文件，例如可以在不同的位置执行类文件。配置：%JAVA_HOME%\lib

---
43.常见的Unicode字符对应的编码
- 大写的A到Z是对应65到90
- 小写的a到z是对应97到122
- 字符0到9是对应48到57

---
44.取余运算符%
运算得到是正数还是负数取决于第一个操作数的符号
```
-5 % 2 = -1;
5 % -2 = 1;
```

---
45.什么是线程同步，何如实现线程的同步？   
当两个或多个线程需要访问同一资源时，它们需要以某种顺序来确保该资源某一时刻只能被一个线程使用的方式称为同步。   
要想实现同步操作，必须要获得每一个线程对象的锁。获得它可以保证在同一时刻只有一个线程访问对象中的共享关键代码，并且在这个锁被释放之前，其他线程就不能再进入这个共享代码。此时，如果还有其他线程想要获得该对象的锁，只得进入等待队列等待。只有当拥有该对象锁的线程退出共享代码时，锁被释放，等待队列中第一个线程才能获得该锁，从而进入共享代码区。

---
46.简述JDBC调用数据库的基本步骤   
1. 加载驱动
2. 创建连接
3. 获取语句对象
4. 执行sql语句
5. 如果是查询,还可以使用结果集
6. 关闭连接
7. 捕捉和处理异常

---
47.卡特兰公式
有n个数顺序(依次)入栈,出栈序列有C(n)种：
C(n)=(2n)!/(n+1)n!n!

---
48.静态链表优缺点    
优点：在插入和删除操6作时，只需要修改游标，不需要移动元素，从而改进了在顺序存储结构中的插入和删除操作需要移动大量元素的缺点。     
缺点：没有解决连续存储分配带来的表长难以确定的问题。失去了顺序存储结构随机存取的特性。

---
49.三元组转置：     
- 将数组的行列值相互交换
- 将每个三元组的i和j相互交换
- 重排三元组的之间的次序      

---
50.n 个字符构成的字符串，假设每个字符都不一样，有n(n+1)/2 + 1个子串（包括其本身及其空串）

---
51.KMP模式匹配算法的时间复杂度为O(n+m)，朴素模式匹配算法的时间复杂度为O((n-m+1)*m)

---
52.如何将输入流转换成字符串
```
public class StreamTools {
    /**
     * 将输入流转换成字符串
     * 
     * @param is
     *            从网络获取的输入流
     * @return
     */
    public static String streamToString(InputStream is) {
        try {
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len = 0;
            while ((len = is.read(buffer)) != -1) {
                baos.write(buffer, 0, len);
            }
            baos.close();
            is.close();
            byte[] byteArray = baos.toByteArray();
            return new String(byteArray);
        } catch (Exception e) {
            Log.e(tag, e.toString());
            return null;
        }
    }
}
```

---
53.HttpUrlConnection和HttpClient
1. HttpUrlConnection：
- HttpUrlConnection发送GET请求
```
/**
 * 通过HttpUrlConnection发送GET请求
 * 
 * @param username
 * @param password
 * @return
 */
public static String loginByGet(String username, String password) {
    String path = http://192.168.0.107:8080/WebTest/LoginServerlet?username= + username + &password= + password;
    try {
        URL url = new URL(path);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setConnectTimeout(5000);
        conn.setRequestMethod(GET);
        int code = conn.getResponseCode();
        if (code == 200) {
            InputStream is = conn.getInputStream(); // 字节流转换成字符串
            return StreamTools.streamToString(is);
        } else {
            return 网络访问失败;
        }
    } catch (Exception e) {
        e.printStackTrace();
        return 网络访问失败;
    }
}
```
- HttpUrlConnection发送POST请求
```
/**
 * 通过HttpUrlConnection发送POST请求
 * 
 * @param username
 * @param password
 * @return
 */
public static String loginByPost(String username, String password) {
    String path = http://192.168.0.107:8080/WebTest/LoginServerlet;
    try {
        URL url = new URL(path);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setConnectTimeout(5000);
        conn.setRequestMethod(POST);
        conn.setRequestProperty(Content-Type, application/x-www-form-urlencoded);
        String data = username= + username + &password= + password;
        conn.setRequestProperty(Content-Length, data.length() + );
        // POST方式，其实就是浏览器把数据写给服务器
        conn.setDoOutput(true); // 设置可输出流
        OutputStream os = conn.getOutputStream(); // 获取输出流
        os.write(data.getBytes()); // 将数据写给服务器
        int code = conn.getResponseCode();
        if (code == 200) {
            InputStream is = conn.getInputStream();
            return StreamTools.streamToString(is);
        } else {
            return 网络访问失败;
        }
    } catch (Exception e) {
        e.printStackTrace();
        return 网络访问失败;
    }
}
```
2. HttpClient
- HttpClient发送GET请求
```
/**
 * 通过HttpClient发送GET请求
 * 
 * @param username
 * @param password
 * @return
 */
public static String loginByHttpClientGet(String username, String password) {
    String path = http://192.168.0.107:8080/WebTest/LoginServerlet?username=
            + username + &password= + password;
    HttpClient client = new DefaultHttpClient(); // 开启网络访问客户端
    HttpGet httpGet = new HttpGet(path); // 包装一个GET请求
    try {
        HttpResponse response = client.execute(httpGet); // 客户端执行请求
        int code = response.getStatusLine().getStatusCode(); // 获取响应码
        if (code == 200) {
            InputStream is = response.getEntity().getContent(); // 获取实体内容
            String result = StreamTools.streamToString(is); // 字节流转字符串
            return result;
        } else {
            return 网络访问失败;
        }
    } catch (Exception e) {
        e.printStackTrace();
        return 网络访问失败;
    }
}
```
- HttpClient发送POST请求
```
/**
 * 通过HttpClient发送POST请求
 * 
 * @param username
 * @param password
 * @return
 */
public static String loginByHttpClientPOST(String username, String password) {
    String path = http://192.168.0.107:8080/WebTest/LoginServerlet;
    try {
        HttpClient client = new DefaultHttpClient(); // 建立一个客户端
        HttpPost httpPost = new HttpPost(path); // 包装POST请求
        // 设置发送的实体参数
        List<namevaluepair> parameters = new ArrayList<namevaluepair>();
        parameters.add(new BasicNameValuePair(username, username));
        parameters.add(new BasicNameValuePair(password, password));
        httpPost.setEntity(new UrlEncodedFormEntity(parameters, UTF-8));
        HttpResponse response = client.execute(httpPost); // 执行POST请求
        int code = response.getStatusLine().getStatusCode();
        if (code == 200) {
            InputStream is = response.getEntity().getContent();
            String result = StreamTools.streamToString(is);
            return result;
        } else {
            return 网络访问失败;
        }
    } catch (Exception e) {
        e.printStackTrace();
        return 访问网络失败;
    }
}
```
54.HTTP请求&响应
1. HTTP请求包结构    
  ![image](http://upload-images.jianshu.io/upload_images/680540-e04227416a611216.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
POST /meme.php/home/user/login HTTP/1.1
Host: 114.215.86.90
Cache-Control: no-cache
Postman-Token: bd243d6b-da03-902f-0a2c-8e9377f6f6ed
Content-Type: application/x-www-form-urlencoded

tel=13637829200&password=123456
```
2. HTTP响应包结构     
  ![image](http://upload-images.jianshu.io/upload_images/680540-c07411436167d333.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
HTTP/1.1 200 OK
Date: Sat, 02 Jan 2016 13:20:55 GMT
Server: Apache/2.4.6 (CentOS) PHP/5.6.14
X-Powered-By: PHP/5.6.14
Content-Length: 78
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: application/json; charset=utf-8

{"status":202,"info":"\u6b64\u7528\u6237\u4e0d\u5b58\u5728\uff01","data":null}
```

---
55.在Java基本数据类型中，整数型的默认类型为int，小数型的默认类型为double。

---
56.补码相关
```
-n = ~n + 1;
/*
以10为例：
计算机中以补码存储。
正数的原码/反码/补码相同，所以
10存储为00000000 00000000 00000000 00001010  
~10的原码为11111111 11111111 11111111 11110101（10取反）
~10的反码为10000000 00000000 00000000 00001010（最高位符号位，不变，其余位取反）
~10的补码为10000000 00000000 00000000 00001011（负数的补码=反码+1)
所以~10 = -11
*/
```

---
57.描述在浏览器中敲入一个网址并按下回车后所发生的事情（尽量详细）    
答：浏览器输入网址之后，首先
步骤1：需要查找域名的IP地址，DNS查找过程如下：      
（1）浏览器缓存 – 浏览器的缓存DNS记录一段时间。 有趣的是，操作系统没有告诉浏览器储存DNS记录的时间，这样不同浏览器会储存各自固定的一个时间（2分钟到30分钟不等）。      
（2）系统缓存 – 如果在浏览器缓存里没有找到需要的记录，浏览器会做一个系统调用（windows里是gethostbyname）。这样便可获得系统缓存中的记录。      
（3）路由器缓存 – 接着，前面的查询请求发向路由器，它一般会有自己的DNS缓存。      
（4）ISP DNS 缓存 – 接下来要check的就是ISP缓存DNS的服务器。在这一般都能找到相应的缓存记录。      
（5）递归搜索 – 你的ISP的DNS服务器从跟域名服务器开始进行递归搜索，从.com顶级域名服务器到Facebook的域名服务器。一般DNS服务器的缓存中会有.com域名服务器中的域名，所以到顶级服务器的匹配过程不是那么必要了。
步骤2：浏览器给web服务器发送一个HTTP请求。请求中也包含浏览器存储的该域名的cookies。可能你已经知道，在不同页面请求当中，cookies是与跟踪一个网站状态相匹配的键值。这样cookies会存储登录用户名，服务器分配的密码和一些用户设置等。Cookies会以文本文档形式存储在客户机里，每次请求时发送给服务器。      
步骤3：服务的永久重定向响应      
步骤4：浏览器跟踪重定向地址      
步骤5：服务器“处理”请求      
步骤6：服务器发回一个HTML响应      
步骤7：浏览器开始显示HTML      
步骤8：浏览器发送获取嵌入在HTML中的对象      

---
58 [我终于搞清楚了和String有关的那点事儿](http://www.hollischuang.com/archives/2517)