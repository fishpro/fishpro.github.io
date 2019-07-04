---
layout: post
title: Spring Boot 2.0 新特性
categories: springboot
description: Spring Boot 2.0 新特性
keywords: springboot,idea
---


**目录**

* TOC
{:toc}

*这是一篇总结文章，主要收集 Spring Boot 2.0 相对于 Spring Boot 1.x 的新特性，本章节并不提供实践性质的源代码。在 Spring Boot 系列文章中会持续退出实践章节。*

# 从 Spring Boot 1.5 开始升级
如果要从 Spring Boot 1.5 升级到 Spring Boot 2.0 可能要花费一些实际，可以参考 [官方迁移指南](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide) ，[中文版本迁移指南](https://www.oschina.net/translate/spring-boot-2-0-migration-guide)

如果是更老的版本，请参考[Upgrading from Spring Boot 1.4 迁移指南](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-1.5-Release-Notes)

当然，你可以从谷歌或百度搜索到中文翻译版本。

本文主要来源于 [spring-boot-2.0-release-notes](https://github.com/spring-projects/spring-boot/wiki/spring-boot-2.0-release-notes)

# 基于 Java 8 支持 Java 9
Spring Boot 2.0 要求 Java 的最低版本是 Java 8，从 Spring Boot 2.0 版本开始许多 APIs 使用了 Java 8 的特性。当然你也可以选择 Java 9。

Spring Boot 2.0 不在支持 Java 6 和 Java 7。

# 第三方依赖库
首先 Spring Boot 2.0 是基于 Spring 5 的。那么所有的 [Spring 5 的新特性](https://github.com/spring-projects/spring-framework/wiki/What%27s-New-in-Spring-Framework-5.x) 在 Spring Boot 2.0 中都有体现。

其中主要的依赖变更包括
1. Tomcat 8.5
2. Flyway 5
3. Hibernate 5.2
4. Thymeleaf 3

# Reactive Spring
Spring Boot 2.0 支持响应式编程

# Spring WebFlux & WebFlux.fn
Spring Boot 2.0 提供基于注释的 Spring WebFlux 以及 WebFlux.fn的自动装配。在启动的时候，使用 `spring-boot-starter-webflux` 来启动项目。


# Reactive Spring Data
在底层技术支持的情况下，Spring Data也为reactive applications提供支持。目前Cassandra，MongoDB，Couchbase和Redis都有响应式API支持。

Spring Boot可为您提供所有针对以上技术的不同 starter-POMs。例如，·spring-boot-starter-data-mongodb-reactive· 包含了所有对响应式mongo的相关驱动依赖。

# Reactive Spring Security
Spring Boot 2.0支持集成Spring Security 5.0。为WebFlux程序提供Spring Security的自动配置。

使用 WebFlux 的 Spring Security 访问规则可以通过 ·SecurityWebFilterChain· 来自动配置。如果你之前使用过Spring MVC，将会感到非常熟悉。有关更多详细信息，请参阅[Spring Boot参考文档](https://docs.spring.io/spring-boot/docs/2.0.x-SNAPSHOT/reference/htmlsingle/#boot-features-security-webflux)和[Spring Security文档](https://docs.spring.io/spring-security/site/docs/5.0.0.RELEASE/reference/htmlsingle/#jc-webflux)。

# Embedded Netty Server 嵌入式 Netty 服务器
由于WebFlux不依赖于Servlet API，现在首次支持Netty作为嵌入式Server。该·spring-boot-starter-webflux· starter POM 将引入 Netty 4.1 和 Ractor Netty。


# HTTP/2 支持
现在Tomcat，Undertow和Jetty都已经提供对HTTP / 2的支持。但是这部分取决于所选的Web服务器和应用程序环境（因为JDK 8不支持该协议）。

# Configuration Property Binding 配置属性的绑定
在 Spring Boot 2.0 中，已经彻底修改了用于绑定Environment属性的@ConfigurationProperties 机制。我们借此机会收紧了松散的绑定规则，并修复了 Spring Boot 1.x 版本中许多不一致的地方。

通过新的 Binder API 可以在您的代码中直接使用 @ConfigurationProperties 。例如，下面的示例将实现绑定PersonName到List对象：
```java
List<PersonName> people = Binder.get(environment)
    .bind("my.property", Bindable.listOf(PersonName.class))
    .orElseThrow(IllegalStateException::new);
``` 
在YAML中配置源可以像这样表示：
```bash
my:
  property:
  - first-name: Jane
    last-name: Doe
  - first-name: John
    last-name: Doe
```
有关更新绑定规则的更多信息，请参阅此[Wiki页面](https://github.com/spring-projects/spring-boot/wiki/Relaxed-Binding-2.0)。


# Property Origins 配置起源
YAML 文件和 Properties 文件现在都包含 Origin 信息，从而可帮助更好的跟踪项目加载情况。有一些 Spring Boot 特性可以利用这些信息，并在适当时用于展示。

例如，BindException 类绑定失败时抛出的OriginProvider。这意味着origin信息可以很好地从故障分析器中显示出来。

另一个例子是env 可用 actuator端点时其包括的origin信息。下面的代码显示的是通过 spring.security.user.name属性，得知application.properties文件来自jar包下行1，列27。
```json
{
  "name": "applicationConfig: [classpath:/application.properties]",
  "properties": {
    "spring.security.user.name": {
      "value": "user",
      "origin": "class path resource [application.properties]:1:27"
    }
  }
}
```
 

# Converter Support 转换器支持
使用新的 `ApplicationConversionService` 类的绑定器，提供了一些对属性绑定特别有用的额外转换器。最引人注目的是 `Duration` 和分隔字符串类型的转换器。

# Gradle 插件
Spring Boot 的 Gradle 插件已经在很大程度上进行了重新编写，以实现[许多重大改进](https://github.com/spring-projects/spring-boot/issues?utf8=%E2%9C%93&q=label%3A%22theme%3A%20gradle-plugin%22%20milestone%3A2.0.0.M1%20)。您可以在其[参考文献](https://docs.spring.io/spring-boot/docs/2.0.x-SNAPSHOT/gradle-plugin/reference/)和 [API](https://docs.spring.io/spring-boot/docs/2.0.0.BUILD-SNAPSHOT/gradle-plugin/api/) 文档中阅读关于插件功能的更多信息。

Spring Boot 现在要求基于 Gradle 4.x. 如果您要升级使用 Gradle 版本，请查看[迁移指南](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)。

# Kotlin
Spring Boot 2.0现在支持Kotlin 1.2.x，并提供了一种 `runApplication` 功能来通过Kotlin运行Spring Boot应用程序。其他Spring项目的最新版本中也对Kotlin做了支持（如Spring Framework，Spring Data和Reactor）。

有关更多信息，请参阅参考文档的Kotlin支持部分。
 
# Actuator Improvements
对Spring Boot 2.0的许多actuator 端口进行了改进。所有HTTP actuator 端口现在都发布在/actuator路径下，并且改进了生成的JSON payloads。

我们现在也不会在默认情况下暴露很多端口。如果您要升级现有的Spring Boot 1.5应用程序，请务必查看迁移指南并特别注意management.endpoints.web.exposure.include属性。
 

# Actuator JSON
Spring Boot 2.0改进了从许多端点返回的JSON payloads信息。

现在许多端口都有能更精确地反映底层数据的JSON信息。例如，/actuator/conditions端口（在Spring Boot 1.5中是/autoconfig）现在将有一个顶级contexts key来将结果分组。

现在可以使用Spring REST Docs生成的REST API 文档，并随每个版本发布。
 

# Jersey and WebFlux Support
除了支持Spring MVC和JMX，您现在可以在开发Jersey或WebFlux应用程序时访问actuator端口。
Jersey通过自定义JerseyResource ，WebFlux使用自定义 HandlerMapping来支持。

# Hypermedia links
该 `/actuator` 端口现在为所有的活动端口提供了一个HAL格式的超媒体链接（即使在classpath下没有Spring HATEOAS）。

# Actuator @Endpoints
为了支持Spring MVC，JMX，WebFlux和Jersey，我们为actuato端口开发了一种新的编程模型。该@Endpoint注解可以与 `@ReadOperation`，`@WriteOperation`、`@DeleteOperation` 组合使用，来定制一个对技术无感知的开发端口。
 
您还可以使用 `@EndpointWebExtension` 或 `@EndpointJmxExtension` 为端口编写特定的技术扩展功能。

# Micrometer
Spring Boot 2.0不再提供自己的metrics API。相反，我们依靠micrometer.io来满足所有应用程序监控需求。

Metrics可以输出到各种系统，如Atlas，Datadog，Ganglia，Graphite，Influx，JMX，New Relic，Prometheus，SignalFx，StatsD和Wavefront等。另外还可以使用简单的in-memory metrics。

支持JVM指标（包括CPU，内存，线程和GC），Logback，Tomcat，Spring MVC＆RestTemplate。

# Data Support
除了上面提到的“Reactive Spring Data”支持外，在数据领域还进行了一些其他更新和改进。

# HikariCP
Spring Boot 2.0 中的默认的数据库连接池组件已从Tomcat连接池切换到HikariCP。Hakari提供了更卓越的性能，不过也有许多用户更喜欢 Tomcat Pool。

# Initialization
数据库初始化逻辑在 Spring Boot 2.0 中已经更加合理化。Spring Batch，Spring Integration，Spring Session和Quartz的初始化现在默认情况下仅在使用嵌入式数据库时才会发生。该 enabled 属性已被更具表现力的枚举所取代。例如，如果您想要始终执行Spring Batch初始化，您可以通过设置 `spring.batch.initialize-schema=always` 来实现。

如果在使用 Flyway 或 Liquibase 管理你的 DataSource，并且您正在使用嵌入式数据库，Spring Boot现在会自动关闭 Hibernate 的自动 DDL 功能。
 

# JOOQ
Spring Boot 2.0 现在基于 DataSource 自动检测 jOOQ 方言（类似于为JPA方言所做的）。`@JooqTest` 还引入了一个新的注解来简化只有 jOOQ 使用的测试。

# JdbcTemplate
Spring Boot 使用自定义的 `spring.jdbc.template` 属性自动配置 `JdbcTemplate` 。

# Spring Data Web Configuration
pring Boot公开了一个新的 `spring.data.web` 配置 namespace 来很容易的配置分页和排序。

# Influx DB
Spring Boot现在支持自动配置开源数据库InfluxDB。要启用InfluxDB支持，您需要设置一个`spring.influx.url` 属性，并将 `influxdb-java` 包含到您的类路径中。

# Flyway/Liquibase Flexible Configuration

# Hibernate
现在支持自定义Hibernate命名策略。对于高级场景，您现在可以使用常规bean在上下文中定义`ImplicitNamingStrategy` 或 `PhysicalNamingStrategy`。

现在也可以通过 `HibernatePropertiesCustomizerbean` Bean 来更加细致地定制 Hibernate 使用的一些属性。
 

# MongoDB Client Customization
现在可以通过定义一个 `MongoClientSettingsBuilderCustomizer` 类型的 bean,来定制支持Spring Boot自动配置的Mongo Client。

# Redis
现在可以使用 `spring.cache.redis.*` 属性配置Redis的缓存默认值。

# Web

# Context Path Logging 上下文路径记录
当使用嵌入式容器时，当您的应用程序启动时，上下文路径将与HTTP端口一起打印出来。例如，embedded Tomcat现在看起来像这样：
```javascript
Tomcat started on port(s): 8080 (http) with context path '/foo'
```

# Web Filter Initialization Web过滤器初始化
Web filters 现在在所有容器内都支持 eagerly 初始化。

# Thymeleaf
Thymeleaf starter 现在包含了支持javax.time 类型的thymeleaf-extras-java8time 。

# JSON Support
新的 `spring-boot-starter-json` starter  gathers必要的字节来读写 JSON。它不仅提供了 `jackson-databind`，同时也为java8环境提供了很多非常有用的模块：`jackson-datatype-jdk8`, `jackson-datatype-jsr310` 和 `jackson-module-parameter-names`。这个新的 starter 现在被用于之前定义 jackson-databind 的地方。


# Quartz
自动配置现在也支持 `Quartz Scheduler`。我们还添加了新的 `spring-boot-starter-quartz` starter POM。

您可以使用内存的 `JobStores` 或完整的基于JDBC存储的 `JobDetail`。所有 `JobDetail`，`Calendar` 和 `Trigger` beans将会通过 `Scheduler` 自动注册。
 
 如果您更喜欢除了 Jackson 以外的产品，Spring Boot 2.0 对GSON支持已经大大提高。我们还引入了对JSON-B的支持（包括JSON-B测试支持）。


# Testing

# Miscellaneous 其他
除了上面列出的变化之外，还有很多小的调整和改进，包括：

`@ConditionalOnBean` 现在在确定条件是否被满足时使用逻辑AND而不是逻辑OR。

Unconditional类现在包含在自动配置报表中。

spring CLI应用程序现在包含可用于创建Spring Security的兼容散列密码的encodepassword command。

计划任务(i.e. ``@EnableScheduling) 可以通过scheduledtasks`actuator 端口来进行review。

loggers actuator 端口现在允许重新设置一个日志级别作为它的默认值。

使用 Spring Session 的用户现在可以通过 sessions actuator 端口查看和删除 sessions。
 

# Animated ASCII Art
最后，为了好玩，Spring Boot 2.0现在支持动画GIF横幅。