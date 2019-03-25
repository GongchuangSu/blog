# Bean Definition Inheritance
```xml
<bean id="helloWorld" class="com.tutorialspoint.HelloWorld">
  <property name="message1" value="Hello World!"/>
  <property name="message2" value="Hello Second World!"/>
</bean>

<bean id="helloIndia" class="com.tutorialspoint.HelloIndia" parent="helloWorld">
  <property name="message1" value="Hello India!"/>
  <property name="message3" value="Namaste India!"/>
</bean>
```
# Dependency Injection
## Constructor-based Dependency Injection
```xml
public TextEditor(SpellChecker spellChecker) {
  System.out.println("Inside TextEditor constructor." );
  this.spellChecker = spellChecker;
}
...
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <constructor-arg ref="spellChecker"/>
</bean>

<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```
## Setter-based Dependency Injection
```xml
// a setter method to inject the dependency.
public void setSpellChecker(SpellChecker spellChecker) {
  System.out.println("Inside setSpellChecker." );
  this.spellChecker = spellChecker;
}
...
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <property name="spellChecker" ref="spellChecker"/>
</bean>

<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```
# 注入内部Bean
```xml
<!-- Definition for textEditor bean using inner bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <property name="spellChecker">
     <bean class="com.tutorialspoint.SpellChecker"/>
   </property>
</bean>
```
内部Bean是通过直接声明一个<bean>元素作为<property>元素的子节点而定义。内部Bean并不限于setter注入，还可以把内部Bean装配到构造方法的入参中，如下列所示：
```xml
<!-- Definition for textEditor bean using inner bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <constructor-arg>
     <bean class="com.tutorialspoint.SpellChecker"/>
   </constructor-arg>
</bean>
```
> 注意：内部Bean没有ID属性，既不能通过名字来引用内部Bean。虽然为内部Bean配置一个ID属性完全合法，但没必要。内部Bean仅适用于一次注入，而且也不能被其它Bean所引用。

# 装配集合
| 元素       | 描述                                             |
| :--------- | :----------------------------------------------- |
| &lt;list>  | 装配list类型的值，允许重复                       |
| &lt;set>   | 装配set类型的值，不允许重复                      |
| &lt;map>   | 装配map类型的值，名称和值可以是任意类型          |
| &lt;props> | 装配properties类型的值，名称和值必须都是String型 |

```xml
<!-- Definition for javaCollection -->
<bean id="javaCollection" class="com.tutorialspoint.JavaCollection">

  <!-- results in a setAddressList(java.util.List) call -->
  <property name="addressList">
     <list>
        <value>INDIA</value>
        <value>Pakistan</value>
        <value>USA</value>
        <value>USA</value>
     </list>
  </property>

  <!-- results in a setAddressSet(java.util.Set) call -->
  <property name="addressSet">
     <set>
        <value>INDIA</value>
        <value>Pakistan</value>
        <value>USA</value>
        <value>USA</value>
    </set>
  </property>

  <!-- results in a setAddressMap(java.util.Map) call -->
  <property name="addressMap">
     <map>
        <entry key="1" value="INDIA"/>
        <entry key="2" value="Pakistan"/>
        <entry key="3" value="USA"/>
        <entry key="4" value="USA"/>
     </map>
  </property>
  
  <!-- results in a setAddressProp(java.util.Properties) call -->
  <property name="addressProp">
     <props>
        <prop key="one">INDIA</prop>
        <prop key="two">Pakistan</prop>
        <prop key="three">USA</prop>
        <prop key="four">USA</prop>
     </props>
  </property>

</bean>
```
## 装配Bean引用
```xml
<!-- Bean Definition to handle references and values -->
<bean id="..." class="...">

  <!-- Passing bean reference  for java.util.List -->
  <property name="addressList">
     <list>
        <ref bean="address1"/>
        <ref bean="address2"/>
        <value>Pakistan</value>
     </list>
  </property>
  
  <!-- Passing bean reference  for java.util.Set -->
  <property name="addressSet">
     <set>
        <ref bean="address1"/>
        <ref bean="address2"/>
        <value>Pakistan</value>
     </set>
  </property>
  
  <!-- Passing bean reference  for java.util.Map -->
  <property name="addressMap">
     <map>
        <entry key="one" value="INDIA"/>
        <entry key ="two" value-ref="address1"/>
        <entry key ="three" value-ref="address2"/>
     </map>
  </property>
  
</bean>
```
> map中的entry元素由一个键和一个值组成，键和值可以是简单类型，也可以是其它Bean的引用。

