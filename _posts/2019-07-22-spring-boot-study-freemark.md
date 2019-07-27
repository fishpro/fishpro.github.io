---
layout: post
title: Spring Boot FreeMarker 使用教程
categories: SpringBoot
description: Spring Boot FreeMarker 使用教程
keywords: SpringBoot, Spring, Date
---

FreeMarker 跟 Thymeleaf 一样，是一种模板引擎，他可以无缝兼容 FreeMarker 在 Spring Boot 开发者中仍然有着很高的地位。

本章重点内容
1. 编写一个最简单的 Freemark 模板示例
2. 简单说明 FreeMarker 


[本项目源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-freemarker)

# 1 FreeMarker 简介
相对于 Jsp ，FreeMarker 具有太多的优势。FreeMarker 适合 Mvc 场景。

FreeMarker 最大的特点就是具有可编程能力，可以对任何后台输出的数据做编程能力，这就像在 Java 中加入了 PHP 功能，这非常有趣。

FreeMarker 支持各类语法包括 字符输出、条件判断 if/else、循环遍历、

## 1.1 变量
```
${...}
```
## 1.2 条件语句
```
<#if condition>
    ...
<#elseif condition2>
    ...
<#elseif condition3>
    ...
<#else>
    ...
</#if>
```
## 1.3 循环语句
```
假设 users 包含['Joe', 'Kate', 'Fred'] 序列：
<#list users as user>
    <p>${user}
</#list>
 
输出：
    <p>Joe
    <p>Kate
    <p>Fred
```

## 1.4 include 包含语句
```
将版权信息单独存放在页面文件 copyright_footer.html 中：
<hr>
<i>
    Copyright (c) 2000 <a href="http://www.baidu.com">Baidu Inc</a>,
    <br>
    All Rights Reserved.
</i>
 
当需要用到这个文件时，可以使用 include 指令来插入：
<html>
    <head>
        <title>Test page</title>
    </head>
    <body>
        <h1>Test page</h1>
        <p>Blah blah...
        <#include "/copyright_footer.html">
    </body>
</html>
```



# 2 Spring Boot 中编写一个 FreeMarker 示例
本示例文件结构,新增了连个用于示例文件的文件 IndexController.java 与 index.ftl
```
+ java/fishpro/freemarker/controller
-- IndexController.java
+ resources/templates
--index.ftl
```



## 2.1 新建一个 Spring Boot 的 Maven 项
1. File > New > Project，如下图选择 `Spring Initializr` 然后点击 【Next】下一步
2. 填写 `GroupId`（包名）、`Artifact`（项目名） 即可。点击 下一步
    groupId=com.fishpro   
    artifactId=freemarker
3. 选择依赖 `Spring Web Starter` 前面打钩,在模板列中勾选 `apache freemarker`。
4. 项目名设置为 `spring-boot-study-freemarker`.

## 2.2 编辑 Pom.xml 引入依赖
注意如果在新建下面的时候没有引入 FreeMarker 那么就在这里复制粘贴上去。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.fishpro</groupId>
    <artifactId>freemarker</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>freemarker</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>
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

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

## 2.3 配置文件
注意 Spring Boot 中默认不需要做任何配置，示例程序把默认的端口 8080 改为了 8086，并把 application.properties 改为了 application.yml
```yml
server:
  port: 8086
```
## 2.4 编写示例代码

本示例文件结构,新增了连个用于示例文件的文件 IndexController.java 与 index.ftl
IndexController.java
```java
/**
 * 在是一个普通的 Controller 类
 * */
@Controller
public class IndexController {


    /**
     * 路由 /index
     * 返回 index 这里默认配置自动映射到 templages/index 
     * */
    @RequestMapping("/index")
    public String index(Model model){
        model.addAttribute("welcome","hello fishpro");
        return "index";
    }
}

```

index.ftl
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
this is welcome ${welcome}
</body>
</html>
```
## 2.5 运行示例
在浏览器中输入 http://localhost:8086/index 显示为，hello fishpro 就是后台输出的 model 对象
```
this is welcome hello fishpro
```


## 2.6 FreeMarker 实际应用场景
FreeMarker 比传统的 JSP 多出一个模板的作用，就是说，我们可以做多个不同样式的模板，动态的切换。这就是 FreeMarker 应用场景。


[本项目源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-freemarker)
