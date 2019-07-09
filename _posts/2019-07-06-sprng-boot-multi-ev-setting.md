---
layout: post
title: Spring Boot 多环境如何配置
categories: springboot
description:  Spring Boot 开发环境、测试环境、预生产环境、生产环境多环境配置
keywords: springboot,active
---


**目录**

* TOC
{:toc}


通常一个公司的应程序可能在开发环境（dev）、测试环境（test）、生产环境（prod）中运行。那么是不是需要拷贝不同的安装包，在不同的环境下运行呢，在 Spring Boot 中一切已经为我们准备就绪，只需要简单的配置，你的程序就能在不同的环境中运行。


# 一、Spring Boot 环境设置机制
`spring.profiles.active` 属性可以为我们指定当前设置的环境，以此来选择我们的配置文件。例如我们有配置文件
- application.yml
- application-dev.yml
- application-test.yml
- application-prod.yml

当执行 java -jar xxx.jar --spring.profiles.actvie=test 此时，系统将启用 `application.yml` 和 `application-test.yml` 配置文件。

当执行 java -jar xxx.jar --spring.profiles.actvie=prod 此时，系统将启用 `application.yml` 和 `application-prod.yml` 配置文件。

正是这种配置参数可以决定我们使用哪种配置文件，如果我们把不同环境的配置写在对应的配置文件中，我们就可以实现多环境机制。



# 二、配置多环境 
正如 第一 点所述，我们配置不同的配置文件
- application.yml
- application-dev.yml（开发环境）
- application-test.yml（测试环境）
- application-uat.yml（预发布环境）
- application-prod.yml（生产环境）

# 三、指定环境
## 1 在 cmd 命令中指定
```bash
java -jar xxx.jar --spring.profiles.actvie=dev 
```

## 2 在 application.yml 中指定
```yml
spring:
  profiles:
    active: dev
```

## 3 在IDEA 编辑器中指定
在运行按钮（绿色三角形按钮）旁边选择 `Edit Configurations...`，在弹出的对话框中 Active profiles 输入 dev 或其他即可。

**这种方法只有在本地调试的时候才生效。**

# 四、程序中获取 applicaton 中的值
```java
@Component
@ConfigurationProperties(prefix = "springstudy")
public class MultienvConfig {
    private String demoname;

    public String getDemoname() {
        return demoname;
    }

    public void setDemoname(String demoname) {
        this.demoname = demoname;
    }
}
```

# 五、程序示例
## 5.1 新建一个工程
- groupId=com.fishpro 
- artifactId=springstudy
- 项目名称 spring-boot-study-multienv
  
## 5.2 新增以下文件 
默认为 `application.properties` 直接重命名为 `application.yml`，其他三个新建就可以
- application.yml
```yml
server:
  port: 8081
spring:
  profiles:
    active: dev
```
- application-dev.yml（开发环境）
```yml
springstudy:
  demoname: multienv-dev
```
- application-test.yml（测试环境）
```yml
springstudy:
  demoname: multienv-test
```
- application-uat.yml（预发布环境）
```yml
springstudy:
  demoname: multienv-uat
```
- application-prod.yml（生产环境）
```yml
springstudy:
  demoname: multienv-prod
```

## 5.2 新建文件 fishpro.springstudy.MultienvConfig.java
```java
@Component
@ConfigurationProperties(prefix = "springstudy")
public class MultienvConfig {
    private String demoname;

    public String getDemoname() {
        return demoname;
    }

    public void setDemoname(String demoname) {
        this.demoname = demoname;
    }
}
```

## 5.3 新建 controller/IndexController.java
```java
@Controller
public class IndexController {
    private final Logger logger = LoggerFactory.getLogger(this.getClass());
    @Autowired
    MultienvConfig multienvConfig;
    @RequestMapping("/")
    String index(){
        logger.info(multienvConfig.getDemoname());
        return "index";
    }
}

```

## 5.4 运行 SpringstudyApplication.java
右键 `SpringstudyApplication.java` 执行 `Run SpringstudyApplication`  

运行程序 http://localhost:8081/ 在控制台中输出
```bash
2019-07-08 23:37:05.223  INFO 66267 --- [nio-8081-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2019-07-08 23:37:05.223  INFO 66267 --- [nio-8081-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2019-07-08 23:37:05.229  INFO 66267 --- [nio-8081-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 6 ms
2019-07-08 23:37:05.252  INFO 66267 --- [nio-8081-exec-1] c.f.s.controller.IndexController         : multienv-dev
```

修改 application.yml 中的配置
```yml
server:
  port: 8081
spring:
  profiles:
    active: prod
```
运行程序 http://localhost:8081/ 在控制台中输出
```bash
2019-07-08 23:42:44.906  INFO 66281 --- [nio-8081-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2019-07-08 23:42:44.910  INFO 66281 --- [nio-8081-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 4 ms
2019-07-08 23:42:44.930  INFO 66281 --- [nio-8081-exec-1] c.f.s.controller.IndexController         : multienv-prod
``` 