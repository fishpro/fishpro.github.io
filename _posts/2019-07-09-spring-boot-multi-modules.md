---
layout: post
title: IDEA 创建 Spring Boot 多模块项目（Multi Modules）
categories: springboot
description:  IDEA 创建 Spring Boot  多模块项目（Multi Modules）
keywords: springboot,idea
---

本准备写点代码实例放到网站，太多的模板，反而每次新建工程的时候很麻烦。于是准备把这个章节的内容提前先讲讲。正好把这个代码也管理起来。话说这个多模块功能还挺爽。

写过 C# 项目用过 Visual Studio 的人已经用惯了 一大把的项目放在一个解决方案中，下面我来实践一下 Java Spring Boot 的玩法。


**目录**

* TOC
{:toc}


[本项目源码下载](https://github.com/fishpro/spring-boot-study) 注意本示例源码只包括仓库中的 `spring-boot-study-helloworld-service` , `spring-boot-study-helloworld` ,`spring-boot-study-log`三个项目，其他项目为其他示例中项目。

本章演示的多模块之间的关系如下图：spring-boot-study-helloworld 依赖人 spring-boot-study-helloworld-service 调用其方法 message 输出字符串。
![项目关系图](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_modules_libs.png)

**本章节主要实现**
- 一个 maven 工程下多个模块项目按时
- 一个 maven 工程下各个项目的依赖关系
- 一个 maven 工程下可以有多个 web application 项目共存
  
# 1 建立根项目 Root Project
**使用 IDEA 创建一个 Maven 工程**

1) File>New>Project，如下图选择 maven ，注意 Create from archetype 不能打钩，然后点击 【Next】下一步。
![选择Maven项目](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_modules0.png)
2) 填写GroupId（包名）、Artifact（项目名） 即可。点击 【Next】下一步
- groupId=com.fishpro 
- artifactId=springstudy
3) 项目名设置为 spring-boot-study
4) 打开 pom.xml 编辑，增加 &lt;packaging&gt;pom&lt;/packaging&gt;
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.fishpro</groupId>
    <artifactId>springstudy</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
</project>

```
5) **删除 src 文件**
6) 注意如果您是创建的 spring 项目作为根项目，那么您需要**删除 src、mvnw、mvnw.cmd、HELP.md、.mvn 文件**
 
# 2 建立子项目
子项目可以是类库项目、Web应用项目
1. 应用模块项目 例如 Application 或 Web Application
2. 类库 Jar 模块项目，即 Library Jar



## 2.1 建立子Web Application 模块 spring-boot-study-helloworld
1. 右键项目名称 `spring-boot-study` > `New` > `Module` 进入项目模块新增页面
2. 因为我们这里是 Spring Boot 项目，那么我选择 `Spring Initializr` 选择【Next】
3. 填写 GroupId（包名）、Artifact（项目名） 即可。点击 【Next】下一步
- groupId=com.fishpro 
- artifactId=helloworld
4. **设置模块为 maven 中的子模块** 项目名设置为 `spring-boot-study-helloworld`，至此子项目已经添加完了，但你会发现，子项目不能允许调试，必须在根目录下的 pom.xml 配置此项目才行。
5. **设置父项目 pom.xml 加入模块 `spring-boot-study-helloworld`**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.fishpro</groupId>
    <artifactId>springstudy</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <modules>
        <module>spring-boot-study-helloworld</module>
    </modules>
</project>
```
6. 重命名`application.properties` 为 `application.yml` 增加端口设置，为了和前天模块不冲突
```yml
server:
  port: 8082
```

![设置父项目的pom](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_modules4.png)
7. **简单调试**，右键 com.fishpro.helloworld.HelloworldApplication > Run HelloworldApplication 子项目 Helloworld 就运行起来了。
![调试子模块项目](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_modules5.png)
**我们还可以使用相同的方式增加多个模块**

## 2.2 建立另一个 library jar 子模块项目 spring-boot-study-hellowrld-servie
1. 右键项目名称 spring-boot-study > New > Module 进入项目模块新增页面
2. 因为我们这里是 Spring Boot 项目，那么我选择 Spring Initializr 选择【Next】
3. 填写GroupId（包名）、Artifact（项目名） 即可。点击 【Next】下一步
- groupId=com.fishpro.helloworld 
- artifactId=service
4. **新建类 MyService**
```java
package com.fishpro.helloworld.service;
import org.springframework.stereotype.Service;
@Service
public class MyService {
    public String message(){
        return "this is module for helloworld.service method message";
    }
}
```
5. **编写测试类 test/myServiceTest**
```java
package com.fishpro.helloworld.service;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.test.context.junit4.SpringRunner;

import static org.assertj.core.api.Java6Assertions.assertThat;

@RunWith(SpringRunner.class)
public class MyServiceTest {
    @Autowired
    private  MyService myService;

    @Test
    public void contextLoads(){
        assertThat(myService.message()).isNotNull();
    }

    @SpringBootApplication
    static class TestConfiguration {
    }
}
```

## 2.3 建立模块之间的依赖关系
有的时候一个项目的 dao、service、controller 是分在不同的模块中，那么他们之间就有一些依赖关系，通常这些依赖关系是单向的。

本项目中项目 `spring-boot-study-hellowrld` 依赖项目 `spring-boot-study-helloworld-service` ，那么在 `spring-boot-study-hellowrld` 模块下 pom.xml 中设置

```xml
<dependency>
	<groupId>com.fishpro.helloworld</groupId>
	<artifactId>service</artifactId>
	<version>0.0.1-SNAPSHOT</version>
</dependency>
```


## 2.4 在 helloworld 模块中调用 helloworld.service 模块方法
我们在 helloworld 项目下新建 controller.IndexController
```java
package com.fishpro.helloworld.controller;

import com.fishpro.helloworld.service.MyService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class IndexController {
    @Autowired
    MyService myService;
    @GetMapping("/say")
    public String say(){
        return  myService.message();
    }
}
```

在浏览器中 输入 http://localhost:8082/say 显示 hellowrld 项目调用成功
>this is module for helloworld.service method message

## 2.4 建立另一个独立的 web application 子模块项目 spring-boot-study-log
我们再建立一个项目，项目名称 spring-boot-study-log 方法同第 2 节一样。最终效果图如下：
![多项目效果图](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_modules7.png)

至此，我们发现可以多个 web application 是可以共存的。所以我们后面的所有示例都是在这个项目中体现。

## 2.5 图形化管理模块界面
IDEA 中也可以通过图形化管理模块
在 File > Project Structrue 中管理项目的模块
![多模块项目管理](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_modules6.png)


*本项目中 `application.properties` 已经重新命名为 `application.yml`*


**总结**
1. 更项目 Root Project 是一个 Maven 项目，但没有 src 目录（手动删除）
2. 一个项目中可以增加多个模块
3. 模块可以说类库 Library，也可以是应用 Application 
4. 模块之间可以相互依赖，一般是单向依赖

---
关联阅读

- [Spring Boot 入门前的准备-安装 Java JDK]()
- [Spring Boot 入门前的准备-IntelliJ IDEA 开发工具的安装与使用]()
- [Spring Boot 学习 IDEA 下的 github 创建提交与修改]()