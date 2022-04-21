---
layout: post
title: SpringBoot入门
categories: study
tags : java
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

# SptringBoot配置解析
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
## yml
### yml语法
- 大小写敏感
- 数据值前边必须有空格，作为分隔符
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格（各个系统 Tab对应的空格数目可能不同，导致层次混乱）。
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- ''#" 表示注释，从这个字符一直到行尾，都会被解析器忽略
### yml数据
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
# 前后端交互
## 返回Json数据

在 Controller 类上面用 @RestController 定义或者在方法上面用 @ResponseBody 定义，表明是在 Body 区域输出数据。


```java
public Users getUser(){
    user=...
    return user;
}
```
## 获取传递的参数
```java
@PostMapping("/login")
    public String login(@RequestParam("username") String username,
                        @RequestParam("password") String password){


//        Query query=new Query(Criteria.where("username").is("admin"));
        System.out.println(username+password);
        return "login"+username+password;
}
```
@PathVaribale 获取url中的数据
@RequestParam 获取请求参数的值
@GetMapping 组合注解，是 @RequestMapping(method = RequestMethod.GET) 的缩写
@RequestBody 利用一个对象去获取前端传过来的数

## 文件传递和展示
### 后端路由直接展示图片
需要一个photoConfig.java
```java
package com.admin.react.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class PhotoConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        //和页面有关的静态目录都放在项目的static目录下
//        registry.addResourceHandler("/static/**").addResourceLocations("classpath:/static/");
        //上传的图片在D盘下的OTA目录下，访问路径如：http://localhost:8081/OTA/d3cf0281-bb7f-40e0-ab77-406db95ccf2c.jpg
        //其中OTA表示访问的前缀。"file:D:/OTA/"是文件真实的存储路径
        registry.addResourceHandler("/manage/photo/**").addResourceLocations("file:"+System.getProperty("user.dir")+"\\src\\main\\resources\\static\\");
//        registry.addResourceHandler("/manage/photo/**").addResourceLocations("file:D:\\Files\\Project\\JavaProject\\RASBoot\\src\\main\\resources\\static\\");
    }
}
```

### 运行文件的当前目录
```java
System.getProperty("user.dir");
```
### 文件存在、删除
```java
if(file.exists()&&file.delete())
        ...
```
### 上传图片
```java
 @PostMapping("/upload")
    public ResultData<ImageUpload>uploadImage(
             @RequestParam MultipartFile image
    ) throws IOException {

        String fileName=image.getOriginalFilename();
        String uuid = UUID.randomUUID().toString().replace("-", "");

        ImageUpload imageUpload=new ImageUpload();
        try{
        String fileType = fileName.substring(fileName.lastIndexOf("."));
        String imageFilePath = System.getProperty("user.dir")+"\\src\\main\\resources\\static\\" + uuid + fileType;
        image.transferTo(new File(imageFilePath));
        imageUpload.setName(uuid+fileType);
        imageUpload.setUrl("http://localhost:5000/manage/photo/"+imageUpload.getName());
        return ResultData.success(imageUpload);
        }catch(Exception e){
            return ResultData.fail(imageUpload);
        }

    }
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
## jpa
### 配置
```yml
spring:
    datasource:
      url: jdbc:mysql://127.0.0.1:3306/test?serverTimezone=UTC
      username: boss
      password: 123456
      driver-class-name: com.mysql.jdbc.Driver
#server:
  jpa:
    show-sql: true  #s输出时候输出sql语句
    properties: 
      hibernate:
        format_sql: true  # 输出多个对象时候换行‘
```
### 基本使用
entity/user.java
字段与数据库保持一致
```java
@Entity
@Data //lombook的注解
public class User{
    @Id
    private int id;
    private String name;
    private String author;
}
```
repository/UserrePository.class之下
```java
public UserRepository extends JpaRepository<User,int>{

}
```

使用
```java
private UserRepository userRepository;
userRepository.findAll();
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
启动redis,进入到安装目录之下
```shell
redis-server.exe redis.windows.conf
# 新建一个cmd
redis-cli.exe -h 127.0.0.1 -p 6379 
```
xml导入依赖
```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
yml配置
```yaml
spring:
    redis:
        host: localhost
        port: 6376
#server:
```
```java

