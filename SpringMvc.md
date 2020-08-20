[TOC]

# 1 快速开始

## 1.0 jar包依赖

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523105806.png)

### 1.0.1 maven 的tomcat配置

```xml
<plugin>

    <groupId>org.apache.tomcat.maven</groupId>

    <artifactId>tomcat7-maven-plugin</artifactId>

    <version>2.2</version>

</plugin>
```



## 1.1 文件结构

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523105927.png)

## 1.2 具体文件内容

### 1.2.1 HelloCon.java（控制器Controller）

```java
package org.suyuesheng.spring.mvc.controler;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class HelloCon implements Controller {
    @Override
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg", "hehe");
        modelAndView.setViewName("hello");
//        httpServletRequest.setAttribute("ss", "rr");
//        httpServletResponse.sendRedirect("https://www.cnblogs.com/mikevictor07/p/3436393.html");
        return modelAndView;
    }
}

```

### 1.2.2 springMvc-servlet.xml(spring容器)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--BeanNameUrlHandlerMapping和SimpleControllerHandlerAdapter可以省略-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">

        <property name="prefix" value="/WEB-INF/jsp/"/>

        <property name="suffix" value=".jsp"/>
    </bean>

    <bean id="/hello" class="org.suyuesheng.spring.mvc.controler.HelloCon"/>

</beans>
```

### 1.2.3 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc01</servlet-name>
<!--        分发器-->
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springMvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc01</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

### 1.2.4 hello.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  Date: 2020/5/4
  Time: 3:35
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>

```



# 2 spring mvc的路

![img](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110007)

https://www.jianshu.com/p/8a20c547e245

> **SpringMVC执行流程:**
>  1.用户发送请求至前端控制器DispatcherServlet
>  2.DispatcherServlet收到请求调用处理器映射器HandlerMapping。
>  3.处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet。
>  4.DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
>  5.执行处理器Handler(Controller，也叫页面控制器)。
>  6.Handler执行完成返回ModelAndView
>  7.HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet
>  8.DispatcherServlet将ModelAndView传给ViewReslover视图解析器
>  9.ViewReslover解析后返回具体View
>  10.DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）。
>  11.DispatcherServlet响应用户。
>
> 
>
> 作者：CoderZS
> 链接：https://www.jianshu.com/p/8a20c547e245
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# 3 注解开发以及restful风格

## spring容器配置👇

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="org.spring.suyuesheng.mvc"/>
<!--    <mvc:annotation-driven>会自动注册RequestMappingHandlerMapping与RequestMappingHandlerAdapter两个Bean,这是Spring MVC为@Controller分发请求所必需的-->
    <mvc:annotation-driven />
<!--    如果发现是静态资源的请求，就将该请求转由Web应用服务器默认的Servlet处理，如果不是静态资源的请求，才由DispatcherServlet继续处理。-->
    <mvc:default-servlet-handler/>
    <context:annotation-config/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">

        <property name="prefix" value="/WEB-INF/jsp/"/>

        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## 控制器配置👇

**`RequestMapping`后面的value可以加斜杠也可以不加斜杠，效果都是一样的**

```java
package org.spring.suyuesheng.mvc.controller;


import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/hello")
public class HelloController {

    //从地址栏输入，默认是get方法
    @RequestMapping(value = "/h1",method = RequestMethod.GET)
    public String hello(Model model){
//        封装数据
        model.addAttribute("msg", "hello speng");
        return "hello";//被视图解析器处理ViewResolver
    }

    //restful 风格
    @RequestMapping("/h1/{a}/{b}")
    public String helloRestFul(@PathVariable int a, @PathVariable int b, Model model){
        model.addAttribute("msg", a+b);
        return "hello";
    }

    @GetMapping("/h2")
    public String helloGet(Model model){
        model.addAttribute("msg", "get");
        return "hello";
    }

    @PostMapping("/h2")
    public String helloPost(Model model){

        model.addAttribute("msg", "get");
        return "hello";
    }
}

```



# 4 转发和重定向

## 4.1 相关文件

