[TOC]

**tomcat在配置web.xml的时候，servlet是一个比较重要的问题，在这里讨论一下servlet中的几个痛点**

1. servlet url-pattern的匹配问题
2. url-pattern中`/`和`/*`的区别
3. url-pattern的优先级问题
4. 根路径`/`的匹配问题

# 1 servlet `url-pattern`的匹配问题

`url-pattern`有三种匹配模式，分别是路径匹配、精确匹配、后缀匹配

## 1.1 精确匹配

`<url-pattern>`中配置的项必须与url完全精确匹配。

代码举例👇

```xml
<servlet-mapping>
    <servlet-name>MyServlet</servlet-name>
    <url-pattern>/kata/detail.html</url-pattern>
    <url-pattern>/demo.html</url-pattern>
    <url-pattern>/table</url-pattern>
</servlet-mapping>
```

当在浏览器中输入如下几种url时，都会被匹配到该servlet
`http://10.43.11.143/myapp/kata/detail.html`
`http://10.43.11.143/myapp/demo.html`
`http://10.43.11.143/myapp/table`

注意：

`http://10.43.11.143/myapp/table/` 是非法的url，不会被当作`http://10.43.11.143/myapp/table`识别

另外上述url后面可以跟任意的查询条件，都会被匹配，如

`http://10.43.11.143/myapp/table?hello `这个请求就会被匹配到MyServlet。

## 1.2 路径匹配

