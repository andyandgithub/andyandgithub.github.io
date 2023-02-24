---
layout: post 
title: Spring Mvc 
categories: study 
tags : java 
toc: true
---

# SpringMVC简介

## 什么是MVC

MVC是一种软件架构的思想，将软件按照模型、视图、控制器来划分

M：Model，模型层，指工程中的JavaBean，作用是处理数据

JavaBean分为两类：

- 一类称为实体类Bean：专门存储业务数据的，如 Student、User 等
- 一类称为业务处理 Bean：指 Service 或 Dao 对象，专门用于处理业务逻辑和数据访问。

V：View，视图层，指工程中的html或jsp等页面，作用是与用户进行交互，展示数据

C：Controller，控制层，指工程中的servlet，作用是接收请求和响应浏览器

> MVC的工作流程： 用户通过视图层发送请求到服务器，在服务器中请求被Controller接收，Controller 调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果 找到相应的View视图，渲染数据后最终响应给浏览器

## 什么是SpringMVC

SpringMVC是Spring的一个后续产品，是Spring的一个子项目 SpringMVC 是 Spring 为表述层开发提供的一整套完备的解决方案。在表述层框架历经 Strust、WebWork、Strust2
等诸多产品的历代更迭之后，目前业界普遍选择了 SpringMVC 作为 Java EE 项目 表述层开发的首选方案。

注：三层架构分为表述层（或表示层）、业务逻辑层、数据访问层，表述层表示前台页面和后台`servlet`

## SpringMVC的特点

- Spring 家族原生产品，与 IOC 容器等基础设施无缝对接
- 基于原生的Servlet，通过了功能强大的前端控制器DispatcherServlet，对请求和响应进行统一处理
- 表述层各细分领域需要解决的问题全方位覆盖，提供全面解决方案
- 代码清新简洁，大幅度提升开发效率
- 内部组件化程度高，可插拔式组件即插即用，想要什么功能配置相应组件即可
- 性能卓著，尤其适合现代大型、超大型互联网项目要求

# 入门案例

## 开发环境

IDE：idea 2019.2 构建工具：maven3.5.4 服务器：tomcat8.5 Spring版本：5.3.1

## 创建maven工程

①添加web模块

`pom.xml`中添加`<packaging>war</packaging>`打包方式

②打包方式：war

③引入依赖

```xml

<dependencies>
    <!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>
    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>
</dependencies>
```

由于 Maven 的传递性，我们不必将所有需要的包全部配置依赖，而是配置最顶端的依赖，其他靠传递性导入

## 配置web.xml

注册SpringMVC的前端控制器DispatcherServlet

①默认配置方式

此配置作用下，SpringMVC的配置文件默认位于`WEB-INF`下，默认名称为`<servlet-name>-servlet.xml`，例如，以下配置所对应SpringMVC的配置文件位于`WEB-INF`
下，文件名为`springMVC-servlet.xml`

```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servletclass>
</servlet>
<servlet-mapping>
<servlet-name>springMVC</servlet-name>
<!--
设置springMVC的核心控制器所能处理的请求的请求路径
/所匹配的请求可以是/login或.html或.js或.css方式的请求路径
但是/不能匹配.jsp请求路径的请求
-->
<url-pattern>/</url-pattern>
</servlet-mapping>

```

②扩展配置方式

可通过`init-param`标签设置SpringMVC配置文件的位置和名称，通过`load-on-startup`标签设置SpringMVC前端控制器`DispatcherServlet`的初始化时间

```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servletclass>
    <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
    <init-param>
        <!-- contextConfigLocation为固定值 -->
        <param-name>contextConfigLocation</param-name>
        <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的
        src/main/resources -->
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!--
    作为框架的核心组件，在启动过程中有大量的初始化操作要做
    而这些操作放在第一次请求时才执行会严重影响访问速度
    因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
    -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
<servlet-name>springMVC</servlet-name>
<!--
设置springMVC的核心控制器所能处理的请求的请求路径
/所匹配的请求可以是/login或.html或.js或.css方式的请求路径
但是/不能匹配.jsp请求路径的请求
-->
<url-pattern>/</url-pattern>
</servlet-mapping>
```

`<url-pattern>`标签中使用`/`和`/*`的区别：

`/`所匹配的请求可以是`/login`或`.html`或`.js`或`.css`方式的请求路径，但是/不能匹配.jsp请求路径的请求

因此就可以避免在访问jsp页面时，该请求被`DispatcherServlet`处理，从而找不到相应的页面

`/*`则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用`/*`的写法

## 创建请求控制器

由于前端控制器对浏览器发送的请求进行了统一的处理，但是具体的请求有不同的处理过程，因此需要创建处理具体请求的类，即请求控制器

请求控制器中每一个处理请求的方法成为控制器方法 因为SpringMVC的控制器由一个POJO（普通的Java类）担任，因此需要通过`@Controller`
注解将其标识为一个控制层组件，交给Spring的IoC容器管理，此时SpringMVC才能够识别控制器的存在

```java

@Controller
public class HelloController {
}
```

## 创建SpringMVC的配置文件

```xml
        <!--
        处理静态资源，例如html、js、css、jpg
        若只设置该标签，则只能访问静态资源，其他请求则无法访问
        <!-- 自动扫描包 -->
<!--前缀+逻辑视图+后缀=物理视图-->
<context:component-scan base-package="com.andy.mvc.controller"/>
        <!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver"
      class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
<property name="order" value="1"/>
<property name="characterEncoding" value="UTF-8"/>
<property name="templateEngine">
    <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
        <property name="templateResolver">
            <bean
                    class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                <!-- 视图前缀 -->
                <property name="prefix" value="/WEB-INF/templates/"/>
                <!-- 视图后缀 -->
                <property name="suffix" value=".html"/>
                <property name="templateMode" value="HTML5"/>
                <property name="characterEncoding" value="UTF-8"/>
            </bean>
        </property>
    </bean>
</property>
</bean>
        <!--
        处理静态资源，例如html、js、css、jpg
        若只设置该标签，则只能访问静态资源，其他请求则无法访问
此时必须设置<mvc:annotation-driven/>解决问题
-->
<mvc:default-servlet-handler/>
        <!-- 开启mvc注解驱动 -->
<mvc:annotation-driven>
<mvc:message-converters>
    <!-- 处理响应中文内容乱码 -->
    <bean
            class="org.springframework.http.converter.StringHttpMessageConverter">
        <property name="defaultCharset" value="UTF-8"/>
        <property name="supportedMediaTypes">
            <list>
                <value>text/html</value>
                <value>application/json</value>
            </list>
        </property>
    </bean>
</mvc:message-converters>
</mvc:annotation-driven>
```

## 测试HelloWorld

①实现对首页的访问 在请求控制器中创建处理请求的方法

```java
// @RequestMapping注解：处理请求和控制器方法之间的映射关系
// @RequestMapping注解的value属性可以通过请求地址匹配请求，/表示的当前工程的上下文路径
// localhost:8080/springMVC/
@RequestMapping("/")
public String index(){
//设置视图名称
        return"index";
        }

@RequestMapping("/test/a")
public void index(){
//设置视图名称

}
//默认无返回值会去寻找/test/a.html
```

②通过超链接跳转到指定页面 在主页index.html中设置超链接

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<h1>首页</h1>
<a th:href="@{/hello}">HelloWorld</a><br/>
</body>
</html>

```

在请求控制器中创建处理请求的方法

```java
@RequestMapping("/hello")
public String HelloWorld(){
    return"target";
}

```

## 总结

浏览器发送请求，若请求地址符合前端控制器的`url-pattern`，该请求就会被前端控制器 `DispatcherServlet`处理。前端控制器会读取SpringMVC的核心配置文件，通过扫描组件找到控制器，
将请求地址和控制器中`@RequestMapping`注解的`value`属性值进行匹配，若匹配成功，该注解所标识的 控制器方法就是处理请求的方法。处理请求的方法需要返回一个字符串类型的视图名称，该视图名称会
被视图解析器解析，加上前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最终转发到视 图所对应页面

# @RequestMapping注解

## @RequestMapping注解的功能

从注解名称上我们可以看到，@RequestMapping注解的作用就是将请求和处理请求的控制器方法关联起来，建立映射关系。 SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。

## @RequestMapping注解的位置

`@RequestMapping`标识一个类：设置映射请求的请求路径的初始信息

`@RequestMapping`标识一个方法：设置映射请求请求路径的具体信息

```java

