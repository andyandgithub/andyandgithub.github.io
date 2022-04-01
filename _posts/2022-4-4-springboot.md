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

# 数据库的连接
## jdbc
```xml
 <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
</dependency>
```