package com.demo.demo1;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest(classes=Demo1Application.class)
@RunWith(SpringRunner.class)
public class redisTest {
    @Autowired
    RedisTemplate redisTemplate;
    @Test
    public void test(){
        redisTemplate.opsForValue().set("a","b");
        redisTemplate.boundValueOps("c").set("d");

    }
    @Test
    public void testRedis2(){
        Object b=redisTemplate.opsForValue().get("a");
        System.out.println(b);
        Object c=redisTemplate.boundValueOps("c").get();
        System.out.println(c);
    }
}

```
## mongoDb
```xml
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-mongodb</artifactId>
    <version>2.1.3.RELEASE</version>
</dependency>
```
### 查询
#### 条件查询
```java
    @PostMapping("test1")
    public ResultData<Users> test2(){
        Query query=new Query(Criteria.where("username").is("admin"));

        Users a=mongoTemplate.findOne(query, Users.class);
        List<Users> ba=mongoTemplate.findAll(Users.class);
        return ResultData.success(a);
    }
```
####  多条件查询
```java

Query query = new Query(Criteria.where("user").is("admin").and("age").is(18));
Usrrs result = mongoTemplate.findOne(query, Users.class, COLLECTION_NAME);
System.out.println("query: " + query + " | andQuery: " + result);
```

```java
 @PostMapping("/login")

    public ResultData<Users> login(
            @RequestBody Users user
    ){
    
        System.out.println("login");
        Query query=new Query();
        query.addCriteria(Criteria.where("username").is(user.getUsername()));
        query.addCriteria(Criteria.where("password").is(user.getPassword()));
        System.out.println(user);
        Users u=mongoTemplate.findOne(query,Users.class);
        if(u!=null){
            u.setRole(mongoTemplate.findOne(new Query(Criteria.where("_id").is(u.getRole_id())), Roles.class));
            System.out.println(ResultData.success(u));
            return ResultData.success(u);
        }else{
            System.out.println(ResultData.fail(u));
            return ResultData.fail(u);
        }
    }
```
####分页查询

```java
/**
 * 分页查询
 */