| 属性      | 用途                                             |
| :-------- | :----------------------------------------------- |
| key       | 指定map中entry的键为String或其它类型             |
| key-ref   | 指定map中entry的键为Spring上下文中其它Bean的引用 |
| value     | 指定map中entry的值为String或其它类型             |
| value-ref | 指定map中entry的值为Spring上下文中其它Bean的引用 |

## 装配空值
装配null值:
```xml
<bean id="..." class="exampleBean">
   <property name="email"><null/></property>
</bean>
```
装配空字符串值:
```xml
<bean id="..." class="exampleBean">
   <property name="email" value=""/>
</bean>
```
# 使用表达式装配Bean

----------
**字面值：**
在`<property>`元素的`value`属性中使用`#{}`界定符把这个值装配到Bean的属性中
```xml
<property name="count" value="#{5}"/>
...
<!--浮点型数字-->
<property name="frequence" value="#{89.7}"/>
<!--科学计数法-->
<property name="capacity" value="#{1e4}"/>
<!--布尔型-->
<property name="enable" value="#{false}"/>
```
`#{}`标记会提示Spring这个标记里的内容是SpEL表达式，它们还可以与非SpEL表达式的值混用：
```xml
<property name="count" value="The value is #{5}"/>
```
String类型的字面值可以使用单引号或双引号作为字符串的界定符：
```xml
<property name="name" value="#{'Chuck'}"/>
<!--或-->
<property name="name" value='#{"Chuck"}'/>
```
----------
**引用Bean、Properties和方法**
在SpEL表达式中使用Bean ID将一个Bean装配到另一个Bean的属性中：
```xml
<property name="instrument" value="#{saxophone}"/>
<!--其等价于-->
<property name="instrument" ref="saxophone"/>
```
在SpEL表达式中使用Bean的引用来获取Bean的属性：
```xml
<bean id="carl"
      class="com.springinaction.springidol.Instrumentalist">
	<property name="song" vaule="#{kenny.song}"/>
</bean>
```
其等价于以下代码
```java
Instrumentalist carl = new Instrumentalist();
carl.setSong(kenny.getSong());
```
在SpEL表达式中使用Bean的引用来调用Bean的方法，假设这里有一个songSelector Bean，该Bean有一个selectSong()方法：
```xml
<property name="song" value="#{songSelector.selectSong()}"/>
```
在SpEL表达式中使用null-safe存取器避免抛出空指针异常(NullPointerException)：
```xml
<!--没有使用null-safe存取器-->
<property name="song" value="#{songSelector.selectSong().toUpperCase()}"/>
<!--使用null-safe存取器-->
<property name="song" value="#{songSelector.selectSong()?.toUpperCase()}"/>
```
如果没有使用null-safe存取器，则selectSong()返回null值会导致空指针异常，而使用null-safe存取器，即使用`?.`运算符代替点`.`来访问toUpperCase()方法，在访问右方法之前，该运算符会确保左边项的值不会为null。如果为null，则SpEL不再尝试调用toUpperCase()方法。

----------
**操作类：**
在SpEL中，使用T()运算符会调用类作用域的方法和常量。
在SpEL表达式中使用T()运算符将指定类的静态常量装配到Bean的一个属性中：
```xml
<!--只需简单引用Math类的PI常量即可-->
<property name="multiplier" value="#{T(java.lang.Math).PI}"/>
```
使用T()运算符调用静态方法：
```xml
<!--将一个随机数（在0到1之间）装配到Bean的一个属性中-->
<property name="randomNumber" value="#{T(java.lang.Math)random()}"/>
```
# SpEL运算符
SpEL提供了几种运算符，这些运算符可以用在SpEL表达式中的值上。