@Controller
@RequestMapping("/test")
public class RequestMappingController {
    //此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping() {
        return "success";
    }
}
```

## @RequestMapping注解的value属性

@RequestMapping注解的value属性通过请求的请求地址匹配请求映射 @RequestMapping注解的value属性是一个字符串类型的数组，表示该请求映射能够匹配多个请求地址所对应的请求
@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射

```html
<a th:href="@{/testRequestMapping}">测试@RequestMapping的value属性--
    >/testRequestMapping</a><br>
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
```

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
        return"success";
        }
```

## RequestMapping注解的method属性

@RequestMapping注解的method属性通过请求的请求方式（get或post）匹配请求映射 @RequestMapping注解的method属性是一个RequestMethod类型的数组，表示该请求映射能够匹配
多种请求方式的请求 若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错405：Request method 'POST' not supported

```html
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
<form th:action="@{/test}" method="post">
    <input type="submit">
</form>

```

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"},
        method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
        return"success";
        }

```

注： 1、对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解

- 处理get请求的映射-->`@GetMapping`
- 处理post请求的映射-->`@PostMapping`
- 处理put请求的映射-->`@PutMapping`
- 处理delete请求的映射-->`@DeleteMapping`

2、常用的请求方式有get，post，put，delete

但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理
若要发送put和delete请求，则需要通过spring提供的过滤器`HiddenHttpMethodFilter`，在`RESTful`部分会讲到

## @RequestMapping注解的params属性（了解）

`@RequestMapping`注解的params属性通过请求的请求参数匹配请求映射 @RequestMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

- "param"：要求请求映射所匹配的请求必须携带param请求参数
- "!param"：要求请求映射所匹配的请求必须不能携带param请求参数
- "param=value"：要求请求映射所匹配的请求必须携带param请求参数且param=value
- "param!=value"：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

```html
<a th:href="@{/test(username='admin',password=123456)">测试@RequestMapping的
    params属性-->/test</a><br>
```

```java

@RequestMapping(
        value = {"/testRequestMapping", "/test"}
        , method = {RequestMethod.GET, RequestMethod.POST}
        , params = {"username", "password!=123456"}
)
public String testRequestMapping(){
        return"success";
        }

```

注： 若当前请求满足`@RequestMapping`注解的value和method属性，但是不满足params属性，此时 页面回报错`400：Parameter conditions "username, password!=123456"
not met for actual request parameters: username={admin}, password={123456}`

## @RequestMapping注解的headers属性（了解）

@RequestMapping注解的headers属性通过请求的请求头信息匹配请求映射

@RequestMapping注解的headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

- "header"：要求请求映射所匹配的请求必须携带header请求头信息
- "!header"：要求请求映射所匹配的请求必须不能携带header请求头信息
- "header=value"：要求请求映射所匹配的请求必须携带header请求头信息且header=value
- "header!=value"：要求请求映射所匹配的请求必须携带header请求头信息且header!=value

若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面 显示404错误，即资源未找到

## consumes

指定请求的 `content-type` 内容：比如如下代码指定 `ContentType` 必须为 `multipart/form-data`（即包含文件域）的请求才能接收处理。

```java
@RequestMapping(value = "/testConsumes", method = Request.POST, consumes = "multipart/form-data")
```

比如如下代码指定 `ContentType` 不能为为 `multipart/form-data`（即包含文件域）的请求才能接收处理

```java
@RequestMapping(value = "/testConsumes", method = Request.POST, consumes = "!multipart/form-data")

```

## produces

指定返回的内容类型，仅当`request`请求头中的(`Accept`)类型中包含该指定类型才返回

## SpringMVC支持ant风格的路径

- `？`：表示任意的单个字符
- `*`：表示任意的0个或多个字符
- `**`：表示任意层数的任意目录 注意：在使用`**`时，只能使用`/**/xxx`的方式

## SpringMVC支持路径中的占位符（重点）

原始方式：`/deleteUser?id=1`
rest方式：`/user/delete/1`

SpringMVC路径中的占位符常用于RESTful风格中，当请求路径中将某些数据通过路径的方式传输到服 务器中，就可以在相应的`@RequestMapping`注解的value属性中通过占位符{xxx}表示传输的数据，在
通过`@PathVariable`注解，将占位符所表示的数据赋值给控制器方法的形参

```html
<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a><br>
```

```java
@RequestMapping("/testRest/{id}/{username}")
public String testRest(@PathVariable("id") String id,@PathVariable("username")
String username){
        System.out.println("id:"+id+",username:"+username);
        return"success";
        }
//最终输出的内容为-->id:1,username:admin
```

# SpringMVC获取请求参数

## 通过ServletAPI获取

将`HttpServletRequest`作为控制器方法的形参，此时`HttpServletRequest`类型的参数表示封装了当前请 求的请求报文的对象

```java
@RequestMapping("/testParam")
public String testParam(HttpServletRequest request){
        String username=request.getParameter("username");
        String password=request.getParameter("password");
        System.out.println("username:"+username+",password:"+password);
        return"success";
        }
```

## 通过控制器方法的形参获取请求参数

在控制器方法的形参位置，设置和请求参数同名的形参，当浏览器发送请求，匹配到请求映射时，在 `DispatcherServlet`中就会将请求参数赋值给相应的形参

```html
<a th:href="@{/testParam(username='admin',password=123456)}">测试获取请求参数--
    >/testParam</a><br>
```

```java
@RequestMapping("/testParam")
public String testParam(String username,String password){
        System.out.println("username:"+username+",password:"+password);
        return"success";
        }
```

注 若请求所传输的请求参数中有多个同名的请求参数，此时可以在控制器方法的形参中设置字符串 数组或者字符串类型的形参接收此请求参数 若使用字符串数组类型的形参，此参数的数组中包含了每一个数据
若使用字符串类型的形参，此参数的值为每个数据中间使用逗号拼接的结果

## @RequestParam

`@RequestParam`是将请求参数和控制器方法的形参创建映射关系

`@RequestParam`注解一共有三个属性：

- `value`：指定为形参赋值的请求参数的参数名
- `required`：设置是否必须传输此请求参数，默认值为true
    - 若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置defaultValue属性，则页面报错`400：Required String parameter 'xxx' is not present；`
    - 若设置为false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为null
- `defaultValue`：不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值为""时，则使用默认值为形参赋值

```java
// http://localhost:8080/springmvc_demo/param/mvc?username=xiaoxuanzi1654&password=16540504
@RequestMapping("/mvc")
public String getParam(String username,String password){
        return"success";
        }


// http://localhost:8080/springmvc_demo/param/mvc?user=xiaoxuanzi1654&pwd=16540504
@RequestMapping("/mvc")
public String getParam(@RequestParam(value = "user", required = true) String username,
@RequestParam(value = "pwd", required = true) String password){
        return"success";
        }
```

## @RequestHeader

- `@RequestHeader`是将请求头信息和控制器方法的形参创建映射关系
- `@RequestHeader`注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

```java
@RequestMapping("/mvc")
public String getParam(@RequestHeader(value = "referer", required = false, defaultValue = "aaa") String referer){
        return"success";
        }
```

## @CookieValue

- @CookieValue是将cookie数据和控制器方法的形参创建映射关系
- @CookieValue注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

```java
@RequestMapping("/mvc")
public String getParam(@CookieValue(value = "JSESSIONID") String jsessionId){
        return"success";
        }
```

## 通过POJO获取请求参数

可以在控制器方法的形参位置设置一个实体类类型的形参，此时若浏览器传输的请求参数的***参数名***和实体类中的***属性名***一致，那么请求参数就会为此属性赋值

```html

<form th:action="@{/testpojo}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    性别：<input type="radio" name="sex" value="男">男<input type="radio"
                                                        name="sex" value="女">女<br>
    年龄：<input type="text" name="age"><br>
    邮箱：<input type="text" name="email"><br>
    <input type="submit">
</form>
```

```java
@RequestMapping("/testpojo")
public String testPOJO(User user){
        System.out.println(user);
        return"success";
        }
//最终结果-->User{id=null, username='张三', password='123', age=23, sex='男',
        email='123@qq.com'}
