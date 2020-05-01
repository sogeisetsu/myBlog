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

[跳过该图👇](#IOC本质)

<img src="http://q9efxddri.bkt.clouddn.com/20200429143203.png"/>

https://blog.csdn.net/weixin_44823472/article/details/97787171

## 2.1 <a name="IOC本质">IOC本质</a>

> 控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。
>
> ------------------------------------------------
> 版权声明：本文为CSDN博主「你的笑容灿烂了这个夏天」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/weixin_44823472/article/details/97787171

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcyMDE4LmNuYmxvZ3MuY29tL2Jsb2cvMTQxODk3NC8yMDE5MDcvMTQxODk3NC0yMDE5MDcyNjExMjgzNzA1NC02MDE2NzcxNTgucG5n)

# 3. Hello Spring

## 3.1 需要导入的包

<img src="http://q9efxddri.bkt.clouddn.com/20200501161255.png"/>

## 3.2 **面试题 IOC是什么？**

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

2. <a name="创建有参构造的三种方式">创建有参构造的三种方式</a>

   **通过有参构造器创建对象(通过构造器进行依赖注入)无须setter方法**

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

# 5 spring 配置

## 5.1 别名

`alias` 给对象的名称取一个别名，可以用别名来生成对象

```xml
 <!--    id 是生成的对象名称  class是类-->
    <!--    name是类的属性名 value是类的属性的内容-->
    <!--    ref元素引用另一个 bean 定义的 name。-->
    <bean id="hello" class="org.suyuesheng.spring.pojo.Hello">
        <property name="str" value="Hello"/>
<!--        ref 指的是beans.xml里面曾经定义过这个类-->
        <property name="user" ref="user"/>
    </bean>

    <alias name="hello" alias="hello2"/>
```

用别名来获取对象👇

```java
@Test
public void testAlias(){
    ApplicationContext applicationContext= new ClassPathXmlApplicationContext("Beans.xml");
    Hello hello = applicationContext.getBean("hello", Hello.class);
    //通过别名
    Hello hello2 = applicationContext.getBean("hello2", Hello.class);

    System.out.println("hello\n"+hello); //Hello{str='Hello', user=User{id=134, name='老刘', pwd='12324'}}
    System.out.println("hello2\n"+hello2); //Hello{str='Hello', user=User{id=134, name='老刘', pwd='12324'}}
}
```

**取别名的另外一种方式，在`bean`标签的属性name里取，而且还能取好多个👇**  推荐！！！

```xml
<bean id="hello" class="org.suyuesheng.spring.pojo.Hello" name="hello3,hello4,hello5">
    <property name="str" value="Hello"/>
    <!--        ref 指的是beans.xml里面曾经定义过这个类-->
    <property name="user" ref="user"/>
</bean>
```

## 5.2 `bean`的配置

```xml
<!--    id 是生成的对象名称  class是类-->
<!--    name是类的属性名 value是类的属性的内容-->
<!--    ref元素引用另一个 bean 定义的 name。-->
<!--    bean标签的name指定别名-->
<bean id="hello" class="org.suyuesheng.spring.pojo.Hello" name="hello3,hello4,hello5">
    <property name="str" value="Hello"/>
    <!--        ref 指的是beans.xml里面曾经定义过这个类-->
    <property name="user" ref="user"/>
</bean>
```

## 5.3 `import`

引入别的文件，那么获取对象的时候就可以从一个bean.xml那里生成spring容器了，这个容器也可以生成被导入的文件的bean

```xml
<import resource="beans2.xml"/>
```



# 6 DI依赖注入

> 依赖注入(DI)是一个 process，其中 objects 仅通过构造函数 arguments，工厂方法的 arguments 或 object 实例在构造之后设置的 properties 定义它们的依赖项(即，它们工作的其他 objects)或者从工厂方法返回。然后容器在创建 bean 时注入这些依赖项。这个 process 基本上是 bean 本身的逆(因此 name，控制反转)，它通过使用 classes 或 Service Locator pattern 的直接构造来控制其依赖项的实例化或位置。
>
> 来源 https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#expressions

https://juejin.im/post/5aa89d076fb9a028c06a846c

 ## 6.1 构造器注入