| 运算符类型 | 运算符                               |
| :--------- | :----------------------------------- |
| 算术运算   | +、-、*、/、%、^                     |
| 关系运算   | <、>、==、<=、>=、lt、gt、eq、le、ge |
| 逻辑运算   | and、or、not、&#124;                 |
| 条件运算   | ?:(ternary)、?:(Elvis)               |
| 正则表达式 | matches                              |

----------
**使用SpEL进行数值运算**
```xml
<!-- +运算符：两个数字相加 -->
<property name="adjustedAmount" value="#{counter.total + 42}"/>
<!-- +运算符：用于连接字符串 -->
<property name="fullName" value="#{performer.firstName + ' ' + performer.lastName}"/>
<!-- -运算符：两个数字相减 -->
<property name="adjustedAmount" value="#{counter.total - 20}"/>
<!-- *运算符：乘法运算 -->
<property name="circumference" value="#{2 * T(java.lang.Math).PI * circle.radius}"/>
<!-- /运算符：除法运算 -->
<property name="average" value="#{counter.total / counter.count}"/>
<!-- %运算符：求余运算 -->
<property name="remainder" value="#{counter.total % counter.count}"/>
<!-- ^运算符：乘方运算 -->
<property name="area" value="#{T(java.lang.Math).PI * circle.radius ^ 2}"/>
```
> 注意：+运算符可以执行字符串连接。

----------
**比较值**
比较两个值是否相等，可以使用"=="运算符：
```xml
<!-- 假设equal属性为布尔属性 -->
<property name="equal" value="#{counter.total == 100}"/>
```
类似的，其他的关系运算符可以用于比较不同的值。
特别注意：由于小于等于和大于等于这两个符号在XML中有特殊意义，所以在Spring的XML配置文件中使用这两个符号时，会报错。当在XML中使用SpEL时，最好对这些运算符使用SpEL的文本替代方法。

|  运算符  | 符号 | 文本类型 |
| :------: | :--: | :------: |
|   等于   |  ==  |    eq    |
|   小于   |  <   |    lt    |
| 小于等于 |  <=  |    le    |
|   大于   |  >   |    gt    |
| 大于等于 |  >=  |    ge    |

----------
**逻辑表达式**
```xml
<!-- and 运算符 -->
<property name="largeCircle" value="#{shape.kind == 'circle' and shape.perimeter gt 10000}"/>
<!-- ! 运算符 -->
<property name="outOfStock" value="#{!product.availiable}"/>
<!-- not 运算符 -->
<property name="outOfStock" value="#{not product.availiable}"/>
```
| 运算符 | 操作                                                         |
| :----: | :----------------------------------------------------------- |
|  and   | 逻辑AND运算操作，只有运算符两边都是true，表达式才能是true    |
|   or   | 逻辑OR运算操作，只要运算符的任意一边是true，表达式就会是true |
| not或! | 逻辑NOT运算操作，对运算结果求反                              |

----------
**条件表达式**
```xml
<!-- ?:三元运算符 -->
<property name="song" value="#{kenny.song != null ? kenny.song : 'Greensleeves'}"/>
```
如果kenny.song值为空，则赋值kenny.song ，否则赋值'Greensleeves'。这里'Greensleeves'的引用重复两次，可简化表达式如下：
```xml
<!-- ?:三元运算符 -->
<property name="song" value="#{kenny.song != null ? 'Greensleeves'}"/>
```
当以这种方式使用时，“?:”通常被称为elvis运算符，而第一种方式则称为ternary运算符。

----------
**SpEL的正则表达式**
SpEL通过matches运算符来支持表达式中的模式匹配。
```xml
<!-- 判断一个字符串是否是有效的邮件地址 -->
<property name="validEmail" value="#{admin.email matches '[a-zA-Z0-9.-%+-]+@[a-zA-Z0-9.-]+\\.com'}"/>
```

# 自动装配Beans

> 创建应用对象之间的协作关系的行为通常被称为**装配(wiring)**,这也是依赖注入的本质。

## no
> This is default setting which means no autowiring and you should use explicit bean reference for wiring. You have nothing to do special for this wiring. This is what you already have seen in Dependency Injection chapter.