```

## 解决获取请求参数的乱码问题

解决获取请求参数的乱码问题，可以使用SpringMVC提供的编码过滤器`CharacterEncodingFilter`，但是 必须在web.xml中进行注册
`web.xml`

```xml
<!--配置springMVC的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filterclass>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
<filter-name>CharacterEncodingFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```
乱码问题和tomcat的版本关系

- tomcat7没有解决乱码问题
- tomcat8解决了get的乱码问题，没有解决post的乱码问题

注： SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效

解决请求参数获取时候的乱码问题 此过滤器一定要配置到最前面，要不然会无效，因为在设置编码之前一定不能设置请求参数。

没有`forceEncoding`的话只会编码处理请求`request`的参数，设置为真的时候也可以处理响应`response`的编码

# 域对象共享数据

`request,application,session`三种域

客户端向服务器发送一个请求，服务器响应之后，这个请求对象就结束了

request的`setAttribute`和`getAttribute`都是在服务器端执行的，客户端并不知情。

request的`getParameter`是可以获取到客户端发送到服务器端的请求参数

session作用域存在于浏览器窗口关闭之前，只要浏览器窗口没有关闭，session对象就会一直存在、

application全局作用范围的存在范围就是只要服务器端没有关闭，application域对象中的数据就不会消失。

## 使用ServletAPI向request域对象共享数据

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request){
        request.setAttribute("testScope","hello,servletAPI");
        return"success";
}
```

## 使用ModelAndView向request域对象共享数据

通过modelandview向请求域共享数据 使用ModelAndView时，可以使用Model向请求域中共享数据 使用View功能设置逻辑视图，但是控制器方法的返回值一定是ModelAndView实例

```java
@RequestMapping("/testModelAndView")
public ModelAndView testModelAndView(){
/**
 * ModelAndView有Model和View的功能
 * Model主要用于向请求域共享数据
 * View主要用于设置逻辑视图，实现页面跳转
 */
        ModelAndView mav=new ModelAndView();
//向请求域共享数据
        mav.addObject("testScope","hello,ModelAndView");
//设置逻辑视图，实现页面跳转
        mav.setViewName("success");
        return mav;
}
```

## 使用Model向request域对象共享数据

```java
@RequestMapping("/testModel")
public String testModel(Model model){
        model.addAttribute("testScope","hello,Model");
        return"success";
}
```

## 使用map向request域对象共享数据

```java
@RequestMapping("/testMap")
public String testMap(Map<String, Object> map){
        map.put("testScope","hello,Map");
        return"success";
}
```

## 使用ModelMap向request域对象共享数据

```java
@RequestMapping("/testModelMap")
public String testModelMap(ModelMap modelMap){
        modelMap.addAttribute("testScope","hello,ModelMap");
        return"success";
}
```

## Model、ModelMap、Map的关系

Model、ModelMap、Map类型的参数其实本质上都是 `BindingAwareModelMap` 类型的

```java
public interface Model {
}

public class ModelMap extends LinkedHashMap<String, Object> {
}

public class ExtendedModelMap extends ModelMap implements Model {
}

public class BindingAwareModelMap extends ExtendedModelMap {
}

```

## 向session域共享数据

```java
@RequestMapping("/testSession")
public String testSession(HttpSession session){
        session.setAttribute("testSessionScope","hello,session");
        return"success";
}
```

```html
<p th:text="${testRequestScope}"></p>
<p th:text="${session.testRequestScope}"></p>
<p th:text="${application.testRequestScope}"></p>
```

## 向application域共享数据

```java
@RequestMapping("/testApplication")
public String testApplication(HttpSession session){
        ServletContext application=session.getServletContext();
        application.setAttribute("testApplicationScope","hello,application");
        return"success";
}
```

## 域对象的区别

- `pageContext`对象的范围只适用于当前页面范围，即超过这个页面就不能够使用了。所以使用`pageContext`对象向其它页面传递参数是不可能的。
- `request`对象的范围是指在一JSP网页发出请求到另一个JSP网页之间，随后这个属性就失效。
- `session`的作用范围为一段用户持续和服务器所连接的时间，但与服务器断线后，这个属性就无效。比如断网或者关闭浏览器。
- `application`的范围在服务器一开始执行服务，到服务器关闭为止。它的范围最大，生存周期最长。

# SpringMVC的视图

SpringMVC中的视图是View接口，视图的作用渲染数据，将模型Model中的数据展示给用户

SpringMVC视图的种类很多，默认有转发视图和重定向视图 当工程引入jstl的依赖，转发视图会自动转换为JstlView 若使用的视图技术为Thymeleaf，在SpringMVC的配置文件中配置了Thymeleaf的视图解析器，由此视
图解析器解析之后所得到的是ThymeleafView 最终都会封装到一个ModelAndView对象中。

## ThymeleafView

当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被SpringMVC配置文件中所配置 的视图解析器解析，视图名称拼接视图前缀和视图 后缀所得到的最终路径，会通过转发的方式实现跳转

```java
@RequestMapping("/testHello")
public String testHello(){
        return"hello";
}
```

## 转发视图

SpringMVC中默认的转发视图是InternalResourceView SpringMVC中创建转发视图的情况： 当控制器方法中所设置的视图名称以"`forward:`"为前缀时，创建`InternalResourceView`视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"`forward:`"去掉，剩余部分作为最终路径通过转发的方式实现跳转 例如"`forward:/`"，"`forward:/employee`"

```java
@RequestMapping("/testForward")
public String testForward(){
        return"forward:/testHello";
}

```

地址不变还是`testFrward`
此种方式不会解析页面中的thymeleaf语句,不会渲染视图

## 重定向视图

SpringMVC中默认的重定向视图是RedirectView

当控制器方法中所设置的视图名称以"`redirect:`"为前缀时，创建`RedirectView`视图，此时的视图名称不 会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"`redirect:`"去掉，剩余部分作为最终路径通过重定向的方式实现跳转 例如"`redirect:/`"，"`redirect:/employee`"

```java
@RequestMapping("/testRedirect")
public String testRedirect(){
        return"redirect:/testHello";
}
```

注： 重定向视图在解析时，会先将redirect:前缀去掉，然后会判断剩余部分是否以/开头，若是则会自动拼接上下文路径 地址变为testHello

## 视图控制器view-controller

当控制器方法中，仅仅用来实现页面跳转，即只需要设置视图名称时，可以将处理器方法使用viewcontroller标签进行表示

```xml
<!--
path：设置处理的请求地址
view-name：设置请求地址所对应的视图名称
-->
<mvc:view-controller path="/testView" view-name="success"></mvc:view-controller>
```

注： 当SpringMVC中设置任何一个view-controller时，其他控制器中的请求映射将全部失效，此时需 要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签：
`<mvc:annotation-driven />`

# RESTful

## RESTful简介

REST：Representational State Transfer，表现层资源状态转移。

①资源

资源是一种看待服务器的方式，即，将服务器看作是由很多离散的资源组成。每个资源是服务器上一个 可命名的抽象概念。因为资源是一个抽象的概念，所以它不仅仅能代表服务器文件系统中的一个文件、
数据库中的一张表等等具体的东西，可以将资源设计的要多抽象有多抽象，只要想象力允许而且客户端 应用开发者能够理解。与面向对象设计类似，资源是以名词为核心来组织的，首先关注的是名词。一个
资源可以由一个或多个URI来标识。URI既是资源的名称，也是资源在Web上的地址。对某个资源感兴 趣的客户端应用，可以通过资源的URI与其进行交互。

②资源的表述

资源的表述是一段对于资源在某个特定时刻的状态的描述。可以在客户端-服务器端之间转移（交 换）。资源的表述可以有多种格式，例如HTML/XML/JSON/纯文本/图片/视频/音频等等。资源的表述格
式可以通过协商机制来确定。请求-响应方向的表述通常使用不同的格式。

③状态转移

状态转移说的是：在客户端和服务器端之间转移（transfer）代表资源状态的表述。通过转移和操作资 源的表述，来间接实现操作资源的目的。

## RESTful的实现

具体说，就是 HTTP 协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。 它们分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。 REST
风格提倡 URL 地址使用统一的风格设计，从前到后各个单词使用斜杠分开，不使用问号键值对方 式携带请求参数，而是将要发送给服务器的数据作为 URL 地址的一部分，以保证整体风格的一致性。

|操作 |传统方式 |REST风格|
|----|----|----|
|查询操作| getUserById?id=1 |user/1-->get请求方式|
|保存操作| saveUser |user-->post请求方式|
|删除操作| deleteUser?id=1| user/1-->delete请求方式|
|更新操作| updateUser |user-->put请求方式|

## HiddenHttpMethodFilter