请看 4.2.2   创建有参构造的三种方式 [跳转到4.2.2](#创建有参构造的三种方式)

## 6.2 Setter注入[重点]

**依赖注入**

	- 依赖  bean对象的创建依赖容器
	- 注入 bean对象的所有属性由spring容器(IOC容器)注入

```xml
<bean id="student" class="org.suyuesheng.spring.s03.pojo.Stusent">
    <property name="age" value="19"/>
</bean>
```



**环境搭建**

- 复杂类型 address

```java
package org.suyuesheng.spring.s03.pojo;

public class Address {
    private double xRay;
    private double yRay;

    public Address() {
    }

    public Address(double xRay, double yRay) {
        this.xRay = xRay;
        this.yRay = yRay;
    }

    public double getxRay() {
        return xRay;
    }

    public void setxRay(double xRay) {
        this.xRay = xRay;
    }

    public double getyRay() {
        return yRay;
    }

    public void setyRay(double yRay) {
        this.yRay = yRay;
    }

    @Override
    public String toString() {
        return "Address{" +
                "xRay=" + xRay +
                ", yRay=" + yRay +
                '}';
    }
}

```

- 对象 student

```java
package org.suyuesheng.spring.s03.pojo;

import java.util.*;

public class Stusent {
    private int age;
    private Address address;
    private Map<String,String> studentMap;
    private List<String> stringList;
    private String[] strings;
    private Properties properties;
    private Set<String> stringSet;

    public Stusent() {
    }

    public Stusent(int age, Address address, Map<String, String> studentMap, List<String> stringList, String[] strings, Properties properties, Set<String> stringSet) {
        this.age = age;
        this.address = address;
        this.studentMap = studentMap;
        this.stringList = stringList;
        this.strings = strings;
        this.properties = properties;
        this.stringSet = stringSet;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public Map<String, String> getStudentMap() {
        return studentMap;
    }

    public void setStudentMap(Map<String, String> studentMap) {
        this.studentMap = studentMap;
    }

    public List<String> getStringList() {
        return stringList;
    }

    public void setStringList(List<String> stringList) {
        this.stringList = stringList;
    }

    public String[] getStrings() {
        return strings;
    }

    public void setStrings(String[] strings) {
        this.strings = strings;
    }

    public Properties getProperties() {
        return properties;
    }

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    public Set<String> getStringSet() {
        return stringSet;
    }

    public void setStringSet(Set<String> stringSet) {
        this.stringSet = stringSet;
    }

    @Override
    public String toString() {
        return "Stusent{" +
                "age=" + age +
                ", address=" + address +
                ", studentMap=" + studentMap +
                ", stringList=" + stringList +
                ", strings=" + Arrays.toString(strings) +
                ", properties=" + properties +
                ", stringSet=" + stringSet +
                '}';
    }
}

```

- 测试类 TestPojo

```java
package org.suyuesheng.spring.so3.pojo;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.suyuesheng.spring.s03.pojo.Stusent;

public class TestPojo {
    @Test
    public void testAdress() {
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        Stusent student = context.getBean("student", Stusent.class);
        System.out.println(student);
    }
}

```

### beans.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="org.suyuesheng.spring.s03.pojo.Stusent">
<!--        普通值注入-->
        <property name="age" value="19"/>
<!--        bean注入-->
        <property name="address" ref="address"/>
<!--        数组注入-->
        <property name="strings">
            <array>
                <value>你好</value>
                <value>haha</value>
                <value>呵呵呵</value>
                <value>或会后</value>
            </array>
        </property>
<!--        list注入-->
        <property name="stringList">
            <list>
                <value>hello</value>
                <value>nininini</value>
            </list>
        </property>
<!--        map注入-->
        <property name="studentMap">
            <map>
                <entry key="name" value="XioaMiHen"/>
                <entry key="age" value="14"/>
                <entry key="lastName" value="Xiao"/>
<!--                为home指定的值为null-->
                <entry key="home">
                    <null/>
                </entry>
            </map>
        </property>
<!--        set注入-->
        <property name="stringSet">
            <set>
                <value>hhset</value>
                <value>llset</value>
            </set>
        </property>
<!--        properties注入-->
        <property name="properties">
            <props>
                <prop key="adamin">root</prop>
                <prop key="pwd">dwef</prop>
            </props>
        </property>

    </bean>

    <bean id="address" class="org.suyuesheng.spring.s03.pojo.Address">
        <property name="xRay" value="12.5"/>
        <property name="yRay" value="14.6"/>
    </bean>
</beans>
```

## 6.3 p 命名注入和 c命名注入

***并不适用于集合参数.`list | set | map | props`等*** 

> 在通过构造方法或`set`方法给`bean`注入关联项时通常是通过`constructor-arg`元素和`property`元素来定义的。在有了`p`命名空间和`c`命名空间时我们可以简单的把它们当做`bean`的一个属性来进行定义。

p命名注入需要在**配置文件头部加上**`xmlns:p="http://www.springframework.org/schema/p"`

c命名注入需要在**配置文件头部加上**`xmlns:c="http://www.springframework.org/schema/c"`

- p命名注入通过属性setter
- c命名注入，通过有参构造，**要想使用c命名构造，必须要有有参构造函数**
- `c\p:属性名称-ref` 来为属性指定bean,如`c:address-ref="address"`

配置👇

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="Beans.xml"/>
<!--    p命名注入通过属性setter-->
    <bean id="user" class="org.suyuesheng.spring.s03.pojo.User" p:age="12" p:name="老刘" />
<!--    c命名注入，通过有参构造-->
    <bean id="userc" class="org.suyuesheng.spring.s03.pojo.UserC" c:age="14" c:name="kk" c:address-ref="address"/>
</beans>
```

测试👇

```java
@Test
public void testPNamespace() {
    ApplicationContext context = new ClassPathXmlApplicationContext("UserBeans.xml");
    User user = context.getBean("user", User.class);
    System.out.println(user);
}
@Test
public void testCNameSpace(){
    ApplicationContext context = new ClassPathXmlApplicationContext("UserBeans.xml");
    UserC user = context.getBean("userc", UserC.class);
    System.out.println(user);
}
```

# 7 <a name=Bean的作用域>Bean的作用域</a>

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance for each Spring IoC container. |
| [prototype](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

## 7.1 配置方法

bean里面的一个属性 `<bean id="user" class="org.suyuesheng.spring.s03.pojo.User" p:age="12" p:name="老刘" scope="singleton"/>`

## 7.2 单例模式和原型模式

- [singleton](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-singleton) 单例模式，类似构造函数私有化，生成的对象为同一个，默认就是单例模式

  ```java
  /**
  * 测试单例模式和多例模式
  */
  @Test
  public void testDD(){
      ApplicationContext context = new ClassPathXmlApplicationContext("UserBeans.xml");
      User user = context.getBean("user", User.class);
      User user1 = context.getBean("user", User.class);
      //单例模式是相等的
      System.out.println(user==user1); //true
  }
  ```

  

- [prototype](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-prototype) 原型模式，生成的对象不同

  ```java
  /**
  * 测试单例模式和多例模式
  */
  @Test
  public void testDD(){
      ApplicationContext context = new ClassPathXmlApplicationContext("UserBeans.xml");
      User user = context.getBean("user", User.class);
      User user1 = context.getBean("user", User.class);
      //原型模式是不相等的
      System.out.println(user==user1); //false
  }
  ```

# 8 自动装配 `autowire`

| Mode          | Explanation                                                  |
| :------------ | :----------------------------------------------------------- |
| `no`          | (Default) No autowiring. Bean references must be defined by `ref` elements. Changing the default setting is not recommended for larger deployments, because specifying collaborators explicitly gives greater control and clarity. To some extent, it documents the structure of a system. |
| `byName`      | Autowiring by property name. Spring looks for a bean with the same name as the property that needs to be autowired. For example, if a bean definition is set to autowire by name and it contains a `master` property (that is, it has a `setMaster(..)` method), Spring looks for a bean definition named `master` and uses it to set the property. |
| `byType`      | Lets a property be autowired if exactly one bean of the property type exists in the container. If more than one exists, a fatal exception is thrown, which indicates that you may not use `byType` autowiring for that bean. If there are no matching beans, nothing happens (the property is not set). |
| `constructor` | Analogous to `byType` but applies to constructor arguments. If there is not exactly one bean of the constructor argument type in the container, a fatal error is raised. |

## 8.0 在`bean`中配置`autowire`属性

```xml
<bean id="address" class="org.suyuesheng.spring.sptu04.pojo.Address" p:address="jinan"/>

<bean id="user" class="org.suyuesheng.spring.sptu04.pojo.User" autowire="byName">
    <property name="age" value="12"/>
    <property name="name" value="ll"/>
</bean>
```

## 8.1 注解方式

https://blog.ahao.moe/posts/Spring_uses_@Autowired_in_three_ways.html

> 在Spring3.0之后，有效的自动装配策略分为`byType、byName、constructor`三种方式。注解Autowired默认使用byType来自动装配，如果存在类型的多个实例就尝试使用byName匹配，如果通过byName也确定不了，可以通过Primary和Priority注解来确定。
>
>
> 作者：清幽之地
> 链接：https://juejin.im/post/5c84b5285188257c5b477177
> 来源：掘金
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 8.1.1 前言 配置的头部文件

头部必须加`xmlns:context="http://www.springframework.org/schema/context"`

`xsi:schemaLocation`要加

```
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd"
```

必须有`<context:annotation-config/>`标签

即👇

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
```

### 8.1.2 在属性中注解

> 可以在属性中使用 **@Autowired** 注释来除去 setter 方法。当时使用 为自动连接属性传递的时候，Spring 会将这些传递过来的值或者引用自动分配给那些属性。

**即可以不用setter方法**

**Person类👇**

```java
package org.suyuesheng.spring.sptu04.pojo;

import org.springframework.beans.factory.annotation.Autowired;

public class Person {
    public Person() {
    }

    @Autowired
    private Address address;
    @Autowired
    private Phone phone;

    public Address getAddress() {
        return address;
    }

    public Phone getPhone() {
        return phone;
    }

    @Override
    public String toString() {
        return "Person{" +
                "address=" + address +
                ", phone=" + phone +
                '}';
    }
}

```

**<a name=phone类>phone类👇</a>**

```java
package org.suyuesheng.spring.sptu04.pojo;


public class Phone {
    public Phone() {
    }

    private String phoneBrand;

    public String getPhoneBrand() {
        return phoneBrand;
    }

    public void setPhoneBrand(String phoneBrand) {
        this.phoneBrand = phoneBrand;
    }

    @Override
    public String toString() {
        return "Phone{" +
                "phoneBrand='" + phoneBrand + '\'' +
                '}';
    }
}
```

**<a name=address类>address类👇</a>**

```java
package org.suyuesheng.spring.sptu04.pojo;

public class Address {
    private String address;

    public Address() {
    }

    public Address(String address) {
        this.address = address;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}

```

**<a name=xml>xml👇</a>**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <import resource="Beans.xml"/>
    <bean id="person" class="org.suyuesheng.spring.sptu04.pojo.Person"/>

    <bean id="phone" class="org.suyuesheng.spring.sptu04.pojo.Phone" p:phoneBrand="iphone"/>
</beans>
```

### 8.1.3 在构造器中注解

**可以不指定无参构造，可以没有setter方法**

> 在构造函数中使用 @Autowired。一个构造函数 @Autowired 说明当创建 bean 时，即使在 XML 文件中没有使用 元素配置 bean ，构造函数也会被自动连接。

person类👇

```java
package org.suyuesheng.spring.sptu04.pojo;

import org.springframework.beans.factory.annotation.Autowired;

public class Person {
    //可以不指定无参构造，可以没有setter方法
//    public Person() {
//    }
    @Autowired
    public Person(Address address, Phone phone) {
        this.address = address;
        this.phone = phone;
    }

    private Address address;

    private Phone phone;

    public Address getAddress() {
        return address;
    }

    public Phone getPhone() {
        return phone;
    }

    @Override
    public String toString() {
        return "Person{" +
                "address=" + address +
                ", phone=" + phone +
                '}';
    }
}
```

[Phone类](#phone类)

[Address类](#address类)

[xml配置文件](#xml)

### 8.1.4 在setter里注解

> 在 XML 文件中的 setter 方法中使用 **@Autowired** 注释来除去 元素。当 Spring遇到一个在 setter 方法中使用的 @Autowired 注释，它会在方法中视图执行 **byType** 自动连接。

person类👇

```java
package org.suyuesheng.spring.sptu04.pojo;

import org.springframework.beans.factory.annotation.Autowired;

public class Person {

    public Person() {
    }

    public Person(Address address, Phone phone) {
        this.address = address;
        this.phone = phone;
    }

    private Address address;

    private Phone phone;
    @Autowired
    public void setAddress(Address address) {
        this.address = address;
    }
    @Autowired
    public void setPhone(Phone phone) {
        this.phone = phone;
    }

    public Address getAddress() {
        return address;
    }

    public Phone getPhone() {
        return phone;
    }

    @Override
    public String toString() {
        return "Person{" +
                "address=" + address +
                ", phone=" + phone +
                '}';
    }
}

```

[Phone类](#phone类)

[Address类](#address类)

[xml配置文件](#xml)

### 8.1.5 **三种注入方式用谁？**

> 如果注入的属性是`必选`的属性，则通过构造器注入。
> 如果注入的属性是`可选`的属性，则通过setter方法注入。
> 至于field注入，不建议使用。
>
> 来源 https://blog.ahao.moe/posts/Spring_uses_@Autowired_in_three_ways.html

### 8.1.6 `@Autowired(required=false)`注入注意的问题

https://blog.csdn.net/static_coder/article/details/79580981

`@Autowired(required=true)`：当使用@Autowired注解的时候，其实默认就是`@Autowired(required=true)`，表示注入的时候，该bean必须存在，否则就会注入失败。`@Autowired(required=false)`：表示忽略当前要注入的bean，如果有直接注入，没有跳过，不会报错。

**实例👇**

person类👇

```java
package org.suyuesheng.spring.sptu04.pojo;

import org.springframework.beans.factory.annotation.Autowired;

public class Person {

    public Person() {
    }

    public Person(Address address, Phone phone) {
        this.address = address;
        this.phone = phone;
    }
    @Autowired(required = false)
    private Address address;
    @Autowired(required = false)
    private Phone phone;


    public Address getAddress() {
        return address;
    }

    public Phone getPhone() {
        return phone;
    }

    @Override
    public String toString() {
        return "Person{" +
                "address=" + address +
                ", phone=" + phone +
                '}';
    }
}

```

没有定义address的bean，因为person类`@Autowired(required = false)`，所以不会报错。

得到的person对象👇，address为null

```
Person{address=null, phone=Phone{phoneBrand='iphone'}}
```

#### 8.1.6.1 `@Autowired(required = false)`构造器注入和其他注入方式的不同

**采用构造器注入的话，只要有一个bean不存在，构造器里面所有的bean都为null**

**采用其他方式的话，如果bean不存在就会注入null，不影响其他的注入**

## 8.2 当环境复杂时

当有多个bean，`@autowired`装配会出错的时候，比如下面这种情况👇

要注入id为address且类为Address的bean👇

```java
@Autowired(required = false)
private Address address;
```

结果，有好多个bean，偏偏又都不符合条件👇

```xml
<bean id="address001" class="org.suyuesheng.spring.sptu04.pojo.Address" p:address="jinan"/>
<bean id="address221" class="org.suyuesheng.spring.sptu04.pojo.Address" p:address="jinan1"/>
<bean id="addressqew" class="org.suyuesheng.spring.sptu04.pojo.Address" p:address="jinan2"/>
<bean id="address21" class="org.suyuesheng.spring.sptu04.pojo.Address" p:address="jinan3"/>
```

这时有两个选择，一是@Qualifier，一是@Resource。

**先说一下优先级的问题**

当bean都不符合要求时，`@Autowired`先会byType去寻找，去找那个类符合要求的bean。如果有多个符合要求的bean就byName去寻找。

### `@Qualifier`

用`@Qualifier`可以指定一个类符合要求的bean，比如👇

```java
@Autowired(required = false)
@Qualifier(value = "address21")
private Address address;
```

`@Qualifier`要和`@Autowired`在一起

### `@Resource`

可以用`@Resource`，来进行依赖注入，`@Resource`的包是`javax.annotation`,可以减少和spring的耦合。**`@Resource`依赖注入的优先级是先byName再byType**。`@Resource`不可以和`@Autowired`放到一起。当环境复杂的时候`@Resource`可以指定符合要求的类👇

```java
@Resource(name = "address221")
private Address address;
```

@Resource不能进行构造器注入

### `@Autowired`和`@Resource`的区别

https://my.oschina.net/yqz/blog/1608023

https://www.jianshu.com/p/49a0929a8cac

优先级和包不一样

`@Autowired`和`@Qualifier`的包都是`org.springframework.beans.factory.annotation`，也就是说，都属于spring框架。

`@Resource`的包是`javax.annotation`,可以减少和spring的耦合。

> `@Autowired` 与`@Resource`：
> **1、`@Autowired`与`@Resource`都可以用来装配bean. 都可以写在字段上,或写在setter方法上。**
>
> 2、`@Autowired`默认**按类型装配**（这个注解是属业spring的），默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，如：
> `@Autowired(required=false) `，如果我们想使用名称装配可以结合`@Qualifier`注解进行使用，
>
> 
>
> 作者：wuxinliulei
> 链接：https://www.zhihu.com/question/39356740/answer/80926247
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

| name         | package                                        |
| ------------ | ---------------------------------------------- |
| `@Autowired` | `org.springframework.beans.factory.annotation` |
| `@Qualifier` | `org.springframework.beans.factory.annotation` |
| `@Resource`  | `javax.annotation`                             |

**优先级👇**

| 注解名称     | 优先级               |
| ------------ | -------------------- |
| `@Autowired` | `byType` >> `byName` |
| `@Resource`  | `byName` >> `byType` |

当`@Qualifier`或`@Resource`指定了bean后，**以指定的bean为准**

# 9 使用注解开发

## 9.1 头部

相较于往常，增加了两个上下文的命名空间，一个前缀

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
    <context:annotation-config/>
</beans>
```

<img src="http://q9efxddri.bkt.clouddn.com/20200501013459.png"/>

**要使用注解开发，必须要有这个包👇**

<img src="http://q9efxddri.bkt.clouddn.com/20200501013632.png"/>

## @Component

**用之前要先定义context的扫描，只有在扫描范围才能用**

```xml
<context:component-scan base-package="org.suyuesheng.spring.sptu05.pojo"/>
```

将类用注解的方式加入到spring容器👇

```java
//等价于<bean id="user" class="org.suyuesheng.spring.sptu05.pojo.User"/>
@Component
public class User
```

`@Component`后面跟括号，括号里面的值**相当于bean的id**

### @Component的衍生

功能相同，对应不同的层

| Annotation  | Meaning                                                      |
| ----------- | ------------------------------------------------------------ |
| @Component  | generic stereotype for any Spring-managed component          |
| @Repository | stereotype for persistence layer   DAO层                     |
| @Service    | stereotype for service layer    service层                    |
| @Controller | stereotype for presentation layer (spring-mvc)   Controller层 |



## @Value

将属性注入，可以属性注入，也可以setter方法注入

**如通过属性注入，可以不写setter方法**

**只能注入普通的数据类型，复杂的还得用配置文件**

```java
package org.suyuesheng.spring.sptu05.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//等价于<bean id="user" class="org.suyuesheng.spring.sptu05.pojo.User"/>
@Component
public class User {
    @Value("12")
    private int age;
    @Value("老刘")
    private String name;

    public User() {
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

```

## @Scope

使用注解设置作用域，[查看Bean的作用域相关配置](#Bean的作用域)

## 开发时的选择(推荐)

- xml用来管理bean，即将类加入到spring容器的工作由xml完成

- 注解负责属性的注入

- 使用过程中注意xml头部两个上下文的命名空间，一个前缀，开启注解配置，配置component扫描范围

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
  
      <context:component-scan base-package="org.suyuesheng.spring.sptu05"/>
      <context:annotation-config/>
  
  </beans>
  ```

## 完全使用java的方式配置Spring

需要一个Configuration类，相当于Bean.xml

```java
package org.suyuesheng.spring06.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.suyuesheng.spring06.pojo.User;

//@Configuration注解，表明这是一个配置spring容器的类，相当于Beans.xml
@Configuration
//@ComponentScan 为@Component指定范围，如果不指定范围，默认是全部，相当于<context:component-scan base-package="扫描的包“/>标签
@ComponentScan("org.suyuesheng.spring06.pojo")
//相当于import标签
@Import(AppConfig2.class)
public class AppConfig {
    //@Bean相当于<bean>标签，默认bean的id是方法名，也可以指定别名通过name={}。指定别名后，方法名就不好使了。方法的返回值相当于bean标签的class
    @Bean({"user","User","getUser"})
    public User getUser(){
        return new User();
    }
}

```

类的配置👇

```java
package org.suyuesheng.spring06.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

//@Component，将该类交由spring容器
@Component
public class User {

    //将属性注入，相当于在bean.xml里面配置依赖注入
    @Value("20")
    private int age;
    @Value("老款")
    private String name;

    public User() {
    }

    public User(int age, String name) {
        this.age = age;
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

```



获取注解的spring容器的context需要`AnnotationConfigApplicationContext`类👇

```java
@Test
public void testOne() {
    //获取注解的spring容器的context
    ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
    User getUser = context.getBean("getUser", User.class);
    System.out.println(getUser);
    //Perpp的来源是@Component("Perpp")
    Person person = context.getBean("Perpp", Person.class);
    System.out.println(person);
}
```

# 10 代理模式



































