---
layout: post
title: SpringBoot入门
categories: study
tags : Java
toc: true
---


# 创建SpringBoot项目
选择maven或者Springboot项目
Spring 项目会自动引入
maven
pom.xml
```xml
  <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.6</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
        <groupId>com.demo</groupId>
        <artifactId>demo1</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>demo1</name>
        <description>demo1</description>
        <properties>
            <java.version>11</java.version>
        </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### control
视图
```java

package com.demo.demo1.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/hello")
public class demoController {
    @GetMapping("/demo1")
    public String d1(){
        return "helloworld,demo1";
    }
    @GetMapping("demo2/{id}/{name}")//不同类型的参数
    public String d2(@PathVariable("id") String id,@PathVariable("name") String name){
        return "demo2"+id+name;
    }
    @PostMapping("/kan")//post类型

}



```
# 后端的类
后端每个API都是可以出现并发场景的，即一个API可能被多个应用同时访问，并且这个访问量可能会变得很大。不像APP、web等客户端程序，虽然会遇到并发问题，但不是核心。

基于性能的考虑，如果频繁创建的大的对象，由于对象占用的是堆内存空间，而堆内存空间正是垃圾回收器的主要处理点。在并发数高的场景下，会导致频繁执行全量垃圾回收操作，即内存频繁出现大块的搬运处理，最终表现就是API响应卡顿或者内存溢出导致服务器挂掉。


正是由于存在上述的特殊性，使得后端程序在设计时会优先考虑避免大对象的频繁创建，也就是最好只创建一次，通过复用该对象实现业务处理（这里，大和频繁是两个重点）。那么这一方案引出了两个关键点：

大的对象本身不能保存状态，否则会出现并发数据共享的问题；如果加锁处理，会严重影响程序性能（并发数高的情况下）。
针对这类对象，保证运行期间只创建一次，即类似于单例模式实现，这点Spring已经提供支持，这也是为什么Spring托管的对象默认都是单例的。

这也就是为什么目前开发后端项目时，开发人员所熟知的Controller、Service、Dao这三层结构的类，他们的实现类都只创建一个对象，并且不保存状态（即不存在保存数值的成员变量）的一个重要原因。

至于这三层结构的划分，则是基于后续体量大的一种考虑方案，并不是最优解，但是最常见的分层。

## SptringBoot配置解析
SpringBoot是基于约定的，所以很多配置都有默认值，但如果想使用自己的配置替换默认配置的话，就可以使用application.properties或者application.yml（application.yaml）进行配置
同一目录之下，优先级`.properties>.yml>.yaml`
.properties
```properties
server.port=3000
server.address=/test
```
.yml    .yaml

```yml
server:
  port: 5000
  address: /test
```
### yml
#### yml语法
- 大小写敏感
- 数据值前边必须有空格，作为分隔符
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格（各个系统 Tab对应的空格数目可能不同，导致层次混乱）。
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- ''#" 表示注释，从这个字符一直到行尾，都会被解析器忽略
#### yml数据
对象(map)：键值对的集合。
```yml
person:
  name: tom
```

行内写法
person: {name: tom}
数组：一组按次序排列的值
```yml
address:
   - beijing
   - shanghai
```

行内写法
address: [beijing,shanghai]
纯量：单个的、不可再分的值
```yml
msg1: 'hello \n world'  # 单引忽略转义字符
msg2: "hello \n world"  # 双引识别转义字符
```


参数引用
```yml
name: itlils
person: 
  name: ${name}
```
### 访问yml数据
```yml
name1: tony
person:
  name: tom
```
1 @Value
```java
import org.springframework.beans.factory.annotation.Value;

@Value("${name1}")
String name1;
@Value("${person.name}")
String name;
System.out.println(name);
@Value("${address[0]}")
String a;
```

2Environment
```java
@Autowired
private Environment env;

System.out.println(env.getProperty("person.name"));

System.out.println(env.getProperty("address[0]"));

```
3 @ConfigurationProperties
```yml
person:
  name: ttony
  age: 10
  address:
    - wuhan
    - qingdao
    - hangzhou
```
```java
@Component
@ConfigurationProperties(prefix ="person")
public class Person {
    String name;
    int age;
    String[] address;
//   setter and getter
    @Override
    @Override
    public String toString(){
        return "person{"+
                "name"+name+
                "address"+ Arrays.toString(address)+
                "age"+age+
                "}"
                ;
    } 
}

```

```java
import org.springframework.beans.factory.annotation.Autowired;

@Autowired
Person person;


System.out.println(person);
System.out.println(person.toString());
```
## 多环境配置
为不同环境配置不同的环境对象
```yaml
---
server:
  port: 8081
spring:
  profiles: dev
---
server:
  port: 8082
spring:
  profiles: pro
---
server:
  port: 8083
spring:
  profiles: test
---
spring:
  profiles:
    active: dev
```
真实开发中，新建application-dev.yml,application-test.yml
在application.yml中
```yml
spring:
  profiles:
    active: test
