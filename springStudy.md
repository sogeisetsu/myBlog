[TOC]

# 1. Why Spring?

> Spring makes programming Java quicker, easier, and safer for everybody. Spring’s focus on speed, simplicity, and productivity has made it the [world's most popular](https://snyk.io/blog/jvm-ecosystem-report-2018-platform-application/) Java framework.

## 1.1 作者

> 第一版由 [Rod Johnson](https://zh.wikipedia.org/w/index.php?title=Rod_Johnson&action=edit&redlink=1) 开发，并在2002年10月发布在 *Expert One-on-One J2EE Design and Development* 一书中。2003年6月，Spring Framework 第一次发布在 [Apache 2.0 许可证](https://zh.wikipedia.org/wiki/Apache许可证)下。2004年3月，发布了里程碑的版本1.0，2004年9月以及2005年3月，又发布了新的里程碑版本。2006年，Spring Framework 获得了 [Jolt 生产力奖](https://zh.wikipedia.org/w/index.php?title=Jolt_Awards&action=edit&redlink=1) 和 [JAX 创新奖](https://zh.wikipedia.org/w/index.php?title=JAX_创新奖&action=edit&redlink=1)。
>
> Spring Framework创始人，著名作者。 Rod在[悉尼大学](https://baike.baidu.com/item/悉尼大学/1700805)不仅获得了计算机学位，同时还获得了音乐学位。更令人吃惊的是在回到软件开发领域之前，他还获得了**音乐学的博士学位**。 有着相当丰富的C/[C++](https://baike.baidu.com/item/C%2B%2B)技术背景的Rod早在1996年就开始了对Java服务器端技术的研究。他是一个在保险、电子商务和金融行业有着丰富经验的技术顾问，同时也是[JSR](https://baike.baidu.com/item/JSR)-154（[Servlet](https://baike.baidu.com/item/Servlet)2.4）和[JDO](https://baike.baidu.com/item/JDO)2.0的规范专家、[JCP](https://baike.baidu.com/item/JCP)的积极成员，是Java development community中的杰出人物。

## 1.2 导包和下载

官网地址 https://spring.io/

官网repo下载地址 https://repo.spring.io/release/org/springframework/spring/

GitHub 地址 https://github.com/spring-projects/spring-framework/releases

maven导包👇

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>

```

## 1.3 spring 优点

> - 简化开发，解耦，集成其它框架。
> - 低侵入式设计，代码污染级别级别。
> - Spring的DI机制降低了业务对象替换的复杂性，提高了软件之间的解耦。
> - Spring AOP支持将一些通用的任务进行集中式的管理，例如：安全，事务，日志等，从而使代码能更好的复用。
> - **控制反转 （IOC）面向切面编程（AOP）**

spring是轻量级的**控制反转 （IOC）面向切面编程（AOP）**框架

## 1.4 spring 组成

![](https://www.ibm.com/developerworks/cn/java/wa-spring1/spring_framework.gif)



> 组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：
>
> - **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 `BeanFactory`，它是工厂模式的实现。`BeanFactory` 使用*控制反转* （IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
> - **Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
> - **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。
> - **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
> - **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
> - **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
> - **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。
>
> Spring 框架的功能可以用在任何 J2EE 服务器中，大多数功能也适用于不受管理的环境。Spring 的核心要点是：支持不绑定到特定 J2EE 服务的可重用业务和数据访问对象。毫无疑问，这样的对象可以在不同 J2EE 环境 （Web 或 EJB）、独立应用程序、测试环境之间重用。

<img src="http://q9efxddri.bkt.clouddn.com/20200429140132.png"/>

# 2. IOC理论

> IoC 全称为 `InversionofControl`，翻译为 “控制反转”，它还有一个别名为 DI（ `DependencyInjection`）,即依赖注入。

https://www.zhihu.com/question/23277575

<img src="http://q9efxddri.bkt.clouddn.com/20200429143203.png"/>

https://blog.csdn.net/weixin_44823472/article/details/97787171

## 2.1 IOC本质

> 控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。
>
> ------------------------------------------------
> 版权声明：本文为CSDN博主「你的笑容灿烂了这个夏天」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/weixin_44823472/article/details/97787171

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcyMDE4LmNuYmxvZ3MuY29tL2Jsb2cvMTQxODk3NC8yMDE5MDcvMTQxODk3NC0yMDE5MDcyNjExMjgzNzA1NC02MDE2NzcxNTgucG5n)

# 3. Hello Spring

**面试题 IOC是什么？**

> 一、IOC是什么？
> IOC全称为“Inversion of Control”，即控制反转，不是一种技术，而是一种设计思想。在这种设计思想中，你设计好的对象交给容器管理，而不是在应用程序内部对对象进行管理。控制的含义是IOC容器控制了对象（也可以包括文件及其他外部资源）；**而反转的含义是IOC容器负责创建及注入依赖的对象**，但在传统的应用程序中，我们需要在对象内部去创建（new）依赖的对象，这叫“正”，在这样的情况下，对象之间的耦合度就非常高。IOC更像是一种中介，帮助雇佣者和被雇佣者。我觉得支付工具如支付宝就像是淘宝体系中的一个IOC。
>
> 
>
> 到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !
>
> 

Beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    
    <bean id="user" class="org.suyuesheng.spring.pojo.User">
        <property name="id" value="1"/>
        <property name="name" value="0"/>
        <property name="pwd" value="0"/>
    </bean>
    
    <!--    id 是生成的对象名称  class是类-->
    <!--    name是类的属性名 value是类的属性的内容-->
    <!--    ref元素引用另一个 bean 定义的 name。-->
    <bean id="hello" class="org.suyuesheng.spring.pojo.Hello">
        <property name="str" value="Hello"/>
<!--        ref 指的是beans.xml里面曾经定义过这个类-->
        <property name="user" ref="user"/>
    </bean>
    
</beans>
```

java代码👇

```java
package org.suyuesheng.spring.pojo;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestHello {
    @Test
    public void testHello(){
        /*
        * `ApplicationContext`是高级工厂的接口，
        * 能够维护不同 beans 及其依赖项的注册表。
        * 使用方法`T getBean(String name, Class requiredType)`，
        * 您可以检索 beans 的实例。*/
        //获取容器
        ApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("Beans.xml");
//        Hello hello = (Hello)classPathXmlApplicationContext.getBean("hello");
        //获取对象
        Hello hello = classPathXmlApplicationContext.getBean("hello", Hello.class);
        System.out.println(hello.getStr());
    }
}

```

`ApplicationContext`是高级工厂的接口，能够维护不同 beans 及其依赖项的注册表。使用方法`T getBean(String name, Class requiredType)`，您可以检索 beans 的实例。👆

`new ClassPathXmlApplicationContext(容器名称)`来获取spring容器

**注意，要想使用bean中的`property`标签，必须在类里声明set方法。**

**获取对象，默认获取的是无参构造对象**

# 4. IOC创建对象的方式

https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#spring-core

## 4.1 官方文档中关于spring创造有参构造对象的解释👇

> **Constructor argument type matching**
>
> In the preceding scenario, the container can use type matching with simple types if you explicitly specify the type of the constructor argument by using the `type` attribute. as the following example shows:
>
> ```xml
> <bean id="exampleBean" class="examples.ExampleBean">
>     <constructor-arg type="int" value="7500000"/>
>     <constructor-arg type="java.lang.String" value="42"/>
> </bean>
> ```
>
> **Constructor argument index**
>
> You can use the `index` attribute to specify explicitly the index of constructor arguments, as the following example shows:
>
> ```xml
> <bean id="exampleBean" class="examples.ExampleBean">
>     <constructor-arg index="0" value="7500000"/>
>     <constructor-arg index="1" value="42"/>
> </bean>
> ```
>
> In addition to resolving the ambiguity of multiple simple values, specifying an index resolves ambiguity where a constructor has two arguments of the same type.
>
> *The index is 0-based.*
>
> **Constructor argument name**
>
> You can also use the constructor parameter name for value disambiguation, as the following example shows:
>
> ```xml
> <bean id="exampleBean" class="examples.ExampleBean">
>     <constructor-arg name="years" value="7500000"/>
>     <constructor-arg name="ultimateAnswer" value="42"/>
> </bean>
> 
> ```

## 4.2 IOC容器创建有参和无参构造对象

1. 默认创建无参构造对象，**默认！！！**

2. 创建有参构造的三种方式

   1. 下标

      Beans.xml里面这样配置👇

      ```xml
      <bean id="user" class="org.suyuesheng.spring.pojo.User">
          <constructor-arg index="0" value="12"/>
          <constructor-arg index="1" value="老张"/>
          <constructor-arg index="2" value="1233"/>
      </bean>
      ```

   2. 类型

      **相当不建议这种，原因是如果参数里面存在相同类型的话，用这个就会比较麻烦**

      ```xml
      <bean id="exampleBean" class="examples.ExampleBean">
          <constructor-arg type="int" value="7500000"/>
          <constructor-arg type="java.lang.String" value="42"/>
      </bean>
      ```

   3. **名称**

      最建议这种👇

      ```xml
      <bean id="user" class="org.suyuesheng.spring.pojo.User">
          <constructor-arg name="id" value="134"/>
          <constructor-arg name="name" value="老刘"/>
          <constructor-arg name="pwd" value="12324"/>
      </bean>
      ```

      

   















