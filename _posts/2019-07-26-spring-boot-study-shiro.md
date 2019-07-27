Apache Shiro 已经大名鼎鼎，搞 Java 的没有不知道的，这类似于 .Net 中的身份验证 form 认证。跟 .net core 中的认证授权策略基本是一样的。当然都不知道也没有关系，因为所有的权限都是模拟的人或机构的社会行为。

本系列从简单的权限讲起，主要涉及到 Shiro、Spring Security、Jwt、OAuth2.0及其他自定义权限策略。

本章主要讲解 Shiro 的基本原理与如何使用，本章主要用到以下基础设施：

- jdk1.8+
- spring boot 2.1.6
- idea 2018.1


# 1 Spring Boot 快速集成 Shiro 示例

首先我们来一段真实的代码演示下 Spring Boot 如何集成 Shiro 。本代码示例暂时没有使用到数据库相关知识，本代码主要使用到：
1. shiro
2. thymeeaf

## 1.1 新建 Spring Boot Maven 示例工程项目

1. File > New > Project，如下图选择 `Spring Initializr` 然后点击 【Next】下一步
2. 填写 `GroupId`（包名）、`Artifact`（项目名） 即可。点击 下一步
    groupId=com.fishpro   
    artifactId=shiro
3. 选择依赖 `Spring Web Starter` 前面打钩，勾选SQL选项的 Mybatis Framework , MySQL Driver
4. 项目名设置为 `spring-boot-study-shiro`.

## 1.2 依赖引入 Pom.xml
在Pom.xml中加入以下代码
```xml

```
## 1.3 配置 application.yml

## 1.4 自定义 Realm 领域 UserRealm 实现自定义认证与授权
在 src/main/java/com/java/fishpro/shiro/config（config是新增的包名） 新增 UserRealm.java 文件
UserRealm 是一种安全数据源，用户登录认证核心在此类中实现，用户授权也在此类中实现，具体看代码注释
```java

```
## 1.5 配置 Shiro 的 ShiroConfig
## 1.6 登录认证
## 1.7 Controller层方法授权
## 1.8 Thymeleaf 中授权