## byName
> 把与Bean的属性具有相同名字（或者ID）的其它Bean自动装配到Bean的对应属性中。如果没有跟属性的名字相匹配的Bean，则该属性不进行装配

Normal condition:
```xml
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <property name="spellChecker" ref="spellChecker" />
  <property name="name" value="Generic Text Editor" />
</bean>

<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```
Use autowiring 'byName':
```xml
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor" 
  autowire="byName">
  <property name="name" value="Generic Text Editor" />
</bean>

<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```
> 注意：如果Spring寻找到多个Bean，它们的类型与需要自动装配的属性的类型都相匹配，在这种场景下，Spring不会猜测哪一个Bean更适合自动装配，而是选择抛出异常。为了避免这种歧义，Spring提供了两种选择：为自动装配表示一个首选Bean或取消某个Bean自动装配的候选资格。
> - primary属性：默认为true。为了使用该属性，必须将其它非首先Bean的primary属性设置为false
> - autowire-candidate属性：当希望排除某些Bean时，可将这些Bean的autowire-candidate属性设置为false

## byType
> 把与Bean的属性具有相同类型的其他Bean自动装配到Bean的对应属性中。如果没有跟属性的类型相匹配的Bean，则该属性不被装配

Normal condition:
```xml
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <property name="spellChecker" ref="spellChecker" />
  <property name="name" value="Generic Text Editor" />
</bean>

<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```
Use autowiring 'byType':
```xml
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor" 
  autowire="byType">
  <property name="name" value="Generic Text Editor" />
</bean>

<!-- Definition for spellChecker bean -->
<bean class="com.tutorialspoint.SpellChecker">
</bean>
```
## by constructor
> 把与Bean的构造器入参具有相同类型的其它Bean自动装配到Bean构造器的对应入参中

Normal condition:
```xml
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <constructor-arg  ref="spellChecker" />
  <constructor-arg  value="Generic Text Editor"/>
</bean>

<!-- Definition for spellChecker bean -->
<bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```
Use autowiring 'by constructor':
```xml
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor" 
  autowire="constructor">
  <constructor-arg value="Generic Text Editor"/>
</bean>

<!-- Definition for spellChecker bean -->
<bean class="com.tutorialspoint.SpellChecker">
</bean>
```
## autodetect
> 首先尝试使用constructor进行自动装配。如果失败，再尝试使用byType进行自动匹配

## autowiring的使用限制
| 限制         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| 可能被覆盖   | 在使用autowiring的同时又使用了&lt;constructor-arg> 和 &lt;property> |
| 原始数据类型 | autowiring不能使用在原始数据类型上，如String等               |
| 容易混淆     | autowiring没有显示wiring那么明确，应尽量使用显示wiring       |

# Annotation Based Configuration
## @Required
`@Required`应用于`bean`属性的`setter`方法。它表明受影响的`bean`属性必须包含在`XML`配置文件中，否则会抛出`BeanInitializationException`异常。
```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">

   <context:annotation-config/>

   <!-- Definition for student bean -->
   <bean id="student" class="com.tutorialspoint.Student">
      <property name="name"  value="Zara" />
      <property name="age"  value="11"/>
   </bean>

</beans>
```
若将属性`age`去掉，即`<!-- property name="age"  value="11"-->`，则会抛出`Property 'age' is required for bean 'student'`。
## @Autowired
`@Autowired `注释对在哪里以及如何完成自动连接提供了更多的精细的控制。
### Setter方法中的@Autowired
可以在`Setter`方法上使用`@Autowired`以替代`XML`配置文件中的`<property>`元素。当`Spring`发现在`Setter`方法上使用了`@Autowired`，它将在该方法上执行`byType`自动连接。
**TextEditor.java**
```java
package com.tutorialspoint;

import org.springframework.beans.factory.annotation.Autowired;

public class TextEditor {
   private SpellChecker spellChecker;

   @Autowired
   public void setSpellChecker( SpellChecker spellChecker ){
      this.spellChecker = spellChecker;
   }
   public SpellChecker getSpellChecker( ) {
      return spellChecker;
   }
   public void spellCheck() {
      spellChecker.checkSpelling();
   }
}
```
**Beans.xml**
```xml
<context:annotation-config/>

<!-- Definition for textEditor bean without constructor-arg  -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
</bean>

<!-- Definition for spellChecker bean -->
<bean class="com.tutorialspoint.SpellChecker">
</bean>
```
### 属性中的@Autowired
可以在属性上使用`@Autowired`以替代`Setter`方法。
**TextEditor.java** 
```java
package com.tutorialspoint;

import org.springframework.beans.factory.annotation.Autowired;

public class TextEditor {
   @Autowired
   private SpellChecker spellChecker;

   public TextEditor() {
      System.out.println("Inside TextEditor constructor." );
   }
   
   public SpellChecker getSpellChecker( ){
      return spellChecker;
   }
   
   public void spellCheck(){
      spellChecker.checkSpelling();
   }
}
```
**Beans.xml**
```xml
<context:annotation-config/>

<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
</bean>

<!-- Definition for spellChecker bean -->
<bean class="com.tutorialspoint.SpellChecker">
</bean>
```