以“/”字符开头，并以“/*”结尾的字符串用于路径匹配

代码举例👇

```xml
<servlet-mapping>
    <servlet-name>MyServlet</servlet-name>
    <url-pattern>/user/*</url-pattern>
</servlet-mapping>
```

路径以/user/开始，后面的路径可以任意。比如下面的url都会被匹配。  

`http://localhost:8080/appDemo/user/users.html ` 

`http://localhost:8080/appDemo/user/addUser.action ` 

`http://localhost:8080/appDemo/user/updateUser.actionl`

## 1.3 后缀匹配

以“*.”开头的字符串被用于后缀匹配

代码举例👇

```xml
<servlet-mapping>
    <servlet-name>MyServlet</servlet-name>
    <url-pattern>*.jsp</url-pattern>
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```

则任何扩展名为jsp或action的url请求都会匹配，比如下面的url都会被匹配  

`http://localhost:8080/appDemo/user/users.jsp `

`http://localhost:8080/appDemo/toHome.action`

## 注意：路径和后缀匹配无法同时设置

> 注意：路径和扩展名匹配无法同时设置，比如下面的三个<url-pattern>都是非法的，如果设置，启动tomcat服务器会报错。
>
> `<url-pattern>/kata/*.jsp</url-pattern>`
>
> `<url-pattern>/*.jsp</url-pattern>`
>
> `<url-pattern>he*.jsp</url-pattern>`

几个实例👇，不明白请看本文第三章

![](https://suyuesheng-biaozhun-blog-tupian.oss-cn-qingdao.aliyuncs.com/blogimg/20200524120943.png)

# 2 url-pattern中`/`和`/*`的区别

`<url-pattern>/</url-pattern>`

`<url-pattern>/*</url-pattern>`

先说`/*`，`/*`相对来讲比较好理解，它是路径匹配的一种，从范围上来讲，它是范围最广的路径匹配，所有的请求都符合它的要求，从精度上来讲，它是精度最低的路径匹配(**注意！我说的是路径匹配**)，路径匹配的优先级是从长到短的(*具体请看本文第三章*)，所以说它是精度最低的路径匹配。很多博客中说它的特点是匹配`*.jsp`,这不是废话吗？  `/*`本身就是路径匹配，它当然可以匹配`*.jsp`。

再说`/`,`/`**是匹配优先级最低的匹配**，当一个url和所有的`url-pattern`匹配都不合适的时候，这个url就会走`/`匹配，根本就没有什么`*.jsp`的限制，大家之所以产生了(客观上也确实是这样)`/`不会匹配`*.jsp`但是`/*`会匹配`*.jsp`的原因是在tomcat/conf/web.xml里面单独配置了`*.jsp`的配置，**具体请看本文第三章**

# 3 url-pattern的优先级问题

当一个url与多个servlet的匹配规则可以匹配时，则按照 “ 精确路径 > 最长路径>后缀匹配”这样的优先级匹配到对应的servlet。

![](https://suyuesheng-biaozhun-blog-tupian.oss-cn-qingdao.aliyuncs.com/blogimg/20200524123739.png)

**例1：**比如servletA 的url-pattern为 /test，servletB的url-pattern为 /* ，这个时候，如果我访问的url为http://localhost/test ，这个时候容器就会先进行精确路径匹配，发现/test正好被servletA精确匹配，那么就去调用servletA，不会去管servletB。

**例2：**比如servletA的url-pattern为/test/*，而servletB的url-pattern为/test/a/*，此时访问http://localhost/test/a时，容器会选择路径最长的servlet来匹配，也就是这里的servletB。 

**例3: 比如**servletA的url-pattern：*.action ，servletB的url-pattern为 `/ *`，这个时候，如果我访问的url为http://localhost/test.action，这个时候容器就会优先进行路径匹配，而不是去匹配扩展名，这样就去调用servletB。

![](https://suyuesheng-biaozhun-blog-tupian.oss-cn-qingdao.aliyuncs.com/blogimg/20200524120943.png)

**那么就产生了一个疑问。为什么/*会匹配到 *.jsp，但是/匹配不到 *.jsp呢？**

原因很简单，在tomcat/conf/web.xml里面会有如下配置

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>

<!-- The mappings for the JSP servlet -->
<servlet-mapping>
    <servlet-name>jsp</servlet-name>
    <url-pattern>*.jsp</url-pattern>
    <url-pattern>*.jspx</url-pattern>
</servlet-mapping>
```

👆可以清楚地看到 `*.jsp`作为名为jsp的servlet的后缀匹配，/*是路径匹配，其优先级高于后缀匹配，所以能匹配到后缀为jsp的文件。而`/`是级别最低的匹配，其级别低于后缀匹配，所以jsp文件不会被`url-pattern`为/的匹配到。

# 4 根路径`/`的匹配问题

大家应该会注意到一个问题，就是当url-pattern为`/*`的时候访问http://localhost:8080/会404，但是访问http://localhost:8080/index.html却没有问题(当然前提是在spring容器里面配置了`<mvc:default-servlet-handler/>`)。当url-pattern为`/`时，http://localhost:8080/会自动转发到http://localhost:8080/index.html而不会404。原因是什么呢？

首先，我们必须要明确，一个网址的根目录即/（比如http://localhost:8080/）到底意味着什么？经过实验发现/是很特殊的，它会被url-pattern为/*的匹配，但他不会被url-pattern为/匹配。

在tomcat中，/默认是属于会被defaultservlet匹配，但是其优先级低于路径匹配，所以当某一个servlet的url-pattern为/*时，/就会被这个servlet匹配，从而不被defaultservlet匹配。

在tomcat源代码中找到如下片段可以佐证我的看法👇

```xml
<!-- ==================== Default Welcome File List ===================== -->
<!-- When a request URI refers to a directory, the default servlet looks  -->
<!-- for a "welcome file" within that directory and, if present, to the   -->
<!-- corresponding resource URI for display.                              -->
<!-- If no welcome files are present, the default servlet either serves a -->
<!-- directory listing (see default servlet configuration on how to       -->
<!-- customize) or returns a 404 status, depending on the value of the    -->
<!-- listings setting.                                                    -->
<!--                                                                      -->
<!-- If you define welcome files in your own application's web.xml        -->
<!-- deployment descriptor, that list *replaces* the list configured      -->
<!-- here, so be sure to include any of the default values that you wish  -->
<!-- to use within your application.                                       -->
```

👆上面是讲 `Welcome File List`的，即`/`路径会被默认转发到`Welcome File List`中规定的网页，即初始页。我翻译一下上面的一部分，具体的可以谷歌翻译，翻译👉

> 翻译👇
>
> 当请求URI指向目录时，默认servlet在该目录中查找“欢迎文件”，如果存在，则在相应的资源URI中查找以进行显示。如果不存在欢迎文件，则默认servlet会提供目录列表（请参阅默认servlet配置中的有关如何自定义的内容）或返回404状态，具体取决于列表设置的值

/会重定向到欢迎页面的原因是`Welcome File List`的存在，`Welcome File List`发挥效果的前提是/必须被defaultservlet匹配。当某一个servlet的url-pattern为/*时，/就会被这个servlet匹配，从而不被defaultservlet匹配。所以只有在自己定义的servlet的url-pattern为/时，http://localhost:8080/会自动转发到http://localhost:8080/index.html而不会404