由于浏览器只支持发送get和post方式的请求，那么该如何发送put和delete请求呢？

SpringMVC 提供了 HiddenHttpMethodFilter 帮助我们将 POST 请求转换为 DELETE 或 PUT 请求 HiddenHttpMethodFilter 处理put和delete请求的条件：

a>当前请求的请求方式必须为`post`

b>当前请求必须传输请求参数`_method` 满足以上条件，`HiddenHttpMethodFilter`过滤器就会将当前请求的请求方式转换为请求参数`_method`的值，因此请求参数_method的值才是最终的请求方式
在web.xml中注册`HiddenHttpMethodFilte`

```xml

<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filterclass>
</filter>
<filter-mapping>
<filter-name>HiddenHttpMethodFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```

```html

<form th:action="@{/user/1}" method="post">
    <!--    真正的请求方式-->
    <input type="hidden" name="_method" value="DELETE"/>
    <input type="submit" value="删除用户"/>
</form>
```

目前为止，SpringMVC中提供了两个过滤器：`CharacterEncodingFilter`和`HiddenHttpMethodFilter`

在web.xml中注册时，必须先注册`CharacterEncodingFilter`，再注册`HiddenHttpMethodFilter` 原因：

在`CharacterEncodingFilter` 中通过 `request.setCharacterEncoding(encoding)` 方法设置字符集的`request.setCharacterEncoding(encoding)`
方法要求前面不能有任何获取请求参数的操作 而 `HiddenHttpMethodFilter` 恰恰有一个获取请求方式的操作：
`String paramValue = request.getParameter(this.methodParam);`

# RESTful案例

## 静态资源

使用默认配置的Servlet处理静态资源 当前工程的web.xml 配置的前端控制器 `DispatcherServlet` 的` url-pattern `是 `/Tomcat` 的 `web.xml `
配置的 `DefaultServlet`
的`url-pattern `也是 `/` 此时，浏览器发送的请求会优先被` DispatcherServlet` 进行处理，但是` DispatcherServlet` 无法处理静态资源
若配置了 `<mvc:default-servlet-handler /> `，则所有请求都会被` DefaultServlet` 处理。 若配置了`<mvc:default-servlet-handler />`
和 `<mvc:annotation-driven /> `，则浏览器发送的请求会先被
`DispatcherServlet` 处理，无法处理的在交给 `DefaultServlet` .

```xml

<mvc:default-servlet-handler/>
<mvc:annotation-driven/>
```

## 拼接路径

```html
<a th:href="@{'/employee/update/'+${employee.id}}">update</a>
```

# SpringMVC处理ajax请求

```html

<div id="app">
    <h1>index</h1>


    <input type="button" @click="testAjax()" value="测试ajax">
</div>
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
<script type="text/javascript">
    var vue = new Vue({
        el: "#app",
        methods: {
            testAjax() {
                axios.post(
                        "/SpringMVC/test/ajax?id=123",
                        {username: "admin", password: "12346"}
                ).then(response => {
                    console.log(response.data);
                })
            }
        }
    });
</script>
```

```java
@RequestMapping("/test/ajax")
public void testAjax(Integer id,@RequestBody String requestbody,HttpServletResponse httpServletResponse)throws IOException{
    System.out.println(requestbody);
    httpServletResponse.getWriter().write("hello ajax");
    
}
```

## @RequestBody

@RequestBody可以获取请求体信息，使用@RequestBody注解标识控制器方法的形参，当前请求的请求体就会为当前注解所标识的形参赋值

```html
<!--此时必须使用post请求方式，因为get请求没有请求体-->
<form th:action="@{/test/RequestBody}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit">
</form
```

```java
@RequestMapping("/test/RequestBody")
public String testRequestBody(@RequestBody String requestBody){
        System.out.println("requestBody:"+requestBody);
        return"success";
}
```

输出结果： `requestBody:username=admin&password=123456`

## @RequestBody获取json格式的请求参数

在使用了`axios`发送`ajax`请求之后，浏览器发送到服务器的请求参数有两种格式： 

1、name=value&name=value...，此时的请求参数可以通过request.getParameter() 获取，对应SpringMVC中，可以直接通过控制器方法的形参获取此类请求参数

2、{key:value,key:value,...}，此时无法通过request.getParameter()获取，之前我们使用操作 json的相关jar包json或jackson处理此类请求参数，可以将其转换为指定的实体类对象或map集合。在SpringMVC中，直接使用@RequestBody注解标识控制器方法的形参即可将此类请求参数 转换为java对象

使用@RequestBody获取json格式的请求参数的条件： 

1、导入jackson的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>

```

2、SpringMVC的配置文件中设置开启mvc的注解驱动

```xml
<!--开启mvc的注解驱动-->
<mvc:annotation-driven/>
```

3、在控制器方法的形参位置，设置json格式的请求参数要转换成的java类型（实体类或map）的参 数，并使用@RequestBody注解标识

```html
<input type="button" value="测试@RequestBody获取json格式的请求参数"
       @click="testRequestBody()"><br>
<script type="text/javascript" th:src="@{/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/js/axios.min.js}"></script>
<script type="text/javascript">
    var vue = new Vue({
        el: "#app",
        methods: {
            testRequestBody() {
                axios.post(
                        "/SpringMVC/test/RequestBody/json",
                        {username: "admin", password: "123456"}
                ).then(response => {
                    console.log(response.data);
                });
            }
        }
    });
</script>
```

```java
//将json格式的数据转换为map集合
@RequestMapping("/test/RequestBody/json")
public void testRequestBody(@RequestBody Map<String, Object> map,
        HttpServletResponse response)throws IOException{
        System.out.println(map);
//{username=admin, password=123456}
        response.getWriter().print("hello,axios");
        }
//将json格式的数据转换为实体类对象
@RequestMapping("/test/RequestBody/json")
<input type="button"value="测试@RequestBody获取json格式的请求参数"
@click="testRequestBody()"><br>
<script type="text/javascript"th:src="@{/js/vue.js}"></script>
<script type="text/javascript"th:src="@{/js/axios.min.js}"></script>
<script type="text/javascript">
        var vue=new Vue({
        el:"#app",
        methods:{
        testRequestBody(){
        axios.post(
        "/SpringMVC/test/RequestBody/json",
        {username:"admin",password:"123456"}
        ).then(response=>{
        console.log(response.data);
        });
        }
        }
        });
</script>
//将json格式的数据转换为map集合
@RequestMapping("/test/RequestBody/json")
public void testRequestBody(@RequestBody Map<String, Object> map,
        HttpServletResponse response)throws IOException{
        System.out.println(map);
//{username=admin, password=123456}
        response.getWriter().print("hello,axios");
        }
//将json格式的数据转换为实体类对象
@RequestMapping("/test/RequestBody/json")
public void testRequestBody(@RequestBody User user,HttpServletResponse
        response)throws IOException{
        System.out.println(user);
//User{id=null, username='admin', password='123456', age=null,
        gender='null'}
        response.getWriter().print("hello,axios");
        }
```

## @ResponseBody

@ResponseBody用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器

```java
@RequestMapping("/testResponseBody")
public String testResponseBody(){
//此时会跳转到逻辑视图success所对应的页面
    return"success";
}
@RequestMapping("/testResponseBody")
@ResponseBody
public String testResponseBody(){
//此时响应浏览器数据success
    return"success";
}
```

## @ResponseBody响应浏览器json数据

服务器处理ajax请求之后，大多数情况都需要向浏览器响应一个java对象，此时必须将java对象转换为 json字符串才可以响应到浏览器，之前我们使用操作json数据的jar包gson或jackson将java对象转换为
json字符串。在SpringMVC中，我们可以直接使用`@ResponseBody`注解实现此功能 `@ResponseBody`响应浏览器json数据的条件：

1. 导入`jackson`的依赖
2. SpringMVC的配置文件中设置开启mvc的注解驱动
3. 使用`@ResponseBody`注解标识控制器方法，在方法中，将需要转换为json字符串并响应到浏览器 的java对象作为控制器方法的返回值，此时SpringMVC就可以将此对象直接转换为json字符串并响应到浏览器

```xml

<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.3</version>
</dependency>
```

```html
<input type="button" value="测试@ResponseBody响应浏览器json格式的数据"
       @click="testResponseBody()"><br>
