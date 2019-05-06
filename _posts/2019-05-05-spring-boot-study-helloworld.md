---
layout: post
title: Spring Boot 2.x 快速入门（上）HelloWorld示例
categories: SpringBoot
description: Spring Boot 2.x 快速入门（上）HelloWorld示例
keywords: SpringBoot, Spring, Thymeleaf
---

*最近决定重新实践下 Spring Boot 的知识体系，因为在项目中遇到的总是根据业务需求走的知识点，并不能覆盖 Spring Boot 完整的知识体系，甚至没有一个完整的实践去实践某个知识点。最好的学习实践就是写下来，供大家参考。*

如何使用 Spring Boot 2.x 快速创建 HelloWorld 项目，主要涉及到

1. 创建(生成）一个Spring Boot标准项目
2. 配置Pom.xml文件
3. 编写示例代码
4. 单元测试
5. 运行和调试
6. 打包发布


**目录**

* TOC
{:toc}

# 1 生成一个 Spring Boot 项目
这里我们介绍 在浏览器中实现一个 http://localhost:8999/hello/say web程序。这里使用IntelliJ IDEA 作为IDE环境来编译。也可以使用其他IDE。
## 1.1 使用 start.spring.io 创建项目
1. 打开https://start.spring.io/
2. 选择构建工具 Maven Project、Java、Spring Boot 版本 2.1.4 (注意这里文档版本是2.1.4，但在下面的实践中2.1.4本地的mvn有问题，后面换成了2.0.0) 、填写 Group、Articfact，本示例项目 GroupId=com.fishpro ArtifactId=springstudy。
3. 点击绿色按钮 Generate Project 生成项目，浏览器则自动下载项目，我命名的是 springhello，那么下载的是 springhello.zip。

## 1.2 使用IDEA 创建项目
使用IDEA创建项目，其实也是从 https://start.spring.io/ 创建，只是更为方便，我们一般采用从IDEA创建Spring Boot项目。

*注意mac和windows的IDEA创建过程是一样的。*
1. File>New>Project，如下图选择Spring Initializr 然后点击 Next 下一步。
2. 填写 GroupId（包名）、Artifact（项目名） 即可，GroupId=com.fishpro ArtifactId=springstudy。
3. 选择依赖，我们选择 Web

# 2 配置 pom.xml
注意如果生成项目的时候没有设置 dependencies，选择web，那么这里要在 pom.xml 中设置， pom.xml设置依赖也非常的简单，直接把 <dependency></dependency> 的节点拷贝到pom依赖节点中即可。

pom.xml属于maven项目结构的项目依赖项配置文件，主要管理第三方包的引用。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

*注意，如果IDEA没有自动导入，那么前往右下角，点击 Import Changes 按钮*


# 3 编写代码

## 3.1 Web 项目常用结构
在编写代码之前，我们应该了解常用的 Web 工程目录包括了 MVC 三层结构，他们分别是
1. 应用层 Controller 
2. 服务层 Service 
3. 数据层 Dao 或者 Respository 

## 3.2 编写 Controller 代码
1. 在本示例中，右键 springstudy 包名，新建包名 controller (注意一般是消息)
2. 在 controller 下新建 HelloWorldController.java (*注意首字母大写*)
3. 在 HelloWorldController 中增加 java 代码

```java
@RestController
@RequestMapping("/hello")
public class HelloWorldController {
    @RequestMapping("/say")
    public String say(){
        return "Hello World";
    }
}
```

## 3.3 更改 Web 端口
因我的系统端口默认8080倍nginx占领了，我把本次项目的启动端口改为8999

在 resources\application.properties 中设置（注意有的网络教程中是 application.yml 其实这是另一种配置文件格式，就想json和xml 只是格式不同，功能作用一样）
```bash
#设置端口号
server.port=8999
```

# 4 单元测试
测试代码一般是编写完一个功能方法后就编写对应的测试方法，以达到快速检测的效果，而不是全部编写完后，开始编写测试代码。

## 4.1 编写测试代码
测试代码在 src\test\java下面编写
1. 在本示例中，右键 src\test\java\com\fishpro\springstudy 包名，新建包名 controller (注意一般是消息)
2. 在 controller下新建HelloWorldControllerTests.java (注意对应于main下，一般后缀Tests)
3. 在 HelloWorldControllerTests 中增加 java 代码

```java
private MockMvc mockMvc;
    @Before
    public void setUp() throws Exception {
        mockMvc = MockMvcBuilders.standaloneSetup(new HelloWorldController()).build();
    }

    @Test
    public void getHello() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders.get("/hello/say").accept(MediaType.APPLICATION_JSON))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andDo(MockMvcResultHandlers.print())
                .andReturn();
    }
```
## 4.2 运行测试用例
右键HelloWorldControllerTests.java 选择Run 'HelloWorldControllerTests' with Coverage


# 5 运行和调试
点击右上角，**绿色**运行三角形按钮，启动运行，或点击它旁边的爬虫按钮，进行**调试**。

或者点击菜单

1. Run>Run 'SpringstudyApplication' 进行运行示例
2. Run>Debug 'SpringstudyApplication' 进行调试示例，可设置断点

在浏览器输入 http://localhost:8999/hello/say 查看输出结果


# 6 打包发布
通常我们一 jar 方式打包发布，war 方式用于单独的发布到已有的 tomcat web 服务器中，以后的实践中再讲。

1. 选择 View> Tool Windows>Terminal
2. 输入命令
```bash
mvn clean
mvn install　
```
在根目录下有个target 文件夹，有个 springstudy-0.0.1-SNAPSHOT.jar 

3. 模拟服务器环境，运行jar文件，输入命令，后则可以在浏览器中得到结果。
```bash
java -jar springstudy-0.0.1-SNAPSHOT.jar
```
4. 在浏览器输入 http://localhost:8999/hello/say 查看输出结果

# 7 常见问题
<font color="SandyBrown">Q. Failed to read artifact descriptor for org.springframework.boot:spring-boot-starter-web:jar:2.1.4.RELEASE less... (⌘F1) 
Inspects a Maven model for resolution problems.</font>

A.未能加载spring-boot-starter-web:jar，这个应该是mvn管理器加载问题。

<font color="SandyBrown">Q. 端口问题</font>

A. 默认是8080端口，如果端口被占用了（例如mac的nginx默认是8080），需要修改，那么在 resources\application.properties 中设置