### 构造器中的@Autowired
可以在构造器上使用`@Autowired`以替代`<constructor-arg>`元素。
**TextEditor.java**
```java
package com.tutorialspoint;

import org.springframework.beans.factory.annotation.Autowired;

public class TextEditor {
   private SpellChecker spellChecker;

   @Autowired
   public TextEditor(SpellChecker spellChecker){
      System.out.println("Inside TextEditor constructor." );
      this.spellChecker = spellChecker;
   }

   public void spellCheck(){
      spellChecker.checkSpelling();
   }
}
```
**Beans.xml**
```xml
<context:annotation-config/>

<!-- Definition for textEditor bean without constructor-arg  -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
</bean>

<!-- Definition for spellChecker bean -->
<bean class="com.tutorialspoint.SpellChecker">
</bean>
```

### @Autowired的(required=false)选项
默认情况下，`@Autowired`注解意味着依赖是必须的，即它所依赖的属性必须包含在`XML`配置文件中。然而，可以通过`@Autowired`的`(required=false)`选项关闭该行为。同`@Required`注解类似。
**Student.java** 
```java
package com.tutorialspoint;

import org.springframework.beans.factory.annotation.Autowired;

public class Student {
   private Integer age;
   private String name;

   @Autowired(required=false)
   public void setAge(Integer age) {
      this.age = age;
   }
   
   public Integer getAge() {
      return age;
   }

   @Autowired
   public void setName(String name) {
      this.name = name;
   }
   
   public String getName() {
      return name;
   }
}
```
> 说明：`age`属性不需任何传递参数。即Spring将尝试装配age属性，如果没有查到与之匹配的类型为Integer的Bean，应用不会抛出NoSuchBeanDefinitionException，而age属性会设置为null。

> 注意：当使用构造器装配时，只有一个构造器可以将@Autowired的required属性设置为true，其它使用@Autowired注解所标注的构造器只能将required属性设置为false。此外，当使用@Autowired注解标注多个构造器时，Spring就会从所有满足装配条件的构造器中选择入参最多的那个构造器。

## @Qualifier
存在这样一种情形：当创建多个同一类型的bean时，在依赖时又只想依赖其中的某一个时该怎么做呢？
我们可以将`@Qualifier`注解和`@Autowired`注解一起使用来确定到底依赖哪一个bean。
**Profile.java**
```java
package com.tutorialspoint;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class Profile {
   @Autowired
   @Qualifier("student1")
   private Student student;

   public Profile(){
      System.out.println("Inside Profile constructor." );
   }

   public void printAge() {
      System.out.println("Age : " + student.getAge() );
   }

   public void printName() {
      System.out.println("Name : " + student.getName() );
   }
}
```
**Beans.xml**
```xml
<context:annotation-config/>

<!-- Definition for profile bean -->
<bean id="profile" class="com.tutorialspoint.Profile">
</bean>

<!-- Definition for student1 bean -->
<bean id="student1" class="com.tutorialspoint.Student">
  <property name="name"  value="Zara" />
  <property name="age"  value="11"/>
</bean>

<!-- Definition for student2 bean -->
<bean id="student2" class="com.tutorialspoint.Student">
  <property name="name"  value="Nuha" />
  <property name="age"  value="2"/>
</bean>
```