public void pageQuery() {
    // limit限定查询2条
    Query query = Query.query(Criteria.where("user").is("一灰灰blog")).with(Sort.by("age")).limit(2);
    List<Map> result = mongoTemplate.find(query, Map.class, COLLECTION_NAME);
    System.out.println("query: " + query + " | limitPageQuery " + result);


    // skip()方法来跳过指定数量的数据
    query = Query.query(Criteria.where("user").is("一灰灰blog")).with(Sort.by("age")).skip(2);
    result = mongoTemplate.find(query, Map.class, COLLECTION_NAME);
    Syste
```
#### or或查询
```java
Query query = new Query(Criteria.where("user").is("admin")
            .orOperator(Criteria.where("age").is(18), Criteria.where("sign").exists(true)));
List<Map> result = mongoTemplate.find(query, Map.class, COLLECTION_NAME);
```
#### in 查询
```java
Query query = new Query(Criteria.where("age").in(Arrays.asList(18, 20, 30)));

```

#### 数字比较
```java
// age > 18
Query query = new Query(Criteria.where("age").gt(18));
// age >= 18
query = new Query(Criteria.where("age").gte(18));
// age < 20
Query query = new Query(Criteria.where("age").lt(20));
// age <= 20
query = new Query(Criteria.where("age").lte(20));
```

#### 正则查询
```java
 // age <= 20
query = new Query(Criteria.where("age").lte(20));
```
#### 查询总数
```java
Query query = new Query(Criteria.where("user").is("admin"));
long cnt = mongoTemplate.count(query, COLLECTION_NAME);
```
#### 分组查询
```java
Aggregation aggregation = Aggregation.newAggregation(Aggregation.group("user").count().as("userCount"));
AggregationResults<Map> ans = mongoTemplate.aggregate(aggregation, COLLECTION_NAME, Map.class);
System.out.println("query: " + aggregation + " | groupQuery " + ans.getMappedResults());
```

#### 排序
```java
Query query = Query.query(Criteria.where("user").is("一灰灰blog")).with(Sort.by("age"));
```
### 插入
```java
mongoTemplate.insert(product);
Person insert = mongoTemplate.insert(person);
```

```java
mongoTemplate.save(user);
mongoTemplate.insert(user);
//  根据集合名称保存对象到mongodb
mongoTemplate.save(user,"mongodb_user");
mongoTemplate.insert(user,"mongodb_user");
//  根据集合名称保存list到mongodb
mongoTemplate.save(list,"mongodb_user");
mongoTemplate.insert(list,"mongodb_user");
mongoTemplate.insert(list,User.class);
```
### 更新
```java
Query query = Query.query(Criteria.where("_id").is("5d1312aeb1829c279c6c256b"));
Update update = Update.update("name","zs");
        update.set("age", 12);

//  更新一条数据
mongoTemplate.updateFirst(query,update, User.class);
mongoTemplate.updateFirst(query,update, "mongodb_user");
mongoTemplate.updateFirst(query,update, User.class,"mongodb_user");
//  更新多条数据
mongoTemplate.updateMulti(query,update, User.class);
mongoTemplate.updateMulti(query,update,"mongodb_user");
mongoTemplate.updateMulti(query,update, User.class,"mongodb_user");
//  更新数据，如果数据不存在就新增
mongoTemplate.upsert(query,update, User.class);
mongoTemplate.upsert(query,update,"mongodb_user");
mongoTemplate.upsert(query,update, User.class,"mongodb_user")
```

```java
Query query = Query.query(Criteria.where("_id").is(productId));
Update update = Update.update("status",productStatus);
update.set("imgs",product.getImgs());
update.set("name",product.getName());
mongoTemplate.updateFirst(query,update,Products.class);
UpdateResult updateResult = mongoTemplate.updateFirst(Query.query(Criteria.where("name").is(name)),Update.update("age", age), Person.class);
long modifiedCount = updateResult.getModifiedCount();
        
```
### 删除
```java
Query query = Query.query(Criteria.where("_id").in("5d1312aeb1829c279c6c256b","5d13133ab1829c29d02ce29c"));
//  根据条件删除
mongoTemplate.remove(query,Users.class);
mongoTemplate.remove(user);
```
# 解决跨域问题
config/CrosConfig.class
```java
@Configruation
public class CrosConfig  implements WebMvcConfig{
    @Override
    ! public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS")
                .allowCredentials(true)
                .maxAge(3600)
                .allowedHeaders("*");
    }
}
```
# 自动配置
## condition
SpringBoot 提供的常用条件注解：

- ConditionalOnProperty：判断配置文件中是否有对应属性和值才初始化Bean

- ConditionalOnClass：判断环境中是否有对应字节码文件才初始化Bean

- ConditionalOnMissingBean：判断环境中没有对应Bean才初始化Bean
## 切换内置服务器
tomcat(默认),jetty,netty
pom文件中的排除依赖效果
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!--排除tomcat依赖-->
    <exclusions>
        <exclusion>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <groupId>org.springframework.boot</groupId>
        </exclusion>
    </exclusions>
</dependency>

<!--引入jetty的依赖-->
<dependency>
    <artifactId>spring-boot-starter-jetty</artifactId>
    <groupId>org.springframework.boot</groupId>
</dependency>
```
> 问你：为什么引入了starter-data-redis，我们就能项目中，直接拿redistemplate?

> springboot中的autoconfig工程里把常用的对象的配置类都有了，只要工程中，引入了相关起步依赖，这些对象在我们本项目的容器中就有了。

## 引入第三方jar包

三种解决方案：

1. 使用@ComponentScan扫描com.ydlclass.config包

2. 可以使用@Import注解，加载类。这些类都会被Spring创建，并放入IOC容器

3. 可以对Import注解进行封装。

重点：Enable注解底层原理是使用@Import注解实现Bean的动态加载

重要：springbootapplication 由三个注解组成

@SpringBootConfiguration 自动配置相关
@EnableAutoConfiguration
@ComponentScan 扫本包及子包


pom中引入springboot-enable-other
```xml

 	<dependency>
          <groupId>com.ydlclass</groupId>
          <artifactId>springboot-enable-other</artifactId>
          <version>0.0.1-SNAPSHOT</version>
    </dependency>
```
## import用法
@Enable底层依赖于@Import注解导入一些类，使用@Import导入的类会被Spring加载到IOC容器中。而@Import提供4中用法：

①导入Bean。注意bean名字是全限定名
```java
@Import(User.class)

```
②导入配置类

③导入 ImportSelector 实现类。一般用于加载配置文件中的类

④导入 ImportBeanDefinitionRegistrar 实现类。

导入Bean @Import(User.class)

导入配置类 @Import(UserConfig.class)

导入 ImportSelector 实现类 @Import(MyImportSelector.class)

MyImportSelector

# 打包上线
maven -package打包
在target里面的jar包
```shell
java -jar xxxx.jar
```
更改最终的名字
pom.xml中的
```xml
<build>
    <finalName>name</finalName>
</build>
```

