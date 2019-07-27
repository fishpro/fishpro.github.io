---
layout: post
title: Spring Boot 配置文件和命令行配置
categories: SpringBoot
description: Spring Boot 配置文件和命令行配置
keywords: FreeMarker, Spring, Date
---

Spring Boot 属于约定大于配置，就是说 Spring Boot 推荐不做配置，很多都是默认配置，但如果想要配置系统，使得软件符合业务定义，Spring Boot 可以通过多种方式进行配置。

Spring Boot 配置文件默认在 src/main/resouces/application.properties ，配置文件可以是 .properties 也可以是 .yml 后缀。两者除了展示形式，没有区别。

**配置文件的三种方式**
1. bootstrap 开头 .properties 后缀 或 .yml 后缀，通常用于 Spring Cloud 应用
2. application 开头 .properties 后缀 或 .yml 后缀，每个 Spring Boot 应用默认
3. 通过 @Configuration 注解的代码类配置
4. 通过 cmd 命令行配置

# 1 bootstrap 

## 1.1 bootstrap 应用场景

因为本系列是 Spring Boot 教程实例，没有设计到 Spring Cloud ，而 Bootstrap 实际应用场景是在 Spring Cloud，本章也不具体讨论。

## 1.2 bootstrap 与 application 

bootstrap.yml（bootstrap.properties）用来程序引导时执行，应用于更加早期配置信息读取，如可以使用来配置application.yml中使用到参数等。

application.yml（application.properties) 应用程序特有配置信息，可以用来配置后续各个模块中需使用的公共参数等。

bootstrap.yml 先于 application.yml 加载

为什么要有 bootstrap 配置

# 2 application
application.yml(application.properties) 是针对独立 Spring Boot 的应用配置文件，跟 .NET 的 web.confg(app.confg) 文件功能一样。
## 2.1 application 常用配配置
### 2.1.1 端口配置
properties 格式
```bash
#指定springboot内嵌容器启动的端口，默认使用tomcat容器时在8080端口    
server.port=8081
```
yml 格式
```bash
#指定springboot内嵌容器启动的端口，默认使用tomcat容器时在8080端口    
server
    port: 8081
```
### 2.1.2 thymeleaf组件配置
properties 格式
```bash
#是否开启thymeleaf缓存
spring.thymeleaf.cache=false
#thymeleaf路径
spring.thymeleaf.prefix=classpath:/templates/ 
#后缀
spring.thymeleaf.suffix=.html
#编码
spring.thymeleaf.encoding=UTF-8
#文本类型
spring.thymeleaf.content-type=text/html
#展示形式
spring.thymeleaf.mode=HTML5
```
yml 格式
```bash
#thymelea模板配置
spring:
  thymeleaf:
    #缓存
    cache: false
    #thymeleaf 所在路径
    prefix: classpath:/templates/
    #thymeleaf 后缀
    suffix: .html
    #thymeleaf 采用的标准
    mode: HTML5
    #thymeleaf 编码格式
    encoding: UTF-8
```
### 2.1.3 数据库连接配置

properties 格式
```bash
#描述数据源 #编码、时区等格式
spring.datasource.url=jdbc:mysql://localhost:3306/tanglong?useUnicode=true&
characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull&allowMultiQueries=true&serverTimezone=Asia
#数据源用户名
spring.datasource.username=root
#数据源密码
spring.datasource.password=0000
#数据源驱动包名
spring.datasource.driverClassName = com.mysql.cj.jdbc.Driver
#数据源类型
spring.datasource.type = com.alibaba.druid.pool.DruidDataSource
```
### 2.1.4 数据持久化
```bash
#是否打印sql语句
spring.jpa.show-sql= true
#mybatis配置文件路径
mybatis.config-location=classpath:MyBatis.xml
#指定 mybatis 本地路径，例如在 mybatis/mappings 下所有的 xml 后缀的文件
mybatis.mapper-locaitons=classpath:mybatis/mappings/*.xml
#打印myBatis的sql语句 com.demo.mapper  为包名
logging.level.com.demo.mapper=debug
#别名实体包，多个逗号隔开
mybatis.type-aliases-package=com.user.bean
```

### 2.1.5 多种开发环境