# 相比EJB
Spring把EJB组件还原成POJO或JavaBean对象，降低了应用开发对传统J2EE技术规范的依赖。

# Spring IoC容器
![](http://i.imgur.com/SxcIt3V.png)
## BeanFactory接口
Bean 工厂是最简单的容器，提供基本的DI支持。
**源代码：**
```java
public interface BeanFactory {
	String FACTORY_BEAN_PREFIX = "&";
	Object getBean(String name) throws BeansException;
	<T> T getBean(String name, Class<T> requiredType) throws BeansException;
	<T> T getBean(Class<T> requiredType) throws BeansException;
	Object getBean(String name, Object... args) throws BeansException;
	<T> T getBean(Class<T> requiredType, Object... args) throws BeansException;
	boolean containsBean(String name);
	boolean isSingleton(String name) throws NoSuchBeanDefinitionException;
	boolean isPrototype(String name) throws NoSuchBeanDefinitionException;
	boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;
	boolean isTypeMatch(String name, Class<?> typeToMatch) throws NoSuchBeanDefinitionException;
	Class<?> getType(String name) throws NoSuchBeanDefinitionException;
	String[] getAliases(String name);
}
```
## ApplicationContext接口
应用上下文基于BeanFactory之上创建，提供面向应用的服务。
**源代码：**
```java
package org.springframework.context;

public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
		MessageSource, ApplicationEventPublisher, ResourcePatternResolver {
	String getId();
	String getApplicationName();
	String getDisplayName();
	long getStartupDate();
	ApplicationContext getParent();
	AutowireCapableBeanFactory getAutowireCapableBeanFactory() throws IllegalStateException;
}
```
说明：
ApplicationContext在BeanFactory的基础上添加了一些以下新特性
 - 支持不同的信息源。支持国际化的实现，为开发多语言版本的应用提供服务
 - 访问资源。可以从不同地方得到Bean定义资源
 - 支持应用事件。在上下文中引入了事件机制

Spring自带了几种类型的应用上下文，下面列出最常见的三种：
- ClassPathXmlApplicationContext：从类路径下的XML配置文件中加载上下文定义，把应用上下文定义文件当作类资源
- FileSystemXmlApplicationContext：读取文件系统下的XML配置文件并加载上下文定义
- XmlWebApplicationContext：读取Web应用下的XML配置文件并装载上下文定义

## XmlBeanFactory类
**源代码：**
```java
package org.springframework.beans.factory.xml;

@Deprecated
@SuppressWarnings({"serial", "all"})
public class XmlBeanFactory extends DefaultListableBeanFactory {
	private final XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(this);
	public XmlBeanFactory(Resource resource) throws BeansException {
		this(resource, null);
	}
	public XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory) throws BeansException {
		super(parentBeanFactory);
		this.reader.loadBeanDefinitions(resource);
	}
}
```
说明：
1. `Resource`是Spring用来封装I/O操作的类
2. 可通过`ClassPathResource res = new ClassPathResource(bean.xml)`来构造需要的`Resource`，这里BeanDefinition信息是以XML文件形式存在
3. `Resource`对象传递给`XmlBeanFactory`构造器后，通过`XmlBeanDefinitionReader`对象的`loadBeanDefinitions`方法进行解析载入至`XmlBeanFactory`

## IoC容器的初始化
IoC容器的初始化是由`refresh()`方法来启动的，它标志着IoC容器的正式启动。
具体来说，这个启动包括`BeanDefinition`的`Resource`定位、载入和注册三个基本过程：
1. `Resource`定位过程。它指的是BeanDefinition的资源定位，由ResourceLoader通过统一的Resource接口完成。如在文件系统中使用的FileSystemResource，在类路径中使用的ClassPathResource
2. `BeanDefinition`的载入。这个载入过程是把用户定义好的Bean表示成IoC容器内部的数据结构，即`BeanDefinition`。`BeanDefinition`实际上就是POJO对象在IoC容器中的抽象
3. 向IoC容器注册`BeanDefinition`。调用BeanDefinitionRegistry接口的实现来完成，在IoC容器内部将`BeanDefinition`注入到一个HashMap中，IoC容器通过这个HashMap来持有这些`BeanDefinition`数据

> 注意：IoC容器的这个初始化过程，一般不包含Bean依赖注入的实现。在Spring IoC的设计中，Bean定义的载入和依赖注入是两个独立的过程。但可通过设置`lazyinit`属性改变依赖注入的时期。

----------

# 《Java实战(第三版)》
SpringMVC的工作原理如下图所示：
![](http://i.imgur.com/P0Tneoy.png)
1. 客户端的所有请求都交给前端控制器DispatcherServlet来处理，它会负责调用系统的其他模块来真正处理用户的请求。
2. DispatcherServlet收到请求后，将根据请求的信息（包括URL、HTTP协议方法、请求头、请求参数、Cookie等）以及HandlerMapping的配置找到处理该请求的Handler（任何一个对象都可以作为请求的Handler）。当然，在这个地方Spring会通过HandlerAdapter对该处理器进行封装，HandlerAdapter是一个适配器，它用统一的接口对各种Handler中的方法进行调用。
3. Handler完成对用户请求的处理后，会返回一个ModelAndView对象给DispatcherServlet，ModelAndView顾名思义，包含了数据模型以及相应的视图的信息。当然，这里的视图是逻辑视图，DispatcherServlet还要借助ViewResolver完成从逻辑视图到真实视图对象的解析工作。
4. 当得到真正的视图对象后，Dispatcher会利用视图对象对模型数据进行渲染。
5. 客户端得到响应，可能是一个普通的HTML页面，也可以是XML或JSON字符串，还可以是一张图片或者一个PDF文件。

为了降低Java开发的复杂性，Spring采取以下4种关键策略：
- 基于POJO的轻量级和最小侵入性编程
- 通过依赖注入和面向切面实现松耦合
- 基于切面和惯例进行声明式编程
- 通过切面和模板减少样板式代码

----------
依赖注入(DI)有助于应用对象之间的解耦，而AOP可以实现横切关注点与它们所影响的对象之间的解耦。

----------

**Spring核心自带了10个命名空间配置：**

| 命名空间 | 用途                                                         |
| :------: | :----------------------------------------------------------- |
|   aop    | 为声明切面以及将@AspectJ注解的类代理为Spring切面提供了配置元素 |
|  beans   | 支持声明Bean和装配Bean，是Spring最核心也是最原始的命名空间   |
| context  | 为配置Spring应用上下文提供了配置元素，包括自动检测和自动装配Bean、注入非Spring直接管理的对象 |
|   jee    | 提供了与Java EE API的集成，例如JNDI和EJB                     |
|   jms    | 为声明消息驱动的POJO提供了配置元素                           |
|   lang   | 支持配置由Groovy、JRuby或BeanShell等脚本实现的Bean           |
|   mvc    | 启用Spring MVC的能力，例如面向注解的控制器、视图控制器和拦截器 |
|   oxm    | 支持Spring的对象到XML映射配置                                |
|    tx    | 提供声明式事务配置                                           |
|   util   | 提供各种各样的工具类元素，包括把集合配置为Bean、支持属性占位符元素 |

----------

通过工厂方法创建Bean，使用`factory-method`将单例类配置为Bean。`factory-method`属性允许调用一个指定的静态方法，代替构造方法来创建一个类的实例。
```xml
<bean id="theStage" 
      class="com.springinaction.springidol.Stage"
      factory-method="getInstance"/>
```

----------

**Bean作用域：**

|    作用域     | 定义                                                         |
| :-----------: | :----------------------------------------------------------- |
|   singleton   | 单例模式，在整个Spring IoC容器中，使用singleton定义的Bean将只有一个实例(默认) |
|   prototype   | 原型模式，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例 |
|    request    | 对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效 |
|    session    | 对于每次HTTP Session，使用session定义的Bean都将产生一个新实例。同样只有在Web应用中使用Spring时，该作用域才有效 |
| globalSession | 每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。典型情况下，仅在使用portlet context的时候有效。同样只有在Web应用中使用Spring时，该作用域才有效 |
|  application  | 对于每个ServletContext，使用application定义的Bean都将产生一个新实例。只有在Web应用中使用Spring时，该作用域才有效 |
|   websocket   | 对于每个WebSocket，使用websocket定义的Bean都将产生一个新实例。只有在Web应用中使用Spring时，该作用域才有效 |

----------

**初始化和销毁Bean：**

| 属性                   | 功能                                       |
| :--------------------- | :----------------------------------------- |
| init-method            | 指定了在初始化一个Bean时要调用的方法       |
| destroy-method         | 指定了一个Bean从容器移除之前要调用的方法   |
| default-init-method    | 为应用上下文中所有的Bean设置共同的初始方法 |
| default-destroy-method | 为应用上下文中所有的Bean设置共同的销毁方法 |

----------

**使用Spring的命名空间p装配属性**
命名空间p的schema URL为http://www.springframework.org/schema/p。如果想使用命名空间p，只需在Spring的XML配置中添加如下一段声明：
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:p="http://www.springframework.org/schema/p"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="
   http://www.springframework.org/schema/beans     
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/context 
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">
```
配置后我们可以使用p:作为<bean>元素所有属性的前缀来装配Bean属性。
```xml
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <p:message="Hello India!"/>
  <p:editor-ref="xxx"/>
</bean>
```
它与下面这段代码等价，-ref后缀作为一个标志来告知Spring应该装配一个引用而不是一个字面值。
```xml
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
  <property name="message" value="Hello India!"/>
  <property name="editor" ref="xxx"/>
</bean>
```

## 面向切面的Spring
**通知(Advice)**
通知定义了切面是什么以及何时使用。
Spring切面可以应用5种类型的通知：
- Before：在方法被调用之前调用通知
- After：在方法完成之后调用通知，无论方法执行是否成功
- After-returning：在方法成功执行之后调用通知
- After-throwing：在方法抛出异常后调用通知
- Around：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为

----------
**连接点(Joinpoint)**
连接点是应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。

----------
**切点(Poincut)**
如果通知定义了切面的“什么”和“何时”，那么切点就定义了“何处”。通常使用明确的类或方法名称来指定这些切点，或是利用正则表达式定义匹配的类和方法名称模式来指定这些切点。

----------
**切面(Aspect)**
切面是通知和切点的结合。通知和切点共同定义了关于切面的全部内容（它是什么，在何时何何处完成其功能）。

----------
**引入(Introduction)**
引入允许我们向现有的类添加新方法或属性。

----------
**织入(Weaving)**
织入是将切面应用到目标对象来创建新的代理对象的过程。切面在指定的连接点被织入到目标对象中。

----------
**几种AOP框架**
- AspectJ
- JBoss AOP
- Spring AOP

Spring提供四种各具特色的AOP支持
- 基于代理的经典AOP
- @AspectJ注解驱动的切面
- 纯POJO切面
- 注入式AspectJ切面

----------
Spring借助AspectJ的切点表达式语言来定义Spring切面，下面列出了Spring AOP所支持的AspectJ切点指示器：

| AspectJ指示器 | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| arg()         | 限制连接点匹配参数为指定类型的执行方法                       |
| @args()       | 限制连接点匹配参数由指定注解标注的执行方法                   |
| execution()   | 用于匹配是连接点的执行方法                                   |
| this()        | 限制连接点匹配AOP代理的Bean引用为指定类型的类                |
| target()      | 限制连接点匹配目标对象为指定类型的类                         |
| @target()     | 限制连接点匹配特定的执行对象，这些对象对应的类要具备指定类型的注解 |
| within()      | 限制连接点匹配指定的类型                                     |
| @within()     | 限制连接点匹配指定注解所标注的类型（当使用Spring AOP时，方法定义在由指定的注解所标注的类里） |
| @annotation   | 限制匹配带有指定注解连接点                                   |

---
# 补充（2017.06.06）
## 类的自动检测及Bean的注册
1、为了能够检测类并注册相应的Bean，需在配置文件中添加以下代码：
```
<context:component-scan base-package="com.gongchuangsu.ssh" />
```
> 说明：`<context:component-scan>`包含`<context:annotation-config>`，通常在使用前者后，不用再使用后者