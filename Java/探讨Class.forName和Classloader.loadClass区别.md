在探讨这两个区别前，先回顾下Java类加载过程，可分为五个阶段：加载，验证，准备，解析，初始化，如下图所示：

![类加载过程.png](http://ww1.sinaimg.cn/mw690/9e3e05f3gy1gdipv8ccm9j20w00astab.jpg)

- **加载**：该阶段会在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的入口
- **验证**：这一阶段的主要目的是为了确保Class文件的字节流中包含的信息是否符合当前虚拟机的要求，并且不会危害虚拟机自身的安全
- **准备**：准备阶段是正式为类变量分配内存并设置类变量的初始值阶段，即在方法区中分配这些变量所使用的内存空间
- **解析**：解析阶段是指虚拟机将常量池中的符号引用替换为直接引用的过程
- **初始化**：在准备阶段，已经给类变量分配了空间和初始化了默认值，而在初始化阶段，则按照源代码的语句的先后顺序去执行类变量的赋值语句和（或）静态语句块



类加载有以下三种方式：

1、命令行启动应用时候由JVM初始化加载

2、通过Class.forName()方法动态加载

3、通过ClassLoader.loadClass()方法动态加载

下面探讨方式2和方式3的区别

```java
package classloader;

/**
 * @author sugongchuang
 * @date 2020.04.05
 */
public class ClassloaderAndForNameTest {
	
	public static void main(String[] args){
		String dogPath = "classloader.Dog";
		String catPath = "classloader.Cat";
		testClassLoader(dogPath, catPath);
		System.out.println("------------------");
		testForName(dogPath, catPath);
		System.out.println("------------------");
		testForNameWithoutInit(dogPath, catPath);
	}
	
	/**
	 * 使用ClassLoader.loadClass()来加载类，不会执行初始化块
	 * @param dogPath
	 * @param catPath
	 */
	private static void testClassLoader(String dogPath, String catPath){
		try {
			Class<?> dog =  ClassLoader.getSystemClassLoader().loadClass(dogPath);
			Class cat =  ClassLoader.getSystemClassLoader().loadClass(catPath);
			System.out.println("dog:" + dog.getName());
			System.out.println("cat:" + cat.getName());
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 使用Class.forName()来加载类，默认会执行初始化块
	 * @param dogPath
	 * @param catPath
	 */
	private static void testForName(String dogPath, String catPath){
		try {
			Class<?> dog = Class.forName(dogPath);
			Class<?> cat = Class.forName(catPath);
			System.out.println("dog:" + dog.getName());
			System.out.println("cat:" + cat.getName());
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 使用Class.forName()来加载类，并指定ClassLoader，初始化时不执行静态块
	 * @param dogPath
	 * @param catPath
	 */
	private static void testForNameWithoutInit(String dogPath, String catPath){
		try {
			Class<?> dog = Class.forName(dogPath, false, ClassLoader.getSystemClassLoader());
			Class<?> cat = Class.forName(catPath, false, ClassLoader.getSystemClassLoader());
			System.out.println("dog:" + dog.getName());
			System.out.println("cat:" + cat.getName());
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	
}
/* 执行结果
dog:classloader.Dog
cat:classloader.Cat
------------------
静态代码块：Dog
静态代码块：Cat
dog:classloader.Dog
cat:classloader.Cat
------------------
dog:classloader.Dog
cat:classloader.Cat
 */
```

Dog.java

```java
public class Dog {
	
	static {
		System.out.println("静态代码块：Dog");
	}
	
}
```

Cat.java

```java
public class Cat {
	
	static {
		System.out.println("静态代码块：Cat");
	}
	
}
```

区别：

1、**Class.forName()**：将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块；Class.forName(name, initialize, loader)带参函数也可控制是否加载static块

2、**ClassLoader.loadClass()**：只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块

两种方式具体执行过程：

**ClassLoader.loadClass()方法加载类及初始化过程**：

```
1. 类加载loadClass（加载） —-> newInstance（连接+初始化）
2. newInstance()：（开始连接）静态代码块 --> 普通变量分配准备（a=0;b=0;c=null）  --> （开始初始化）普通变量赋值（a=1;b=2;c=”haha”）  --> 构造方法  --> 初始化成功
```

**Class.forName(String className)一个参数方法加载类及初始化过程**：

```
1. 类加载Class.forName（加载）—-> 静态代码块 —-> newInstance（连接+初始化）
2. newInstance()：（开始连接）普通变量分配准备（a=0;b=0;c=null） --> （开始初始化）普通变量赋值（a=1;b=2;c=”haha”） --> 构造方法 --> 初始化成功
```

> 1、Class.forName()三个参数的加载类及初始化过程同classLoader一样
>
> 2、静态代码块不是在初始化阶段完成的，它先于类初始化，先于普通变量默认分配（整型分配为0，字符串分配为null）

JVM的类加载及初始化过程基本如下：

```
1. 类加载：Bootstrap Loader——》Extended Loader——》System Loader   
2. 静态代码块初始化   
3. 链接：   
a) 验证：是否符合java规范   
b) 准备：默认初始值   
c) 解析：符号引用转为直接引用，解析地址   
4. 初始化   
a) 赋值：java代码中的初始值   
b) 构造:构造函数 
```

# 参考资料

- [Difference between Loading a class using ClassLoader and Class.forName](https://stackoverflow.com/questions/4285855/difference-between-loading-a-class-using-classloader-and-class-forname)
- [Get a load of that name](https://www.javaworld.com/article/2077332/get-a-load-of-that-name.html)
- [深入探讨 Java 类加载器](https://www.ibm.com/developerworks/cn/java/j-lo-classloader/index.html)
- [深入研究Java类加载机制](https://www.iteye.com/blog/zyjustin9-2092131)
- [Java虚拟机：JVM类加载机制](https://www.fangzhipeng.com/javainterview/2019/04/15/class-loader.html)