<script type="text/javascript" th:src="@{/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/js/axios.min.js}"></script>
<script type="text/javascript">
    var vue = new Vue({
        el: "#app",
        methods: {
            testResponseBody() {
                axios.post("/SpringMVC/test/ResponseBody/json").then(response => {
                    console.log(response.data);
                });
            }
        }
    });
</script>
```

map,实体类->json对象

list->json数组

```java
//响应浏览器list集合
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public List<User> testResponseBody(){
        User user1=new User(1001,"admin1","123456",23,"男");
        User user2=new User(1002,"admin2","123456",23,"男");
        User user3=new User(1003,"admin3","123456",23,"男");
        List<User> list=Arrays.asList(user1,user2,user3);
        return list;
}
//响应浏览器map集合
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public Map<String, Object> testResponseBody(){
        User user1=new User(1001,"admin1","123456",23,"男");
        User user2=new User(1002,"admin2","123456",23,"男");
        User user3=new User(1003,"admin3","123456",23,"男");
        Map<String, Object> map=new HashMap<>();
        map.put("1001",user1);
        map.put("1002",user2);
        map.put("1003",user3);
        return map;
}
//响应浏览器实体类对象
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public User testResponseBody(){
        return user;
}

```

## @RestController注解

`@RestController`注解是springMVC提供的一个**_复合注解_**，标识在控制器的类上，

就相当于为类添加了 `@Controller`注解，并且为其中的每个方法添加了`@ResponseBody`注解

# 文件上传和下载

## 文件下载

`ResponseEntity`用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文 使用ResponseEntity实现下载文件的功能

```java
@RequestMapping("/testDown")
public ResponseEntity<byte[]>testResponseEntity(HttpSession session)throws IOException{
//获取ServletContext对象
        ServletContext servletContext=session.getServletContext();
//获取服务器中文件的真实路径
        String realPath=servletContext.getRealPath("/static/img/1.jpg");
//创建输入流
        InputStream is=new FileInputStream(realPath);
//创建字节数组
        byte[]bytes=new byte[is.available()];
//将流读到字节数组中
        is.read(bytes);
//创建HttpHeaders对象设置响应头信息
        MultiValueMap<String, String> headers=new HttpHeaders();
//设置要下载方式以及下载文件的名字
        headers.add("Content-Disposition","attachment;filename=1.jpg");
//设置响应状态码
        HttpStatus statusCode=HttpStatus.OK;
//创建ResponseEntity对象
        ResponseEntity<byte[]>responseEntity=new ResponseEntity<>(bytes,headers,
        statusCode);
//关闭输入流
        is.close();
        return responseEntity;
}
```

## 文件上传

文件上传要求form表单的请求方式必须为`post`，并且添加属性`enctype="multipart/form-data"`

SpringMVC中将上传的文件封装到`MultipartFile`对象中，通过此对象可以获取文件相关信息

上传步骤：

①添加依赖

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
<groupId>commons-fileupload</groupId>
<artifactId>commons-fileupload</artifactId>
<version>1.3.1</version>
</dependency>
```

②在SpringMVC的配置文件中添加配置：

```xml
<!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
<bean id="multipartResolver"
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>
```

③控制器方法：

```java
@RequestMapping("/testUp")
public String testUp(MultipartFile photo,HttpSession session)throws
        IOException{
//获取上传的文件的文件名
        String fileName=photo.getOriginalFilename();
//处理文件重名问题
        String hzName=fileName.substring(fileName.lastIndexOf("."));
        fileName=UUID.randomUUID().toString()+hzName;
//获取服务器中photo目录的路径
        ServletContext servletContext=session.getServletContext();
        String photoPath=servletContext.getRealPath("photo");
        File file=new File(photoPath);
        if(!file.exists()){
            file.mkdir();
        }
        String finalPath=photoPath+File.separator+fileName;
//实现上传功能
        photo.transferTo(new File(finalPath));
        return"success";
        }

```

# 拦截器

## 拦截器的配置

SpringMVC中的拦截器用于拦截控制器方法的执行

SpringMVC中的拦截器需要实现`HandlerInterceptor`

SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置

默认为所有请求拦截

```xml

<mvc:interceptors>
    <bean id="firstInterceptor" class="com.andy.interceptor.FirstInterceptor"></bean>
</mvc:interceptors>
```

```xml

<bean class="com.andy.interceptor.FirstInterceptor"></bean>
<ref bean="firstInterceptor"></ref>
        <!-- 以上两种配置方式都是对DispatcherServlet所处理的所有的请求进行拦截 -->
<mvc:interceptors>
<mvc:interceptor>
    <!--    第一次目录的请求-->
    <mvc:mapping path="/*"/>
    <!--    所有路径的请求-->
    <mvc:mapping path="/**"/>
    <!--    排除扫描-->
    <mvc:exclude-mapping path="/testRequestEntity"/>
    <ref bean="firstInterceptor"></ref>
</mvc:interceptor>
</mvc:interceptors>
        <!--
        以上配置方式可以通过ref或bean标签设置拦截器，通过mvc:mapping设置需要拦截的请求，通过
        mvc:exclude-mapping设置需要排除的请求，即不需要拦截的请求
        -->
```

```java
package com.andy.interceptor;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class FirstInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("FirstInterceptor -->[pre]Handle");
//        return HandlerInterceptor.super.preHandle(request, response, handler);
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);

        System.out.println("FirstInterceptor -->[post]Handle");

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
        System.out.println("FirstInterceptor -->[after]Handle");

    }
}

```

## 拦截器的三个抽象方法

SpringMVC中的拦截器有三个抽象方法：

- `preHandle`：控制器方法执行之前执行`preHandle()`，其boolean类型的返回值表示是否拦截或放行，返回true为放行，即调用控制器方法；返回false表示拦截，即不调用控制器方法
- `postHandle`：控制器方法执行之后执行`postHandle()`
- `afterCompletion`：处理完视图和模型数据，渲染视图稳定完毕之后执行`afterCompletion()`

## 多个拦截器的执行顺序

①若每个拦截器的preHandle()都返回true

此时多个拦截器的执行顺序和拦截器在SpringMVC的配置文件的配置顺序有关：

`preHandle()`会按照配置的顺序执行，

而`postHandle()`和`afterCompletion()`会按照配置的反序执行

②若某个拦截器的`preHandle()`返回了false

`preHandle()`返回false和它之前的拦截器的`preHandle()`都会执行，

`postHandle()`都不执行，

返回false 的拦截器之前的拦截器的`afterCompletion()`会执行

# 异常处理器

## 基于配置的异常处理

SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口：`HandlerExceptionResolver`

HandlerExceptionResolver接口的实现类有：`DefaultHandlerExceptionResolver`和` SimpleMappingExceptionResolver`

SpringMVC提供了自定义的异常处理器`SimpleMappingExceptionResolver`，

使用方式：

```xml

<bean
        class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <!--
            properties的键表示处理器方法执行过程中出现的异常
            properties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
            -->
            <prop key="java.lang.ArithmeticException">error</prop>
        </props>
    </property>
    <!--
    exceptionAttribute属性设置一个属性名，将出现的异常信息在请求域中进行共享,即异常信息的属性名、键名
    -->
    <property name="exceptionAttribute" value="ex"></property>
</bean>
```

## 基于注解的异常处理

```java
//@ControllerAdvice将当前类标识为异常处理的组件
@ControllerAdvice
public class ExceptionController {
    //@ExceptionHandler用于设置所标识方法处理的异常
    @ExceptionHandler(ArithmeticException.class)
//ex表示当前请求处理中出现的异常对象
    public String handleArithmeticException(Exception ex, Model model) {
        model.addAttribute("ex", ex);
        return "error";
    }
}

```

# 注解配置SpringMVC

使用配置类和注解代替web.xml和SpringMVC配置文件的功能

## 创建初始化类，代替web.xml

在Servlet3.0环境中，容器会在类路径中查找实现`javax.servlet.ServletContainerInitializer`接口的类.如果找到的话就用它来配置Servlet容器。

Spring提供了这个接口的实现，名为`SpringServletContainerInitializer`，这个类反过来又会查找实现`WebApplicationInitializer`的类并将配 置的任务交给它们来完成。

Spring3.2引入了一个便利的`WebApplicationInitializer`基础实现，名为
`AbstractAnnotationConfigDispatcherServletInitializer`，当我们的类扩展了
`AbstractAnnotationConfigDispatcherServletInitializer`并将其部署到Servlet3.0容器的时候，容器会自 动发现它，并用它来配置Servlet上下文。