多环境配置请参见 [Spring Boot 多环境如何配置](https://www.cnblogs.com/fishpro/p/spring-boot-study-multienv.html)

```bash
#开发/测试/生产环境分别对应dev/test/prod，可以自由定义，当前配置为开发环境
spring.profiles.active=dev

#不同环境中的配置信息可以写在其他文件中
application-test.properties 或者 application-prod.properties
```

## 2.2 如何获取 application 中的值
### 2.2.1 新建一个类标注 @ConfigurationProperties
1. 新建一个 java 类
2. 给类加  @ConfigurationProperties(prefix = "corn") 其中 prefix 表前缀，例如 corn.username=123,那么corn就是前缀
假设 application.yml 代码如下
```bash
corn:
  uploadPath: /Users/jiaojunkang/Documents/project_java/corn/src/corn-master/files/
  rootPath: /Users/jiaojunkang/Documents/project_java/fwrsoft/html/
  host: corn.icontag.cn
  componentAppId:
  componentSecret:
  componentToken:
  componentAesKey:
  baiduApikey:
  baiduSecretkey:
  baiduAppId:
  smsUrl:
  smsUid:
  smsPwd:
  pageSize: 10
```
那么对应的 java 配置类
```java

@Component
@ConfigurationProperties(prefix = "corn")
public class CornConfig {
    //上传路径
    private String uploadPath;

    //静态文件跟目录
    private String rootPath;

    //HOST
    private String host;
    //微信开发平台appid
    private String componentAppId;
    private String componentSecret;
    private String componentToken;
    private String componentAesKey;

    private String baiduApikey;
    private String baiduSecretkey;

    private   String  smsUrl;

    private String pageSize;

    public String getPageSize() {
        return pageSize;
    }

    public void setPageSize(String pageSize) {
        this.pageSize = pageSize;
    }

    public String getSmsUrl() {
        return smsUrl;
    }

    public void setSmsUrl(String smsUrl) {
        this.smsUrl = smsUrl;
    }

    public String getSmsUid() {
        return smsUid;
    }

    public void setSmsUid(String smsUid) {
        this.smsUid = smsUid;
    }

    public String getSmsPwd() {
        return smsPwd;
    }

    public void setSmsPwd(String smsPwd) {
        this.smsPwd = smsPwd;
    }

    private   String  smsUid;

    private   String  smsPwd;


    public String getBaiduAppId() {
        return baiduAppId;
    }

    public void setBaiduAppId(String baiduAppId) {
        this.baiduAppId = baiduAppId;
    }

    private String baiduAppId;



    public String getUploadPath() {
        return uploadPath;
    }

    public void setUploadPath(String uploadPath) {
        this.uploadPath = uploadPath;
    }

    public String getRootPath() {
        return rootPath;
    }

    public void setRootPath(String rootPath) {
        this.rootPath = rootPath;
    }

    public String getHost() {
        return host;
    }

    public void setHost(String host) {
        this.host = host;
    }

    public String getComponentAppId() {
        return componentAppId;
    }

    public void setComponentAppId(String componentAppId) {
        this.componentAppId = componentAppId;
    }

    public String getComponentSecret() {
        return componentSecret;
    }

    public void setComponentSecret(String componentSecret) {
        this.componentSecret = componentSecret;
    }

    public String getComponentToken() {
        return componentToken;
    }

    public void setComponentToken(String componentToken) {
        this.componentToken = componentToken;
    }

    public String getComponentAesKey() {
        return componentAesKey;
    }

    public void setComponentAesKey(String componentAesKey) {
        this.componentAesKey = componentAesKey;
    }

    public String getBaiduApikey() {
        return baiduApikey;
    }

    public void setBaiduApikey(String baiduApikey) {
        this.baiduApikey = baiduApikey;
    }

    public String getBaiduSecretkey() {
        return baiduSecretkey;
    }

    public void setBaiduSecretkey(String baiduSecretkey) {
        this.baiduSecretkey = baiduSecretkey;
    }
}

```

### 2.2.2 在普通的类中使用 value 配置

给属性增加 @Value 标签，例如 使用  @Value("${spring.redis.host}") 标注 host 表示 Redis

```java
@Value("${spring.redis.host}")
    private String host = "127.0.0.1";

    @Value("${spring.redis.port}")
    private int port = 6379;

    // 0 - never expire
    private int expire = 0;

    //timeout for jedis try to connect to redis server, not expire time! In milliseconds
    @Value("${spring.redis.timeout}")
    private int timeout = 0;

    @Value("${spring.redis.password}")
    private String password = "";
```




# 3 @Configuration
使用 @Configuration 也可以替代部分 application.yml 文件配置，但 @Configuration 可以更加灵活的实现配置的自定义，他们的区别或者在于 @Configuration 可以动态配置，缺点是一旦发布无法更改。
1. @Configuration 通常用于 aplication的补充
2. @Configuration 用在类上，@Bean 用在方法上


例如配置 QuartzConfigration 通常采用的配置
```java

@Configuration
public class QuartzConfigration {
    @Bean
    public Properties quartzProperties() throws IOException {
        PropertiesFactoryBean propertiesFactoryBean = new PropertiesFactoryBean();
        propertiesFactoryBean.setLocation(new ClassPathResource("/config/quartz.properties"));
        propertiesFactoryBean.afterPropertiesSet();
        return propertiesFactoryBean.getObject();
    }

    // 创建schedule
    @Bean(name = "scheduler")
    public Scheduler scheduler() {
        return schedulerFactoryBean().getScheduler();
    }
}
```

# 4 命令行配置
Spring Boot 在服务器部署可以使用下面命令，这时使用 `--` 开头引入 spring 中的 application 的值，即可在命令行配置 Spring Boot。

```bash
java -jar xxxx-0.0.1-SNAPSHOT.jar
```

我们可以通过在命令行增加配置的方式给 Spring Boot 添加配置，命令行配置优先于 application.yml 执行。

如下配置了一个端口，使用  `--server.port=8088` 配置 来实现命令行配置
```bash
java -jar xxxx-0.0.1-SNAPSHOT.jar --server.port=8088 --spring.profiles.active=dev
```