[跳过](#转发和重定向的基本)

### 4.1.1 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.spring.suyueshneg</groupId>
  <artifactId>sptumvc-03</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <repositories>
    <repository>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.19</version>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.22</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.4</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
    </dependency>


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
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.1.1</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.8</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
      </resource>
    </resources>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.1</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
            <encoding>utf-8</encoding>
          </configuration>
        </plugin>
        <plugin>

          <groupId>org.apache.tomcat.maven</groupId>

          <artifactId>tomcat7-maven-plugin</artifactId>

          <version>2.2</version>

        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

### 4.1.2 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
  <servlet>
    <servlet-name>springmvc03</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvcconfig.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc03</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

### 4.1.3 spring容器配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <mvc:default-servlet-handler/>
    <mvc:annotation-driven/>
    <context:component-scan base-package="org.spring.suyuesheng.mvc03"/>
    <context:annotation-config/>


    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

### 4.1.4 <a name="研究转发和重定向的控制器">研究转发和重定向的控制器</a>

```java
package org.spring.suyuesheng.mvc03.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import javax.servlet.http.HttpServletRequest;


@Controller
@RequestMapping("/hello")
public class ReHelloController {

    @RequestMapping("/h1")
    public String hello(Model model){
        model.addAttribute("msg", "hhh");
        return "helllo";
    }

    //转发
    @RequestMapping("/h2")
    public String hello2(Model model, HttpServletRequest request){
        model.addAttribute("hello", "hi");
        request.setAttribute("hh",12);
        //也可以通过在url后面加东西传递数据
        return "forward:/hello/h8?username=77";
    }

    //重定向
    @RequestMapping("/h3")
   public String hello3(Model model){
        return "redirect:https://www.cnblogs.com/sogeisetsu/";
   }
//   传递参数
    @RequestMapping("/h4")
    public String hello4(RedirectAttributes model){
        model.addAttribute("username", "77");
        //相当于 redirect:/hello/h5?username=77
        return "redirect:/hello/h5";

    }

    @RequestMapping("/h5")
    public String hello5(Model model,String username){
        System.out.println(username);
        return "helllo";
    }
//    重定向传递参数隐藏传递
    @RequestMapping("/h6")
    public String hello6(RedirectAttributes model){
        model.addFlashAttribute("username", "liuji");
        return "redirect:/hello/h7";
    }

    @RequestMapping("/h7")
    public String hello7(@ModelAttribute(value = "username") String username, ModelMap modelMap){
        /*
        * model.addFlashAttribute("username", "liuji");
        * 可以用@ModelAttribute，或者用ModelMap获得
        * */
        System.out.println(username);
        //通过ModelMap获得👇
        System.out.println("modelmap能获得吗？");
        System.out.println("modelmap.get(\"username\")"+modelMap.get("username"));
        System.out.println(modelMap);
        return "helllo";
    }

    @RequestMapping("/h8")
    public String hello8(Model model,HttpServletRequest request,String username){
        //转发无法通过model传递参数
        System.out.println(model.getAttribute("hello"));
        //可以通过传统的request传递参数
        System.out.println(request.getAttribute("hh"));
        System.out.println(">>"+username);
        return "helllo";
    }
}

```



## 4.2 转发和重定向的基本<a name="转发和重定向的基本">实现</a>

### 转发

**转发只能在同一服务器的同一项目，url不变，request和response不变**。

springmvc实现转发的基本形式👇

```java
@RequestMapping("/h2")
public String hello2(Model model, HttpServletRequest request){
    model.addAttribute("hello", "hi");
    request.setAttribute("hh",12);
    return "forward:/hello/h8";
}
```

可以看到，所谓转发，即在返回字符串到到视图解析器(`ViewResolver`)的地方，在字符串前面写一个`forward`然后后面写路径。`/`指的是从tomcat规定的虚拟路径开始，走的是绝对路径；没有`/`或者前面是`./`表示是相对路径，是从当前目录开始的。` return "forward:/hello/h8"`就是一个绝对路径转发

### 重定向

**重定向，可以访问不同服务器的资源，url发生改变，request发生改变**

重定向的基本形式👇

```java
@RequestMapping("/h3")
public String hello3(Model model){
    return "redirect:https://www.cnblogs.com/sogeisetsu/";
}
```

重定向就是在路径前面加一个`redirect`,其余和转发一样。但必须要注意一点，**springmvc实现重定向和传统的servlet实现重定向是不同的，传统的的重定向的根路径是服务器的根路径，springmvc实现重定向的根路径是当前web项目的根路径(即从虚拟路径开始，如果tomcat没有设定虚拟路径的话那就是从服务器的根路径开始)**

### 4.2.1 转发和重定向区别

-  **重定向的特点:`redirect`**
  - 1. 地址栏发生变化
  - 2. 重定向可以访问其他站点(服务器)的资源
  - 3. 重定向是两次请求。**不能使用request对象来共享数据**
-  **转发的特点：`forward`**
  - 1. 转发地址栏路径不变
  - 2. 转发只能访问当前服务器下的资源
  - 3. 转发是一次请求，**可以使用request对象来共享数据**

## 4.3 转发和重定向的参数问题

**注意，探讨的是springmvc在转发和重定向时通过model来传参数，就单纯传参而言，传参的方式是多种多样的，本文不探讨传统的servlet传递数据的方式，只讨论通过model传参时转发和重定向的区别**

### 4.3.1 转发传递参数

**通过model转发无法传递参数**，可以通过request传参(当然，仅传参而言方法是多种多样的，这里只介绍一个)👇

```java
//转发
@RequestMapping("/h2")
public String hello2(Model model, HttpServletRequest request){
    model.addAttribute("hello", "hi");
    request.setAttribute("hh",12);
    return "forward:/hello/h8";
}
@RequestMapping("/h8")
public String hello8(Model model,HttpServletRequest request){
    //转发无法通过model传递参数
    System.out.println(model.getAttribute("hello"));
    //可以通过传统的request传递参数
    System.out.println(request.getAttribute("hh"));
    return "helllo";
}
```

即通过request的`setAttribute`和`getAttribute`传参。

还有一种方法是通过在url后面加东西传递数据，比如`return "forward:/hello/h8?username=77"`

### 4.3.2 重定向传递参数

*可以通过在url后面加东西实现，比如`return "redirect:/hello/h8?username=77"`只是此方法没有太麻烦，且不是通过model传递*

有一个专门服务于重定向的接口`RedirectAttributes`,该接口继承了Model接口。`public interface RedirectAttributes extends Model`

#### 4.3.2.1  `RedirectAttributes`的`addAttribute`方法传递参数

比如这样`model.addAttribute("username", "77");`相当于是在重定向的url后面加上参数👇

```java
//   传递参数
@RequestMapping("/h4")
public String hello4(RedirectAttributes model){
    model.addAttribute("username", "77");
    //相当于 redirect:/hello/h5?username=77
    return "redirect:/hello/h5";

}

//接收参数
@RequestMapping("/h5")
public String hello5(Model model,String username){
    System.out.println(username);
    return "helllo";
}
```

这种方法很**不安全**，参数直接暴露

#### 4.3.2.2 **`RedirectAttributes`**的`addFlashAttribute`方法

> 这种方式也能达到重新向带参，而且能隐藏参数，其原理就是放到session中，session在跳到页面后马上移除对象。所以你刷新一下后这个值就会丢掉

```java
//    重定向传递参数隐藏传递
@RequestMapping("/h6")
public String hello6(RedirectAttributes model){
    model.addFlashAttribute("username", "liuji");
    return "redirect:/hello/h7";
}
//获取addFlashAttribute传递的参数
@RequestMapping("/h7")
public String hello7(@ModelAttribute(value = "username") String username, ModelMap modelMap){
       /* 
        * model.addFlashAttribute("username", "liuji");
        * 可以用@ModelAttribute，或者用ModelMap获得
        * */
    System.out.println(username);
    //通过ModelMap获得👇
    System.out.println("modelmap能获得吗？");
    System.out.println("modelmap.get(\"username\")"+modelMap.get("username"));
    System.out.println(modelMap);
    return "helllo";
}
```

通过`addFlashAttribute(name,value)`传递的参数，可以用@ModelAttribute，或者用ModelMap获得

**modelMap、 modelAttribute以及RequestContextUtils.getInputFlashMap(request).get("key")都可以获取到RedirectAttributes设置的值**

# 5 乱码

配置两点，可以解决百分之八十的乱码，乱码的产生，其根本原因是编码方式的不统一，让浏览器，后端，数据以相同的编码方式，就不会出现乱码

## 5.1 配置监听器，使服务器返回的页面皆为utf-8

在web.xml

```xml
<!--  配置过滤器-->
  <filter>
    <filter-name>filterForCharSet</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceResponseEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>filterForCharSet</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

## 5.2 StringHttpMessageConverter,使@Response返回的页面以utf-8编码

### @ResponseBody乱码原因👇

> 在spring处理ResponseBody时涉及到org.springframework.http.converter.StringHttpMessageConverter这个类，该类在默认实现中将defaultCharset设为ISO-8859-1。当@RequestMapping标记的方法未配置produces属性时，将自动使用默认编码；如果配置了produces属性，AbstractHttpMessageConverter中的write方法将不会受supportedMediaTypes影响，而用produce设置的header赋值给contenttype。改造一下RequestMappingHandlerAdapter的配置，springMvc.xml如下：
>
> **注意：基于spring3.2以后的配置文件。编码配置需要放在<mvc:annotation-driven/>之前，否则无效。**
> 
> 版权声明：本文为CSDN博主「c.」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/cckevincyh/article/details/81227864

### <a name="@ResponseBody乱码问题解决">@ResponseBody乱码问题解决</a>

配置spring容器👇（**这个方法有缺陷，即使用@Datetimeformat会报错**）[**解决responsebody乱码问题最佳解决方案**](#解决responsebody乱码问题最佳解决方案)

```xml
<!--        @Response乱码问题解决-->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter" >
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter" />
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <property name="supportedMediaTypes">
                    <list>
                        <value>text/plain;charset=utf-8</value>
                        <value>text/html;charset=UTF-8</value>
                        <value>applicaiton/*;charset=UTF-8</value>
                    </list>
                </property>
            </bean>
        </list>
    </property>
</bean>
```

*其他解决办法👉 https://blog.csdn.net/cckevincyh/article/details/81227864   https://www.cnblogs.com/zou-zou/p/9345485.html*

### <a name="解决responsebody乱码问题最佳解决方案">解决responsebody乱码问题最佳解决方案</a>

```xml
<mvc:annotation-driven >
    <!-- 消息转换器 -->
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"/>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```



## 5.3 小知识，乱码问题

**服务器输出字符数据**

- 输出**字符数据**或者**字节数据**
  - **getOutputStream和getWriter这两个方法互相排斥，调用了其中的任何一个方法后，就不能再调用另一个方法。**
- 编码问题
  - https://github.com/sogeisetsu/javaweblearn/blob/master/day16/src/cn/suyuesheng/servlet/ServletCookieTest.java
  - Tomcat使用IOS 8859-1编码 我们要将他改成utf-8
  - https://segmentfault.com/a/1190000013126031
  - resp.setContentType("text/html;charset=utf-8"); 将自己的编码和客户端的解码设置成utf-8

# 6. json数据传输

## 6.1 json基础

- 基础

  -  概念： JavaScript Object Notation JavaScript对象表示法

    -  json现在多用于存储和交换文本信息的语法
    - 进行数据的传输
    -  JSON 比 XML 更小、更快，更易解析。

  -  语法：

    - 1. 基本规则

      -  数据在名称/值对中：json数据是由键值对构成的
      - 键**用引号(单双都行)引起来**，也可以不使用引号
      -  值得取值类型：
        - 1. 数字（整数或浮点数）
        - 2. 字符串（在双引号中）
        - 3. 逻辑值（true 或 false）
        - 4. 数组（在方括号中） {"persons":[{},{}]}
        - 5. 对象（在花括号中） {"address":{"province"："陕西"....}}
        - 6. null
      -  数据由逗号分隔：多个键值对由逗号分隔
      - 花括号保存对象：使用{}定义json 格式
      -  方括号保存数组：[]

    -  获取数据:

      - 1. json对象.键名

      - 2. json对象["键名"]

      - 3. 数组对象[索引]

      - 4. 遍历

        ![img](https://img.mubu.com/document_image/ef51d424-5689-4d07-b7b9-928479bec563-6002481.jpg)

- 解析器

  - 常见的解析器：Jsonlib，Gson，fastjson，jackson

  - jackson

    - https://github.com/sogeisetsu/javaweblearn/blob/master/day22/src/tk/suyuesheng/domain/JackSonTextDemo.java

    - 对象 转 json

      - 1.**ObjectMapper mapper = new ObjectMapper();**

      - \2. **String s = mapper.writeValueAsString(person);**或者 **mapper.writeValue(new File("e:"+File.separator+"a.txt"), person);**

      - writevalue 的多种形式

        ![img](https://img.mubu.com/document_image/3d7f955c-70bf-4d47-8987-a21b82c25a6b-6002481.jpg)

      - 注释

        - 在对象转json 的时候 对象的参数有两个注释
          - @JsonIgnore这个是在转json的时候默认不转它
          - @JsonFormat(pattern = "yyyy-MM-dd") 这个是在转json的时候采用pattern里定义的方式转

    - json 转对象

      - 把一段json格式的字符串转换为对象

      - 使用readValue方法

        ![img](https://img.mubu.com/document_image/210da424-0310-4f5c-bf35-652f7339fc11-6002481.jpg)

      - 该方法的参数

        - 

          ![img](https://img.mubu.com/document_image/11dcd3d5-7217-404a-b7cf-4186c9a74f29-6002481.jpg)

## 6.2 乱码

json的乱码问题和普通的乱码在根源上没有不一样，如果用@ResponseBody的话，所谓页面返回只是返回了文本，并非有视图解析器返回一个新的页面。详细请看👉[@ResponseBody乱码问题解决](#@ResponseBody乱码问题解决)

## 6.3 js 的json知识

### 6.3.1 json和字符串转换

https://www.cnblogs.com/gemeiyi/p/11045640.html

> JSON.parse(jsonstr); //可以将json字符串转换成json对象 
>
> JSON.stringify(jsonobj); //可以将json对象转换成json对符串 

### 6.3.2 json数据的读取

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110305.png)

### 6.3.2 表单转为json

借助jquery.serializejson.js插件

https://raw.githubusercontent.com/marioizquierdo/jquery.serializeJSON/master/jquery.serializejson.js

`$('#ajform').serializeJSON()`将表单转化为json，`JSON.stringify($('#ajform').serializeJSON())`将json转化为字符串格式，json传输要用这种格式(后面会讲到)

## 6.4 前后端数据传输

### 6.4.1 前端向后端传

格式如下👇

```javascript
$.ajax({
    url:"gjs",
    data:JSON.stringify($('#ajform').serializeJSON()),
    type:"post",
    contentType:"application/json;charset=UTF-8",
    success:function (data) {
        console.log(data);
        for(let key in data){
            console.log(data[key])
        }
        console.log(JSON.stringify($('#ajform').serializeJSON()));
        $('#ajh2').html(JSON.stringify(data));
        alert("成功")
    }
})
```

一定要配置`contentType:"application/json;charset=UTF-8",`并且传输到后端的数据(data)要是字符串格式，不能是原生的json格式。

### 6.4.2 后端接收前端的数据

#### 6.4.2.1接收非json格式

https://blog.csdn.net/weixin_39220472/article/details/80293888?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1

- @RequestParam来接收，起到的效果是request.getParamater()的效果

  ```java
  @ResponseBody
  @RequestMapping(value = "/one",method = RequestMethod.POST)
  public String one(@RequestParam("id") int id, @RequestParam("password") String password) throws JsonProcessingException {
      User user = new User(id, password);
      System.out.println(user);
      String s = new ObjectMapper().writeValueAsString(user);
      return s;
  }
  ```

- 不用@RequestParam来接收，这就要求传过来的name和java后端定义的一致

- 通过类来接收

  这要求传过来的数据内容和指定类相吻合

  ```java
  @ResponseBody
  @RequestMapping(value = "/gjs",method = RequestMethod.POST)
  public Object ch( User user){
      //        System.out.println(map);
      System.out.println(user);
      return user;
  }
  ```

#### 6.4.2.2接收json格式

接收json格式的指导思想就是`@RequestBody`，接收的格式可以是map、list、实体类

https://blog.csdn.net/weixin_39220472/article/details/80725574   👈此文章有糟粕，请结合评论使用

### 6.4.3 后端向前端传json数据

**先说一个坑,后端向前端传json的话，请直接传实体类，map或者list，不要传转成字符串的json，转成字符串的json在前端眼里只是字符串，他们要将此字符串用`JSON.parse(jsonstr)`处理，所以为了不给前端添麻烦，您就别直接传字符串了**

这一切多亏了他👉`@ResponseBody`，他可以让返回的字符串不走视图解析器

> @responseBody注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML
>  数据，需要注意的呢，在使用此注解之后不会再走试图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。
>
> 
>
> 作者：wanggs
> 链接：https://www.jianshu.com/p/1233b22738d8
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

- 向前端传实体类

  - ```java
    @ResponseBody
    @RequestMapping(value = "/gjs",method = RequestMethod.POST)
    public Object ch(@RequestBody User user){
        //        System.out.println(map);
        System.out.println(user);
        return user;
    }
    ```

    ![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110412.png)
    

- 向前端传map

  - ```java
    @ResponseBody
    @RequestMapping("/map")
    public Object four(){
        Map<String,Object> map = new HashMap<>();
        map.put("other", new String[]{"12","ewq","234rwef","132dfwf反对法"});
        map.put("name", "老刘");
        map.put("id", 12);
        map.put("user", new User(1223, "hh搜索"));
        //        List<Object> list = new ArrayList<>();
        //        list.add("12");
        //        list.add("收到");
        //        list.add(123);
        return map;
    }
    ```

    ![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110306.png)

- 传list

  - ```java
    @ResponseBody
    @RequestMapping("/fatjson")
    public Object hh(){
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.S");
        String date = simpleDateFormat.format(new Date());
        String s = JSON.toJSONString(new Date());
        String s1 = JSON.toJSONString(new Date(), SerializerFeature.WriteDateUseDateFormat);
        String s2 = JSON.toJSONStringWithDateFormat(new Date(), "yyyy-MM-dd HH:mm:ss.S");
        Object parse = JSON.parse(s2);
        System.out.println(s2);
        System.out.println(s);
        Object o = JSON.toJSON(date);
        List<Object> list = new ArrayList<>();
        list.add(o);
        list.add(s);
        list.add(s1);
        list.add(s2);
        list.add(parse);
        return list;
    }
    ```

    ![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110307.png)

# 7 联系mybatis

## 7.0.1 文件结构

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110308.png)

## github地址

https://github.com/sogeisetsu/springstudyy/tree/master/sptumvc-07

## 7.1 建表

### 7.1.1 **sql语句👇**

```sql
create database if not exists springstudy character set utf8;
use springstudy;
show databases ;
show tables ;
create table `User` (
    `uid` int primary key ,
    `username` varchar(20) not null ,
    `password` varchar(20) not null ,
    `status` char(1),
    `code` varchar(50),
    constraint check_status check ( status='Y'or 'N')
);
select * from springstudy.user;
desc springstudy.user;
insert into springstudy.user values(1,'root','1223456','Y','001');
insert into springstudy.user values(2,'老刘','1223456','Y','001');
insert into springstudy.user values(3,'老张','1223456','Y','001');
insert into springstudy.user values(4,'alicy','1223456','Y','001');
insert into springstudy.user values(5,'timi','1223456','Y','001');
insert into springstudy.user values(6,'tom','1223456','Y','001');
insert into springstudy.user values(7,'jckman','1223456','Y','001');
alter table springstudy.user modify uid int auto_increment;
alter table springstudy.user add `date` DATETIME;
```

### 7.1.2 表的结构

| Field    | Type          | Null | Key  | Default | Extra           |
| :------- | :------------ | :--- | :--- | :------ | :-------------- |
| uid      | int\(11\)     | NO   | PRI  | NULL    | auto\_increment |
| username | varchar\(20\) | NO   |      | NULL    |                 |
| password | varchar\(20\) | NO   |      | NULL    |                 |
| status   | char\(1\)     | YES  |      | NULL    |                 |
| code     | varchar\(50\) | YES  |      | NULL    |                 |
| date     | datetime      | YES  |      | NULL    |                 |

## 7.2 mybatis配置

### 7.2.1 mybatis在spring容器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="org.suyuesheng.spring7"/>
    <context:annotation-config/>
    <import resource="springmvcconfig.xml"/>
    <context:property-placeholder location="classpath:druid.properties"/>
<!--    datasource-->
<!--    连接池 druid-->
    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.name}"/>
        <property name="password" value="${jdbc.password}"/>

        <property name="initialSize" value="${jdbc.initialSize}"/>
        <property name="minIdle" value="${jdbc.minIdle}"/>
        <property name="maxActive" value="${jdbc.maxActive}"/>

        <property name="maxWait" value="${jdbc.maxWait}"/>

        <property name="timeBetweenEvictionRunsMillis" value="${jbbc.timeBetweenEvictionRunsMillis}"/>
        <property name="minEvictableIdleTimeMillis" value="${jdbc.minEvictableIdleTimeMillis}"/>

        <property name="validationQuery" value="${jdbc.validationQuery}"/>
        <property name="testWhileIdle" value="true"/>
    </bean>
<!--    sqlsessionFactory-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactory">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatisConfig.xml"/>
        <property name="mapperLocations" value="classpath:org/suyuesheng/spring7/mapper/*.xml"/>
    </bean>
<!--sqlsession-->
    <bean class="org.mybatis.spring.SqlSessionTemplate" id="sqlSessionTemplate" scope="prototype">
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
    
<!--mapper或service类-->
    <bean class="org.suyuesheng.spring7.mapper.UserMapperImpl" id="userMapper">
        <property name="sqlSession" ref="sqlSessionTemplate"/>
    </bean>
    <bean class="org.suyuesheng.spring7.services.UserService" id="userservice">
        <property name="userMapper" ref="userMapper"/>
    </bean>
</beans>
```

### 7.2.2 druid 连接池

**druid.properties👇**

```properties
jdbc.url=jdbc:mysql://localhost:3307/springstudy?characterEncoding=utf-8&useUnicode=true
jdbc.name=root
jdbc.password=15990904343
jdbc.initialSize=5
jdbc.minIdle=5
jdbc.maxActive=10
jdbc.maxWait=10000
#配置间隔多久启动一次DestroyThread，对连接池内的连接才进行一次检测，单位是毫秒
#检测时:1.如果连接空闲并且超过minIdle以外的连接，如果空闲时间超过minEvictableIdleTimeMillis设置的值则直接物理关闭。
#     2.在minIdle以内的不处理。
jbbc.timeBetweenEvictionRunsMillis=600000
#配置一个连接在池中最大空闲时间，单位是毫秒
jdbc.minEvictableIdleTimeMillis=300000
#用来检测连接是否有效的sql，要求是一个查询语句，常用select 'x'。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会起作用。
#mysql select 1
#oracle select 1 from dual
jdbc.validationQuery=select 1
#建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
jdbc.testWhileIdle=true
```

### 7.2.3 mybatisConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="logImpl" value="LOG4J"/>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="cacheEnabled" value="true"/>
    </settings>
    <typeAliases>
        <package name="org.suyuesheng.spring7.pojo"/>
    </typeAliases>
</configuration>
```

### 7.2.4 log4j配置

```properties
log4j.rootLogger=DEBUG,console,rollingFile
#表示Logger会在父Logger的appender里输出，默认为true
log4j.additivity.org.apache=true

# 控制台(console)
log4j.appender.console=org.apache.log4j.ConsoleAppender
#指定日志信息的最低输出级别
log4j.appender.console.Threshold=DEBUG
#表示所有消息都会被立即输出，设为false则不输出，默认值是true
log4j.appender.console.ImmediateFlush=true
#默认值是System.out。
log4j.appender.console.Target=System.out
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%d{yyyy/MM/dd HH:mm:ss,SSS}][%c.%M]%p:%m%n


# 回滚文件(rollingFile)
log4j.appender.rollingFile=org.apache.log4j.RollingFileAppender
log4j.appender.rollingFile.Threshold=WARN
log4j.appender.rollingFile.ImmediateFlush=true
log4j.appender.rollingFile.Append=true
log4j.appender.rollingFile.File=D:/logs/log.log4j3
log4j.appender.rollingFile.MaxFileSize=10mb
#指定可以产生的滚动文件的最大数，例如，设为2则可以产生logging.log4j.1，logging.log4j.2两个滚动文件和一个logging.log4j文件。
log4j.appender.rollingFile.MaxBackupIndex=50
log4j.appender.rollingFile.layout=org.apache.log4j.PatternLayout
log4j.appender.rollingFile.layout.ConversionPattern=[%-5p] %d(%r) --> [%t] %l: %m %x %n

# 日志输出级别
log4j.logger.org.suyuesheng=DEBUG
log4j.logger.java.sql=DEBUG
```

### 7.2.5 mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.suyuesheng.spring7.mapper.UserMapper">
    <cache
            eviction="FIFO"
            flushInterval="60000"
            size="512"
            readOnly="true"/>
    <resultMap id="userMap" type="user">
        <result property="uId" column="uid"/>
        <result property="userName" column="username"/>
    </resultMap>
    <select id="findAll" resultType="user">
        select * from springstudy.user
    </select>

    <select id="findById" resultType="user" parameterType="_int">
        select * from springstudy.user
        <where>
            <if test="uId!=null">
                uid=#{uId}
            </if>
        </where>
    </select>

    <insert id="insert" parameterType="user">
        insert into springstudy.user
        <choose>
            <when test="uId==null">
                (username, password, status, code, date)
                values(#{userName},#{password},#{status},#{code},#{date})
            </when>
            <when test="uId!=null">
                (uid, username, password, status, code, date)
                values(#{uId},#{userName},#{password},#{status},#{code},#{date})
            </when>
        </choose>
    </insert>


</mapper>
```

### 7.2.6 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.suyuesheng.spring7</groupId>
  <artifactId>sptumvc-07</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <repositories>
    <repository>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
  <dependencies>
    <!-- https://mvnrepository.com/artifact/javax.mail/mail -->
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.68</version>
    </dependency>
<!--    数据库连接池-->
    <!-- https://mvnrepository.com/artifact/c3p0/c3p0 -->
    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.22</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.11.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.11.0</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.19</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.4</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
    </dependency>


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
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.1.1</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.8</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
      </resource>
    </resources>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.1</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
            <encoding>utf-8</encoding>
          </configuration>
        </plugin>
        <plugin>

          <groupId>org.apache.tomcat.maven</groupId>

          <artifactId>tomcat7-maven-plugin</artifactId>

          <version>2.2</version>
          <configuration>
            <uriEncoding>UTF-8</uriEncoding>
          </configuration>

        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

# 8 一个真实的开发总结

## 8.1 网站地址

http://106.14.162.154:8086/

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110312.png)

## 8.2 代码文件

https://github.com/sogeisetsu/springstudyy/tree/master/sptumvc-07

### sql建表

```sql
show databases ;
create database if not exists springstudy character set utf8;
use springstudy;
create table `User` (
                        `uid` int primary key ,
                        `username` varchar(20) not null ,
                        `password` varchar(20) not null ,
                        `status` char(1),
                        `code` varchar(50),
                        constraint check_status check ( status='Y'or 'N')
);
show tables ;
desc User;
alter table User modify uid int auto_increment;
alter table User modify uid int auto_increment;
alter table User modify username varchar(20) unique not null ;

alter table User add `date` DATETIME;
alter table User add `email` varchar(25);
```

### 表格结构

| Field    | Type          | Null | Key  | Default | Extra           |
| :------- | :------------ | :--- | :--- | :------ | :-------------- |
| uid      | int\(11\)     | NO   | PRI  | NULL    | auto\_increment |
| username | varchar\(20\) | NO   | UNI  | NULL    |                 |
| password | varchar\(20\) | NO   |      | NULL    |                 |
| status   | char\(1\)     | YES  |      | NULL    |                 |
| code     | varchar\(50\) | YES  |      | NULL    |                 |
| date     | datetime      | YES  |      | NULL    |                 |
| email    | varchar\(25\) | YES  |      | NULL    |                 |

### 配置文件关系图

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110310.png)

### 导包

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110311.png)

### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
<!--静态资源的名字和controller的路径名字相同，需要特殊配置让其走tomcat默认的servlet-->
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>/login.html</url-pattern>
    <url-pattern>/regist.html</url-pattern>
  </servlet-mapping>
<!--  配置spring的DispatcherServlet-->
  <servlet>
    <servlet-name>springmvc06</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:Beans.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc06</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
<!--配置字符编码-->

  <filter>
    <filter-name>filterForCharSet</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceRequestEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
    <init-param>
      <param-name>forceResponseEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>filterForCharSet</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

<!--  配置session存活时间-->
  <session-config>
    <session-timeout>40</session-timeout>
  </session-config>
<!--  配置初始页面-->
  <welcome-file-list>
    <welcome-file>login.html</welcome-file>
  </welcome-file-list>

  <error-page>
    <error-code>404</error-code>
    <location>/error/sea-404page.html</location>
  </error-page>
  <error-page>
    <error-code>405</error-code>
    <location>/error/405.html</location>
  </error-page>
  <error-page>
    <error-code>500</error-code>
    <location>/error/500.html</location>
  </error-page>
</web-app>
```

### springmvc 配置（springmvcconfig.xml）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <context:component-scan base-package="org.suyuesheng.spring7"/>
    <!--        @Response乱码问题解决-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter" >
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter" />
                <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                    <property name="supportedMediaTypes">
                        <list>
                            <value>text/plain;charset=utf-8</value>
                            <value>text/html;charset=UTF-8</value>
                            <value>applicaiton/*;charset=UTF-8</value>
                        </list>
                    </property>
                </bean>
            </list>
        </property>
    </bean>
    <mvc:default-servlet-handler/>
    <mvc:annotation-driven/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"></property>
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

### 数据库配置(mybatisBean.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:component-scan base-package="org.suyuesheng.spring7"/>
    <context:annotation-config/>
    <import resource="springmvcconfig.xml"/>
<!--    引入配置文件-->
    <context:property-placeholder location="classpath:druid.properties"/>
<!--    datasource-->
<!--    连接池 druid-->
    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.name}"/>
        <property name="password" value="${jdbc.password}"/>

        <property name="initialSize" value="${jdbc.initialSize}"/>
        <property name="minIdle" value="${jdbc.minIdle}"/>
        <property name="maxActive" value="${jdbc.maxActive}"/>

        <property name="maxWait" value="${jdbc.maxWait}"/>

        <property name="timeBetweenEvictionRunsMillis" value="${jbbc.timeBetweenEvictionRunsMillis}"/>
        <property name="minEvictableIdleTimeMillis" value="${jdbc.minEvictableIdleTimeMillis}"/>

        <property name="validationQuery" value="${jdbc.validationQuery}"/>
        <property name="testWhileIdle" value="true"/>
    </bean>
<!--    sqlsessionFactory-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactory">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatisConfig.xml"/>
        <property name="mapperLocations" value="classpath:org/suyuesheng/spring7/mapper/*.xml"/>
    </bean>
<!--sqlsession-->
<!--    MapperScannerConfigurer会自动代理，其实不用配置-->
<!--    <bean class="org.mybatis.spring.SqlSessionTemplate" id="sqlSessionTemplate" scope="prototype">-->
<!--        <constructor-arg index="0" ref="sqlSessionFactory"/>-->
<!--    </bean>-->

    <!--    自动代理mapper接口-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer" id="mapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <property name="basePackage" value="org.suyuesheng.spring7.mapper"/>
    </bean>

    <bean class="org.suyuesheng.spring7.services.UserService" id="userservice">
        <property name="userMapper" ref="userMapper"/>
    </bean>

<!--    配置事务管理器-->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>



    <tx:advice transaction-manager="transactionManager" id="interceptor">
        <tx:attributes >
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <aop:config proxy-target-class="true">
        <aop:pointcut id="tx" expression="execution(* org.suyuesheng.spring7.services.*.*(..))"/>
        <aop:advisor advice-ref="interceptor" pointcut-ref="tx"/>
    </aop:config>
</beans>
```

### druid连接池配置 （druid.properties）

```properties
jdbc.url=jdbc:mysql://106.14.162.154:3306/springstudy?characterEncoding=utf-8&useUnicode=true
jdbc.name=root
jdbc.password=密码是常规密码
jdbc.initialSize=5
jdbc.minIdle=5
jdbc.maxActive=10
jdbc.maxWait=10000
#配置间隔多久启动一次DestroyThread，对连接池内的连接才进行一次检测，单位是毫秒
#检测时:1.如果连接空闲并且超过minIdle以外的连接，如果空闲时间超过minEvictableIdleTimeMillis设置的值则直接物理关闭。
#     2.在minIdle以内的不处理。
jbbc.timeBetweenEvictionRunsMillis=600000
#配置一个连接在池中最大空闲时间，单位是毫秒
jdbc.minEvictableIdleTimeMillis=300000
#用来检测连接是否有效的sql，要求是一个查询语句，常用select 'x'。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会起作用。
#mysql select 1
#oracle select 1 from dual
jdbc.validationQuery=select 1
#建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
jdbc.testWhileIdle=true
```



### 数据库配置 （mybatisConfig.xml）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="logImpl" value="LOG4J"/>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <setting name="cacheEnabled" value="true"/>
    </settings>
    <typeAliases>
        <package name="org.suyuesheng.spring7.pojo"/>
    </typeAliases>
</configuration>
```

### Beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <context:component-scan base-package="org.suyuesheng.spring7"/>
    <import resource="mybatisBean.xml"/>
    <import resource="springmvcconfig.xml"/>
    <bean class="org.suyuesheng.spring7.pojo.User" id="user"/>
<!--    配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/"/>
            <mvc:mapping path="/**"/>
            <mvc:mapping path="/**/*.html"/>
            <mvc:mapping path="/index.html"/>
            <mvc:exclude-mapping path="/login*"/>
            <mvc:exclude-mapping path="/regist*"/>
            <mvc:exclude-mapping path="/**/*.js"/>
            <mvc:exclude-mapping path="/**/*.css"/>
            <mvc:exclude-mapping path="/bootstrap-3.3.7-dist/**"/>
            <mvc:exclude-mapping path="/img/**"/>
            <mvc:exclude-mapping path="/active"/>
            <bean class="org.suyuesheng.spring7.interceptor.Logininterceptor" id="logininterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
```

## 8.3 开发过程中遇到的问题

### druid配置相关资料

https://my.oschina.net/xzfx/blog/478482

https://www.jianshu.com/p/e75d73129f51

https://blog.csdn.net/sjtu_chenchen/article/details/77618967

### MapperScannerConfigurer配置

https://www.cnblogs.com/daxin/p/3545040.html

###  aop中的propagation的7种配置的意思

[可能是最漂亮的Spring事务管理详解](https://juejin.im/post/6844903608224333838#heading-12)

https://my.oschina.net/wangyongzhi/blog/631200

> 下面是Spring中Propagation类的事务属性详解： 
> REQUIRED：支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。 
> SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行。 
> MANDATORY：支持当前事务，如果当前没有事务，就抛出异常。 
> REQUIRES_NEW：新建事务，如果当前存在事务，把当前事务挂起。 
> NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。 
> NEVER：以非事务方式执行，如果当前存在事务，则抛出异常。 
> NESTED：支持当前事务，如果当前事务存在，则执行一个嵌套事务，如果当前没有事务，就新建一个事务。 

### spring mvc路径匹配原则

![](https://suyueshengtuchuang.oss-cn-beijing.aliyuncs.com/20200523110309.png)

### 静态资源和controller重名

静态资源的名字和controller的路径名字相同，需要特殊配置让其走tomcat默认的servlet

比如说有个静态资源叫hello.html 有个controller的路径是/hello。那么访问`localhost/hello.html`的时候会自动跳转到`localhost/hello`。为了避免这种现像，需要在web.xml里面定义👇

```xml
<!--静态资源的名字和controller的路径名字相同，需要特殊配置让其走tomcat默认的servlet-->
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>/login.html</url-pattern>
    <url-pattern>/regist.html</url-pattern>
  </servlet-mapping>
```



### mvc:interceptors拦截器的用法

[mvc:interceptors拦截器的用法](https://www.cnblogs.com/lcngu/p/7096597.html)

### 阿里云服务器25端口的问题

 Could not connect to SMTP host: smtp.163.com, port: 25，阿里云服务器封禁了25，解决办法是使用465端口👇

```java
package org.suyuesheng.spring7.util;

import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

/**
 * 发邮件工具类
 */
public final class MailUtils {
    private static final String USER = "sys088519@163.com"; // 发件人称号，同邮箱地址
    private static final String PASSWORD = "授权码"; // 如果是qq邮箱可以使户端授权码，或者登录密码

    /**
     *
     * @param to 收件人邮箱
     * @param text 邮件正文
     * @param title 标题
     */
    /* 发送验证信息的邮件 */
    public static boolean sendMail(String to, String text, String title){
        try {
            final String SSL_FACTORY = "javax.net.ssl.SSLSocketFactory";
            final Properties props = new Properties();
            props.setProperty("mail.smtp.socketFactory.class", SSL_FACTORY);
            props.setProperty("mail.smtp.socketFactory.fallback", "false");
            props.put("mail.smtp.auth", "true");
            props.put("mail.smtp.host", "smtp.163.com");
            props.setProperty("mail.smtp.port", "465");
            props.setProperty("mail.smtp.socketFactory.port", "465");
            // 发件人的账号
            props.put("mail.user", USER);
            //发件人的密码
            props.put("mail.password", PASSWORD);

            // 构建授权信息，用于进行SMTP进行身份验证
            Authenticator authenticator = new Authenticator() {
                @Override
                protected PasswordAuthentication getPasswordAuthentication() {
                    // 用户名、密码
                    String userName = props.getProperty("mail.user");
                    String password = props.getProperty("mail.password");
                    return new PasswordAuthentication(userName, password);
                }
            };
            // 使用环境属性和授权信息，创建邮件会话
            Session mailSession = Session.getInstance(props, authenticator);

            // 创建邮件消息
            MimeMessage message = new MimeMessage(mailSession);
            // 设置发件人
            String username = props.getProperty("mail.user");
            /**
             * 发件人地址：sys088519@163.com
             * 发件人姓名：节能减排小组
             */
            InternetAddress form = new InternetAddress(username, "节能减排小组");
            message.setFrom(form);
            // 设置收件人
            InternetAddress toAddress = new InternetAddress(to);
            message.setRecipient(Message.RecipientType.TO, toAddress);

            // 设置邮件标题
            message.setSubject(title);

            // 设置邮件的内容体
            message.setContent(text, "text/html;charset=UTF-8");
            // 发送邮件
            Transport.send(message);
            return true;
        }catch (Exception e){
            e.printStackTrace();
        }
        return false;
    }

    public static void main(String[] args) throws Exception { // 做测试用
        MailUtils.sendMail("1446942825@qq.com","<h1>测试邮件，无须回复</h1><hr><p>这是一封测试邮件</p>","测试");
        System.out.println("发送成功");
    }



}
```

使用465端口还有一个证书的问题👉`[javax.net.ssl.SSLException](http://javax.net.ssl.sslexception/): java.lang.RuntimeException: Unexpected error: java.security.InvalidAlgorithmParameterException: the trustAnchors parameter must be non-empty`解决办法是👉https://blog.csdn.net/yu849893679/article/details/86081562

### tomcat的项目，不同端口访问的问题

https://blog.csdn.net/gang_strong/article/details/29415301

# 9 SpringMvc上传文件

## 0 学习链接

[Spring MVC 上传文件(upload files)](https://www.cnblogs.com/hamawep789/p/10923722.html)

[ajax上传带文件的form表单，springmvc接收](https://blog.csdn.net/m0_37572458/article/details/78797614)

[SpringMVC 单文件上传与多文件上传](https://juejin.im/post/594b31da1b69e60062a199fa)

[HTML上传文件的多种方式](https://www.jianshu.com/p/7636d5c60a8d)

## 1 前期准备

需要Multipart，所以需引入👇

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.2</version>
</dependency>
```

### 1.1 在spring容器中引入CommonsMultipartResolver👇

```xml
<bean id="multipartResolver"
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!--上传文件的最大大小，单位为字节 -->
    <property name="maxUploadSize" value="17367648787"></property>
    <!-- 上传文件的编码 -->
    <property name="defaultEncoding" value="UTF-8"></property>
</bean>
```

CommonsMultipartResolver有多个属性👇

> maxUploadSize 上传的最大字节数，-1代表没有任何限制
> maxInMemorySize 读取文件到内存中最大的字节数，默认是1024
> defaultEncoding 文件上传头部编码，默认是iso-8859-1，注意defaultEncoding必须和用户的jsp的pageEncoding属性一致，以便能正常读取文件
> uploadTempDir文件上传暂存目录，文件上传完成之后会清除该目录，模式是在servlet容器的临时目录，例如tomcat的话，就是在tomcat文件夹的temp目录
>
> maxUploadSizePerFile 跟maxUploadSize差不多，不过maxUploadSizePerFile是限制每个上传文件的大小，而maxUploadSize是限制总的上传文件大小
>
> ------------------------------------------------
> 版权声明：本文为CSDN博主「node2017」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/yingxiake/article/details/51148540

CommonsMultipartResolver的原理👉https://blog.csdn.net/suifeng3051/article/details/51659731

## 2 文件的上传

https://github.com/sogeisetsu/springstudyy/tree/master/springmvc-09

## 2.1 使用Ajax上传带文件的表单

```javascript
$('#tbutton').click(function () {
    let formData = new FormData($('#tform')[0]);
    $.ajax({
        url:"/h3",
        type:"POST",
        data:formData,
        contentType:false,
        // 告诉jQuery不要去处理发送的数据，用于对data参数进行序列化处理 这里必须false
        processData:false,
        cache:false,
        success:function (data) {
            console.log(data);
        }
    })
})
```

https://www.jianshu.com/p/380661f02997

在Ajax里面，表单data用FormData，记得要设置`contentType:false`和`processData:false`

## 2.2 使用springmvc接收上传的文件

有两种方法，一种是直接用MultipartFile接收，第二种方法是用MultipartHttpServletRequest来getFile来获取上传的file

关于MultipartFile👇

![](https://suyuesheng-biaozhun-blog-tupian.oss-cn-qingdao.aliyuncs.com/blogimg/20200525000819.png)

**接收文件的代码👇**

```java
@RequestMapping(value = "/h3",method = RequestMethod.POST)
public Object three(MultipartHttpServletRequest request){
    MultipartFile file = request.getFile("file");
    String contentType = file.getContentType();
    System.out.println("文件类型"+contentType);
    String filename = file.getOriginalFilename();
    String realPath = request.getSession().getServletContext().getRealPath("/WEB-INF/file");
    boolean b = UploadFileUtil.uploadFile(file, realPath);
    if(b){
        System.out.println("创建文件成功"+"\t"+filename);
    }else {
        System.out.println("文件创建失败");
    }
    return JSON.toJSON(new User("老刘", "12354657"));
}
```

```java
package org.suyuesheng.spring09.util;

import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;
import java.util.Random;

public class UploadFileUtil {
    public static boolean uploadFile(MultipartFile file,String path){
        try {
            String filename = file.getOriginalFilename();
            File realFile = new File(path, filename);
            if(!realFile.getParentFile().exists()){
                realFile.getParentFile().mkdirs();
            }
            if(realFile.exists()){
                String newFileName = new Random().nextInt(100) + filename;
                realFile.renameTo(new File(path, newFileName));
            }
            file.transferTo(realFile);
            return true;
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
    }
}

```

**关于@RestController**

相当于controller加responseBody

# 10 springmvc下载文件

## DEMO

```java
@GetMapping("/xiazai")
public void four(HttpServletRequest request, HttpServletResponse response) throws IOException {
    long start = System.currentTimeMillis();
    String path = request.getSession().getServletContext().getRealPath("/file");
    String filename="Mybatis第一天讲义.pdf";
    //刷新，不留缓存
    response.reset();

    //        response.setCharacterEncoding("UTF-8"); 因为监听器已经提前设置了request编码，所以不用额外设置
    //文件类型（二进制）
    response.setContentType("multipart/form-data");
    //3.Content-Disposition常用取值有：attachment和inline，

    //attachment：打开下载框

    //inline：将文件直接显示在页面

    response.setHeader("Content-Disposition", "attachment;fileName=" + URLEncoder.encode(filename, "UTF-8"));
    //👆切记，要将文件名编码
    OutputStream outputStream = response.getOutputStream();
    File file = new File(path, filename);
    System.out.println(file.getCanonicalPath());
    FileInputStream inputStream = new FileInputStream(file);
    int temp=0;
    byte[] bb = new byte[1024*1024];
    while ((temp=inputStream.read(bb))!=-1){
        outputStream.write(bb, 0, temp);
    }
    outputStream.close();
    inputStream.close();
    long end = System.currentTimeMillis();
    System.out.println("花费"+(end-start)+"毫秒");
}
```