```java
public class WebInit extends
        AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * 指定spring的配置类代替spring的配置文件
     * @return
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    /**
     * 指定SpringMVC的配置类，代替SpringMvc的配置文件
     * @return
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebConfig.class};
    }

    /**
     * 指定DispatcherServlet的映射规则，即url-pattern
     * 设置SpringMvc的前端控制器DispatchServlet的url-parrern
     * @return
     */
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    /**
     * 添加过滤器
     * 返回过滤器数组
     * @return
     */
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
        encodingFilter.setEncoding("UTF-8");
        encodingFilter.setForceRequestEncoding(true);
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new
                HiddenHttpMethodFilter();//默认的过滤容器
        return new Filter[]{encodingFilter, hiddenHttpMethodFilter};
    }
}

```

## 创建SpringConfig配置类，代替spring的配置文件

```java
//标识为内部类
@Configuration
public class SpringConfig {
//ssm整合之后，spring的配置信息写在此类中
}

```

## 创建WebConfig配置类，代替SpringMVC的配置文件

```java

@Configuration
//扫描组件
@ComponentScan("com.andy.mvc.controller")
//开启MVC注解驱动
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
    //使用默认的servlet处理静态资源
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer
                                                        configurer) {
        configurer.enable();
    }

    //配置文件上传解析器,配置bean
    //bean注解可以将方法的返回值 作为bean进行管理，bean的id为方法的方法名字
    @Bean
    public CommonsMultipartResolver multipartResolver() {
        return new CommonsMultipartResolver();
    }

    //配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        FirstInterceptor firstInterceptor = new FirstInterceptor();
        registry.addInterceptor(firstInterceptor).addPathPatterns("/**");
    }

    //配置视图控制
/*@Override
public void addViewControllers(ViewControllerRegistry registry) {
registry.addViewController("/").setViewName("index");
}*/
//配置异常映射
/*@Override
public void
configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
SimpleMappingExceptionResolver exceptionResolver = new
SimpleMappingExceptionResolver();
Properties prop = new Properties();
prop.setProperty("java.lang.ArithmeticException", "error");
//设置异常映射
exceptionResolver.setExceptionMappings(prop);
//设置共享异常信息的键
exceptionResolver.setExceptionAttribute("ex");
resolvers.add(exceptionResolver);
}*/
//配置生成模板解析器
    @Bean
    public ITemplateResolver templateResolver() {
        WebApplicationContext webApplicationContext =
                ContextLoader.getCurrentWebApplicationContext();
// ServletContextTemplateResolver需要一个ServletContext作为构造参数，可通过
        //WebApplicationContext 的方法获得
        ServletContextTemplateResolver templateResolver = new
                ServletContextTemplateResolver(
                webApplicationContext.getServletContext());
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        templateResolver.setCharacterEncoding("UTF-8");
        templateResolver.setTemplateMode(TemplateMode.HTML);
        return templateResolver;
    }

    //生成模板引擎并为模板引擎注入模板解析器
    @Bean
    public SpringTemplateEngine templateEngine(ITemplateResolver
                                                       templateResolver) {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver);
        return templateEngine;
    }

    //生成视图解析器并未解析器注入模板引擎
    @Bean
    public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setCharacterEncoding("UTF-8");
        viewResolver.setTemplateEngine(templateEngine);
        return viewResolver;
    }
}


```

## 测试功能

```java
@RequestMapping("/")
public String index(){
        return"index";
}

```

# SpringMVC执行流程

## SpringMVC常用组件

- DispatcherServlet：前端控制器，不需要工程师开发，由框架提供
    - 作用：统一处理请求和响应，整个流程控制的中心，由它调用其它组件处理用户的请求
- HandlerMapping：处理器映射器，不需要工程师开发，由框架提供
    - 作用：根据请求的url、method等信息查找Handler，即控制器方法。匹配控制器方法
- Handler：处理器，需要工程师开发`/controller`
    - 作用：在DispatcherServlet的控制下Handler对具体的用户请求进行处理。
- HandlerAdapter：处理器适配器，不需要工程师开发，由框架提供
    - 作用：通过HandlerAdapter对处理器（控制器方法）进行执行。调用控制器方法
- ViewResolver：视图解析器，不需要工程师开发，由框架提供
    - 作用：进行视图解析，得到相应的视图，例如：ThymeleafView、InternalResourceView、 RedirectView
- View：视图
    - 作用：将模型数据通过页面展示给用户

## DispatcherServlet初始化过程

`DispatcherServlet` 本质上是一个 Servlet，所以天然的遵循 Servlet 的生命周期。所以宏观上是 Servlet 生命周期来进行调度。

①初始化`WebApplicationContext`

所在类：`org.springframework.web.servlet.FrameworkServlet`

②创建`WebApplicationContext`

所在类：`org.springframework.web.servlet.FrameworkServlet`

③`DispatcherServlet`初始化策略

`FrameworkServlet`创建`WebApplicationContext`后，刷新容器，调用`onRefresh(wac)`，

此方法在`DispatcherServlet`中进行了重写，调用了`initStrategies(context)`方法，初始化策略，即初始化`DispatcherServlet`的各个组件
所在类：`org.springframework.web.servlet.DispatcherServlet`

## DispatcherServlet调用组件处理请求

①`processRequest()`

`FrameworkServlet`重写`HttpServlet`中的`service()`和`doXxx()`，这些方法中调用了`processRequest(request, response)`

所在类：`org.springframework.web.servlet.FrameworkServlet`

②`doService()`

所在类：`org.springframework.web.servlet.DispatcherServlet`

③`doDispatch()`

所在类：`org.springframework.web.servlet.DispatcherServlet`

④`processDispatchResult()`

## SpringMVC的执行流程

1) 用户向服务器发送请求，请求被SpringMVC 前端控制器 `DispatcherServlet` 捕获。
   
2) `DispatcherServlet`对请求URL进行解析，得到请求资源标识符（URI），判断请求URI对应的映射： 
   
   a) 不存在

        i. 再判断是否配置了 `mvc:default-servlet-handler`

        ii. 如果没配置，则控制台报映射查找不到，客户端展示404错误

        iii. 如果有配置，则访问目标资源（一般为静态资源，如：JS,CSS,HTML），找不到客户端也会展示404 错误

   b) 存在则执行下面的流程

3) 根据该URI，调用`HandlerMapping`获得该Handler配置的所有相关的对象（包括Handler对象以及 Handler对象对应的拦截器），最后以`HandlerExecutionChain`执行链对象的形式返回。
   
4) `DispatcherServlet` 根据获得的Handler，选择一个合适的`HandlerAdapter`。
   
5) 如果成功获得`HandlerAdapter`，此时将开始执行拦截器的`preHandler(…)`方法【正向】
   
6) 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)方法，处理请求。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作： 
   
   a)`HttpMessageConveter`： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息

   b) 数据转换：对请求消息进行数据转换。如String转换成Integer、Double等

   c) 数据格式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等

   d) 数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中

7) Handler执行完成后，向`DispatcherServlet` 返回一个`ModelAndView`对象。
   
8) 此时将开始执行拦截器的`postHandle(...)`方法【逆向】。
   
9)  根据返回的ModelAndView（此时会判断是否存在异常：如果存在异常，则执行 `HandlerExceptionResolver`进行异常处理）选择一个适合的`ViewResolver`进行视图解析，根据Model
   和View，来渲染视图。

10) 渲染视图完毕执行拦截器的`afterCompletion(…)`方法【逆向】。
    
11) 将渲染结果返回给客户端。

# SSM整合

整合：Spring 和SpringMVC各自创建自己的IOC容器，各自管理自己的组件

SpringMVC的ioc容器是在DispatchServlet初始化的时候创建。

Spring的Ioc容器床架必须早于SpringMVC

监听器->过滤器->Servlet

Springioc容器的创建可以放在监听器或者过滤器的初始化过程中创建

springMVC和spring的ioc容器初始化的关系

spring的ioc容器是 ***父容器*** ，springmvc中的ioc容器是 ***子容器*** ，父容器不能访问子容器的bean，子容器能够访问父容器的bean

- mvc的初始化时机是在mvc的前端控制器初始化的时候初始化的
- 而前端控制器初始化之前会经过 监听器和过滤器 这里使用的是监听器
- 所以是要在监听器里面写代码 让sping的ioc容器在监听器里面初始化
- 而这个监听器spring已经写好提供给你了
- 就是服务器启动时一次性将所有bean加载到应用域