```

marven中 package打包成jar包在target中
```shell
java -jar xxxx.jar

java -jar xxxx.jar --spring.profiles.active=dev
```


虚拟机参数：在VM options 指定：-Dspring.profiles.active=pro

命令行参数： --spring.profiles.active=dev

相当于上线时，运行jar包：java -jar xxx.jar --spring.profiles.active=dev

测试：使用maven 打包此项目，在target包中出现xxxx.jar

### 加载顺序
内部配置文件
加载顺序为下文的排列顺序，高优先级配置的属性会生效

- file:./config/：当前项目下的/config目录下,父工程

- file:./ ：当前项目的根目录

- classpath:/config/：classpath的/config目录

- classpath:/ ：classpath的根目录（../src/maini/resource）
外部配置文件
命令行
```shell
java -jar myproject.jar --spring.config.location=d://application.properties
java -jar app.jar --name="Spring“ --server.port=9000
```

# 整合junit
JUnit 是一个Java 编程语言的单元测试框架。
```xml
<!--引入starter-test起步依赖-->

 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

```java


import com.demo.demo1.Demo1Application;

@SpringBootTest(classes = Demo1Application.class)
@RunWith(SpringRunner.class)
public class UserServiceTest {
    @Autowired
    UserService userService;

    @Test
    public void testAdd() {
        userService.add();
    }
}
```
# 数据库的连接
## jdbc
```xml
 <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
</dependency>
```
```java
package com.demo.demo1;
//import com.mysql.jdbc.Driver;
import com.mysql.cj.jdbc.Driver;
import java.sql.*;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;



//import static com.sun.tools.classfile.Attribute.Exceptions;

@SpringBootApplication
public class Demo1Application {
    private static void jdbcTest()throws  ClassNotFoundException, SQLException  {
        // 将JDBC驱动加载到DriverManager
//        Class.forName("com.mysql.jdbc.Driver");
        //建议
        Class.forName("com.mysql.cj.jdbc.Driver");

        // 从DriverManager处获取数据库连接
        Connection conn = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/test",
                "root",
                "123456" );

        // The object used for executing a static SQL statement and returning the results it produces. 也就是创建出执行sql语句的对象
        Statement st = conn.createStatement();

        // 只查找一行数据
        ResultSet rs = st.executeQuery("SELECT user_id, user_name, password FROM user LIMIT 0,3");

        User user = new User();
        // 遍历每一行数据
        while (rs.next()) {
            System.out.println(rs.getString("user_id"));
            System.out.println(rs.getString("user_name"));
            System.out.println(rs.getString("password"));
            user.setUser_id(rs.getString("user_id"));
            user.setUser_name(rs.getString("user_name"));
            user.setPassword(rs.getString("password"));
        }

        System.out.println(user.toString());

        rs.close();
        st.close();
        conn.close();
    }
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        jdbcTest();
        SpringApplication.run(Demo1Application.class, args);

    }

}


```


## mybatis
1. 引入依赖
```xml
<dependencies>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.0</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <!--<scope>runtime</scope>-->
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
</dependencies>
```
2. 定义表和实体类
3. 添加yml配置
```yaml
spring:
    datasource:
      url: jdbc:mysql://127.0.0.1:3306/test?serverTimezone=UTC
      username: boss
      password: 123456
      driver-class-name: com.mysql.jdbc.Driver
#server:
```
两种使用方式，注解方式、xml方式
### 注解方式
User.java 类
```java
public class User{
    private String id;
   // ...
   // setter And getter;
    @Override
    public String toString(){
        return "id : "+user_id+"   "+
                "name : "+user_name+"   "+
                "password : "+password+"   "+
                "\n"
                ;
    }
}
```
Mapper.UserMapper.java 接口
```java
package com.demo.demo1.mapper;

import com.demo.demo1.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

import java.util.List;
@Mapper
public interface  UserMapperr{
    @Select("select * from user")
    public List<User> findAll();
}
```
调用test.java 或者

```java
import com.demo.demo1.mapper.UserMapper;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.List;

@SpringBootTest(classes = Demo1Application.class)
@RunWith(SpringRunner.class)
public class UserTest {
    @Autowired
    UserMapper user;
    @Test
    public void findAllTest(){
        List<User> userlist=user.findAll();
        System.out.println(userlist);
    }
}

```


### xml配置方式
在resource.mapper.xxxx.xml
```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.demo.demo1.mapper.UserXmlMapper">
<!--    接口类-->
    <select id="findAll" resultType="com.demo.demo1.User">
<!--        接口函数和返回类型-->
        select * from user
    </select>
</mapper>
```

在com.demo.demo1.mapper.xmlmapper.java中
```java
package com.demo.demo1.mapper;

import com.demo.demo1.User;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

@Mapper
public interface UserXmlMapper {
    public List<User> findAll();
}

```
调用test.java中
```java
    @Autowired
    UserXmlMapper xmluser;
    @Test
    public void findAllXmlTest(){
        List<User> userlist=xmluser.findAll();
        System.out.println(userlist);
    }
```
## redis

