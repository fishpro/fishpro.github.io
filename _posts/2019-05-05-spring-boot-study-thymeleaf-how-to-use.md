---
layout: post
title: Spring Boot 2.x教程-如何使用 Thymeleaf
categories: SpringBoot
description: Spring Boot 2.x教程-如何使用 Thymeleaf
keywords: SpringBoot, Spring, Thymeleaf
---

**Thymeleaf知识分三步讲解：**
1. 如何使用 Thymeleaf
2. Thymeleaf 原理是什么
3. Thymeleaf 全攻略示例讲解

本章是第一章节讲解如何在 Spring Boot 中使用 Themealf.

[源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-thymeleaf)

**目录**

* TOC
{:toc}

# 1 引入依赖
打开根目录下的文件 pom.xml dependencies 节点加入以下代码
``` xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

# 2 配置Thymeleaf
找到src\main\resources\application.yml，如果是application.properties更名后缀yml即可,当然习惯使用 properties 后缀的则不需要更改。
注意这里的配置不是必须的，不配做，thymeleaf则有默认的配置。
```yml
#thymelea模板配置
spring:
  thymeleaf:
    #thymeleaf 所在路径
    prefix: classpath:/templates/
    #thymeleaf 后缀
    suffix: .html
    #thymeleaf 采用的标准
    mode: HTML5
    #thymeleaf 编码格式
    encoding: UTF-8
```
application.properties 后缀格式 表示为 ``` spring.thymeleaf.prefix=classpath:/templates/``` 其他类似修改。

# 3 编写代码实例
## 3.1 项目结构
在编写代码之前应该搞清楚 thymeleaf 结构。
src\main\resources\templates 为目录的 thymeleaf 模板存放路径

## 3.2 数据准备
1. 新建Java文件 src\main\...\controller\DemoController.java  
在 DemoController 增加展示层数据的输出

```java
 @RequestMapping("/simple")
    public String simpleDemo(Model model){
        model.addAttribute("user",getUserDTOData());
        List<UserDTO> users=new ArrayList<>();
        users.add(new UserDTO("zhangsan","男",new Date()));
        users.add(new UserDTO("wangjingjing","女",new Date()));
        users.add(new UserDTO("limeimei","女",new Date()));
        users.add(new UserDTO("lisi","男",new Date()));
        model.addAttribute("users",users);
        return "/demo/simple";
    }
```

2. 新建模板文件src\main\resources\templatesdemo\simple.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Thymeleaf简单示例</title>
</head>
<body>
<p style="background-color: #c7ddef">
    ${...} 可以显示后台传输的变量
</p>
<p th:text="${user.username}"></p>
<p style="background-color: #c7ddef">
    th:each循环标签
</p>
<p th:each=" userobject : ${users}">
    <span th:text="${userobject.username}"></span>
</p>
</body>
</html>
```


# 4 运行实例
在浏览器中输入 http://localhost:8080/demo/simple



---

欢迎关注我的微信公众号,我们一起编程聊天看世界
 
<div align="center"><img width="192px" height="192px" src="http://www.fishpro.com.cn/assets/images/qrcode.jpg"/></div>