## ContextLoaderListener

Spring提供了监听器`ContextLoaderListener`，实现`ServletContextListener`
接口，可监听ServletContext的状态，在web服务器的启动，读取Spring的配置文件，创建Spring的IOC容器。web 应用中必须在web.xml中配置

```xml

<listener>
    <!--
    配置Spring的监听器，在服务器启动时加载Spring的配置文件
        Spring配置文件默认位置和名称：/WEB-INF/applicationContext.xml
    可通过上下文参数自定义Spring配置文件的位置和名称
    -->
    <listenerclass>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
        <!--自定义Spring配置文件的位置和名称-->
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath:spring.xml</param-value>
</context-param>

```

## 准备工作

①创建Maven Module

②导入依赖

```xml

<packaging>war</packaging>
<properties>
<spring.version>5.3.1</spring.version>
</properties>
<dependencies>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>${spring.version}</version>
</dependency>
<!--springmvc-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${spring.version}</version>
</dependency>
<!-- Mybatis核心 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>
<!--mybatis和spring的整合包-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
</dependency>
<!-- 连接池 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.0.9</version>
</dependency>
<!-- junit测试 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<!-- MySQL驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>
<!-- log4j日志 -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.2.0</version>
</dependency>
<!-- 日志 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
<!-- ServletAPI -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
<!-- Spring5和Thymeleaf整合包 -->
<dependency>
    <groupId>org.thymeleaf</groupId>
    <artifactId>thymeleaf-spring5</artifactId>
    <version>3.0.12.RELEASE</version>
</dependency>
</dependencies>
```

|**包**| javax.servlet-api |
|----|----|
|spring-webmvc| 间接依赖 springioc的四个核心jar包 + spring-web包 | 
|jackson-databind| springmvc 在异步请求处理JSON数据的时候会使用 |
|thymeleaf-spring5| springmvc + thymeleaf 整合 |
|spring-jdbc| 只使用里面的事务支持，其会为我们提供已经编写好的事务管理的切面类 |
|spring-aspects|AOP功能的支持 | 
|mybatis| 持久层框架 | 
|pagehelper| 为mybatis 提供分页功能的插件 | 
|mybatis-spring| mybatis 和 spring 的整合包 :其提供了一个SqlSessionFactoryBean，用于创建SqlSession | 
|druid| 德鲁伊连接池，提供数据源 | 
|mysql-connector-java| MYSQL驱动 | 
|spring-test|Spring提供的测试功能，可以直接在测试代码中注入容器当中的Bean | 
|log4j （**druid/mybatis 选用**）| 日志 （经验证发现 , Druid / mybatis 内部使用 log4j，而且是 **provide**的间接依赖，可能是检测到有包就用，当你在系统里面依赖 log4j ，不提供log4j的配置文件，则会发生提示 ：log4j:WARN No appenders could be found for logger (com.alibaba.druid.pool.DruidDataSource).<br>log4j:WARN Please initialize the log4j system properly.）<br><br>因此：要么就依赖log4j.jar 且 配置 log4j.properties/log4j.xml 给 Druid 使用，要么就不提供依赖，druid也不会检测到系统有jar包，不会给出 WARN | 
|logback-classic（**Spring/thymeleaf 整合包使用**）| slf4j的日志实现<br>1. thymeleaf 使用了 sl4j 的依赖，因此需要提高 logback 实现，并且配置logback;  <br>2. spring-context 会间接依赖spring.jcl 包判断日志实现的时候会默认为 slfj 的具体实现（发现机制），因此可以配置个 slf4j 的具体实现使用Spring的日志。若系统内部没有 slf4j ，会搜寻 log4j2，还是没有则默认使用 JUL日志输出。 | 
|javax.servlet-api| MVC的Controller中可能会使用到Servlet的API |
|commons-fileupload| SpringMVC底层支持文件上传需要jar包 |


③创建表

```sql
CREATE TABLE `t_emp`
(
    `emp_id`   int(11) NOT NULL AUTO_INCREMENT,
    `emp_name` varchar(20) DEFAULT NULL,
    `age`      int(11) DEFAULT NULL,
    `sex`      char(1)     DEFAULT NULL,
    `email`    varchar(50) DEFAULT NULL,
    PRIMARY KEY (`emp_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

## 配置web.xml
配置四个东西 ：

1. 核心控制器 DispatcherServlet ; 
2. ContextLoaderListener ; 
3. 编码过滤器（放过滤器中最前）；
4. 请发方法处理器 HiddenMethodFilter

```xml
<!-- 配置Spring的编码过滤器 -->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filterclass>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
<filter-name>CharacterEncodingFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
        <!-- 配置处理请求方式PUT和DELETE的过滤器 -->
<filter>
<filter-name>HiddenHttpMethodFilter</filter-name>
<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filterclass>
</filter>
<filter-mapping>
<filter-name>HiddenHttpMethodFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
        <!-- 配置SpringMVC的前端控制器 -->
<servlet>
<servlet-name>DispatcherServlet</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servletclass>
<!-- 设置SpringMVC的配置文件的位置和名称 -->
<init-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:SpringMVC.xml</param-value>
</init-param>
<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
<servlet-name>DispatcherServlet</servlet-name>
<url-pattern>/</url-pattern>
</servlet-mapping>
        <!-- 设置Spring的配置文件的位置和名称 -->
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath:Spring.xml</param-value>
</context-param>
        <!-- 配置Spring的监听器 -->
<listener>
<listenerclass>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```


## 创建SpringMVC的配置文件并配置

只列出必选项 和 推荐项：
1. Controller包的扫描 （必选）
2. thymeleaf 的视图解析器（必选）
3. 静态资源处理 （必选）
4. 视图控制器（可选）
5. 注解驱动（必选）



```xml
<!--扫描组件-->
<context:component-scan base-package="com.andy.ssm.controller">
</context:component-scan>
        <!--配置视图解析器-->
<bean id="viewResolver"
      class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
<property name="order" value="1"/>
<property name="characterEncoding" value="UTF-8"/>
<property name="templateEngine">
    <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
        <property name="templateResolver">
            <bean
                    class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                <!-- 视图前缀 -->
                <property name="prefix" value="/WEB-INF/templates/"/>
                <!-- 视图后缀 -->
                <property name="suffix" value=".html"/>
                <property name="templateMode" value="HTML5"/>
                <property name="characterEncoding" value="UTF-8"/>
            </bean>
        </property>
    </bean>
</property>
</bean>
        <!-- 配置访问首页的视图控制 -->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
        <!-- 配置默认的servlet处理静态资源 -->
<mvc:default-servlet-handler/>
        <!-- 开启MVC的注解驱动 -->
<mvc:annotation-driven/>
```

## 搭建MyBatis环境

1. `resources/mybatis-config.xml` 中编写配置，其中关于数据源的配置交给  在Spring中进行配置，向SpringIOC容器中注入 `SqlSessionFactory` 对象。
2. `resources/` 下创建同包的路径的Mapper目录，并编写mapper.xml 
3. 配置分页插件

①创建属性文件`jdbc.properties`

```properties
jdbc.user=boss
jdbc.password=123456
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
jdbc.driver=com.mysql.cj.jdbc.Driver
```

②创建MyBatis的核心配置文件mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--将下划线映射为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
    <plugins>
        <!--配置分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
</configuration>
```

③创建Mapper接口和映射文件

```java
public interface EmployeeMapper {
    List<Employee> getEmployeeList();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.andy.ssm.mapper.EmployeeMapper">
    <select id="getEmployeeList" resultType="Employee">
        select * from t_emp
    </select>
</mapper>
```

④创建日志文件log4j.xml

非必须，但还是配下好。
在 resources/ 下编写 log4j.xml，创建就能生效了。
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}
%m (%F:%L) \n"/>
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug"/>
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info"/>
    </logger>
    <root>
        <level value="debug"/>
        <appender-ref ref="STDOUT"/>
    </root>
</log4j:configuration>

```

lockback 日志(Spring/mybatis) 

非必须，但还是配下好。
在 resources/ 下编写 log4j.xml，创建就能生效了。

```xml
xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration>
<configuration debug="true">
    <!-- 指定日志的输出位置 -->
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- 日志的输出格式 -->
            <!-- 按照顺序分别是: 时间、日志级别、线程名称、打印日志的类、日志的主体内容、换行 -->
            <pattern>[%d{HH:mm:ss.SSS}] [%-5level] [%thread] [%logger] [%msg]%n</pattern>
        </encoder>
    </appender>

    <!-- 根据特殊需求指定局部的日志级别 : SQL 语句是 info 级别 -->
    <logger name="com.atguigu.crowd.mapper">
        <level value="DEBUG"/>
    </logger>

    <!-- 设置全局的日志级别。日志级别分别是 : DEBUG、INFO、WARN、ERROR -->
    <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志 -->
    <root level="error">
        <!-- 指定打印日志的appender，这里通过 STDOUT 引用了前面配置的 appender -->
        <appender-ref ref="stdout"/>
    </root>
</configuration>
```


## 创建Spring的配置文件并配置
配置分为三大部分：
一： 1. 包扫描、排除@Controller

二：事务相关
1. 事务管理器
2. 事务注解驱动

三：MyBaits需要的配置：
1. 引入 jdbc.properties
2. druid 数据源（需要1）
3. `SqlSessionFactoryBean` 注册到IOC（需要2）
4.. 产生的mapper代理类注册到IOC → 即配置 `MapperScannerConfigurer`


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd">
    <!--扫描组件-->
    <context:component-scan base-package="com.andy.ssm">
        <context:exclude-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <!-- 引入jdbc.properties -->
    <context:property-placeholder location="classpath:jdbc.properties">
    </context:property-placeholder>
    <!-- 配置Druid数据源 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
    <!-- 配置用于创建SqlSessionFactory的工厂bean可以在Spring的ioc容器中直接获取SQLsession对象 -->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 设置MyBatis核心配置文件的路径（可以不设置） -->
        <property name="configLocation" value="classpath:mybatis-config.xml">
        </property>
        <!-- 设置数据源 -->
        <property name="dataSource" ref="dataSource"></property>
        <!-- 设置类型别名所对应的包 -->
        <property name="typeAliasesPackage" value="com.andy.ssm.pojo">
        </property>
        <!--
        设置映射文件的路径
        若映射文件所在路径和mapper接口所在路径一致，则不需要设置
        -->
        <!--
        <property name="mapperLocations" value="classpath:mapper/*.xml">
        </property>
        -->
    </bean>
    <!--
    配置mapper接口的扫描配置
    由mybatis-spring提供，可以将指定包下所有的mapper接口通过SqlSession    创建动态代理
    并将这些动态代理作为IOC容器的bean管理
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.andy.ssm.mapper"></property>
    </bean>


    <!--    配置事务管理器-->
    <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="datasource"></property>
    </bean>
    <!--    开启事务的注解驱动，使用注解@Transactional标识的方法和或类中所有的方法进行管理-->
    <tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>
</beans>
```

测试：直接访问视图控制器配置的首页看是否正常即可。

## 分页操作

分页页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>title</title>
    <link rel="stylesheet" th:href="@{/static/css/index_work.css}">
</head>
<body>
<!--<h1>首页</h1>-->
<!--<a th:href="@{/hello}">HelloWorld</a><br/>-->
<div id="app">

    <table border="1" cellpadding="0" cellspacing="0" style="text-align:center;" id="dataTable">
        <tr th:colspan="6">employee list</tr>
        <tr>
            <th>流水号</th>
            <th>id</th>
            <th>empName</th>
            <th>age</th>
            <th>gender</th>
            <th>option(<a th:href="@{/to/add}">add</a>)</th>
        </tr>
        <tr th:each="employee ,status: ${page.list}">
            <td th:text="${status.count}"></td>
            <td th:text="${employee.empId}"></td>
            <td th:text="${employee.empName}"></td>
            <td th:text="${employee.age}"></td>
            <td th:text="${employee.gender}"></td>
            <td>
                <a @click="deleteEmployee()" th:href="@{'/employee/'+${employee.empId}}">delete</a>
                <a th:href="@{'/employee/'+${employee.empId}}">update</a>
            </td>

        </tr>
    </table>
    <div style="text-align:center;">
        <a th:if="${page.hasPreviousPage}" th:href="@{/employee/page/1}"> 首页</a>
        <a th:if="${page.hasPreviousPage}" th:href="@{'/employee/page/'+${page.prePage}}"> 上一页</a>
        <span th:each="num: ${page.navigatepageNums}">
            <a th:if="${ page.pageNum==num}" style="color: darkred" th:href="@{'/employee/page/'+${num}}"
               th:text="'['+${num}+']'"></a>
            <a th:if="${page.pageNum!=num}" style="color: blue" th:href="@{'/employee/page/'+${num}}"
               th:text="${num}"></a>
        </span>

        <a th:if="${page.hasNextPage}" th:href="@{'/employee/page/'+${page.nextPage}}"> 下一页</a>
        <a th:if="${page.hasNextPage}" th:href="@{'/employee/page/'+${page.pages}}"> 尾页</a>
    </div>
    <!--    <form method="post">-->
    <!--        <input type="hidden" name="_method" value="delete">-->
    <!--    </form>-->
</div>
<!--<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>-->
<!--<script type="text/javascript">-->
<!--    var vue = new Vue({-->
<!--        el: "#app",-->
<!--        methods: {-->
<!--            deleteEmployee() {-->
<!--                console.log("day day day ");-->
<!--                var form = document.getElementsByTagName("form")[0];-->
<!--                form.action = event.target.href;-->
<!--                form.submit();-->
<!--                event.preventDefault();-->
<!--            }-->
<!--        }-->
<!--    });-->

<!--</script>-->
</body>
</html>
```

Employeecontroller.java

```java
package com.andy.controller;

import com.andy.pojo.Employee;
import com.andy.service.EmployeeService;
import com.github.pagehelper.PageInfo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import java.util.List;

@Controller
public class EmployeeCotroller {
    @Autowired
    private EmployeeService employeeService;

    @RequestMapping(value = "/employee", method = RequestMethod.GET)
    public String getEmployeeList(Model model) {
        List<Employee> list = employeeService.getAllEmployee();
        model.addAttribute("allemployee", list);
        return "employeelist";
    }

    @RequestMapping(value = "/employee/page/{pagenum}", method = RequestMethod.GET)
    public String getEmployeePage(@PathVariable("pagenum") Integer pagenum, Model model) {
        PageInfo<Employee> page = employeeService.getEmployeePage(pagenum);
        model.addAttribute("page", page);
        return "employeepage";
    }
}

```

EmployeeService.java

```java
package com.andy.service;

import com.andy.mapper.EmployeeMapper;
import com.andy.pojo.Employee;
import com.github.pagehelper.PageInfo;
import org.springframework.beans.factory.annotation.Autowired;

import java.util.List;

public interface EmployeeService {

    List<Employee> getAllEmployee();

    PageInfo<Employee> getEmployeePage(Integer pagenum);
}

```

实现类

```java
package com.andy.service;

import com.andy.mapper.EmployeeMapper;
import com.andy.pojo.Employee;
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
@Transactional
public class EmployeeServiceImpl implements EmployeeService {
    @Autowired
    protected EmployeeMapper employeeMapper;

    @Override
    public List<Employee> getAllEmployee() {
        List<Employee> list = employeeMapper.getAllEmployee();
        System.out.println(list);
        return list;

    }

    @Override
    public PageInfo<Employee> getEmployeePage(Integer pagenum) {
        PageHelper.startPage(pagenum, 4);
        List<Employee> list = employeeMapper.getAllEmployee();
        PageInfo<Employee> page = new PageInfo<>(list, 5);

        return page;
    }
}

```

employeeMapper.java

```java
package com.andy.mapper;

//import org.apache.ibatis.annotations.Mapper;

import com.andy.pojo.Employee;

import java.util.List;

//@Mapper
public interface EmployeeMapper {

    List<Employee> getAllEmployee();
}

```

EmployeeMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.andy.mapper.EmployeeMapper">
    <select id="getAllEmployee" resultType="Employee">
        select * from t_emp
    </select>
</mapper>
```

> 注意，` PageHelper.startPage(pagenum,4);`是在mapper.xml的sql之后加上 `limit pageNum ;`不可以在原有的select sql语句中加上;否则会报sql语法错误


 使用上述方式快速搭建SSM环境的时候，需要注意扫描包路径、别名路径等都需要改，自行根据下方列出的地方进行检查。

1. MyBatis ：别名包路径。
2. Spring：包扫描路径(注意排除 Controller)、MapperScannerConfigurer中指定Mapper的扫描路径。
3. SpringMVC：Controller包扫描、视图解析器的prefix。











