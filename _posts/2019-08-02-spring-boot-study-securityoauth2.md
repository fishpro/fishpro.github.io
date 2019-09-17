# 1 OAuth2 概述
# 1.1 什么是 OAuth2

1. 假如 你访问一个 www.a.com 的网站，但是这个网站根本就没有注册，却要求你登录（授权）
2. 网站提供了QQ登录，微信登录、新浪登录、豆瓣登录、知乎登录
3. 你可以选择任何一个进行登录，你选择了QQ登录
4. 浏览器跳转到了QQ授权登录页面，这个页面有你的QQ头像或扫描二维码或输入QQ的登录名和密码框，你授权登录，
5. 浏览器返回到 www.a.com ，此时 www.a.com 获取了你的授权，就去 QQ 服务器获取你的个人信息作为 wwww.a.com的 用户信息。
   
   **这个过程就是一个 OAuth2 授权的过程。**


官方解释如下：


>OAuth（开放授权）是一个开放标准，允许用户授权第三方移动应用访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方移动应用或分享他们数据的所有内容，OAuth2.0是OAuth协议的延续版本，但不向后兼容OAuth 1.0即完全废止了OAuth1.0。
- 是一个标准
- 不需要提供用户名密码
- OAuth协议的延续版本
- 不兼容OAuth1.0

# 1.2 OAuth2 应用场景

1. **第三方应用授权**

    直观的看，我们访问一些网站经常可以使用微信登录、支付宝扥估、微博登录、豆瓣登录，使用这些登录后，可以正常访问该网站的功能，那么这就是一个常用的场景。这个场景的特点是：**认证服务器由第三方网站认证例如由微信、支付宝、新浪**

2. **原生App**

    请求无状态，每次都是使用 token


3. **Spa 的Html5 应用**
   
    前后端分离框架，前端请求后台数据，需要进行oauth2安全认证，比如使用vue、react后者h5开发的app。


# 1.3 名词
OAuth2 有一些特定的名称需要了解

（1） Third-party application：第三方应用程序，本文中又称"客户端"（client），比如打开知乎，使用第三方登录，选择qq登录，这时候知乎就是客户端。

（2）HTTP service：HTTP服务提供商，本文中简称"服务提供商"，即上例的qq。

（3）Resource Owner：资源所有者，本文中又称"用户"（user）,即登录用户。

（4）User Agent：用户代理，本文中就是指浏览器。

（5）Authorization server：认证服务器，即服务提供商专门用来处理认证的服务器。

（6）Resource server：资源服务器，即服务提供商存放用户生成的资源的服务器。它与认证服务器，可以是同一台服务器，也可以是不同的服务器。

# 1.4 授权模式

- 授权码模式（authorization code）
- 简化模式/隐藏模式（implicit）
- 密码模式（resource owner password credentials）
- 客户端模式（client credentials）

# 2 Spring Boot OAuth2 示例

本示例代码参考自 [How to Implement Spring Security With OAuth2](https://dzone.com/articles/spring-security-with-oauth2) 和 [基于Springboot技术开发OAuth2认证授权与资源服务器](https://blog.csdn.net/banat020/article/details/85083869)

## 2.1 新建 Spring Boot OAuth2 的 Maven 工程示例项目
Spring Boot 中，Spring Boot Security 集成了 OAuth2
## 2.2 导入依赖 Pom.xml
需要引入的插件如下：
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
    <artifactId>oauthdemo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>oauthdemo</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
        </dependency>
        <!-- 当前版本是在本机编译 -->
        <dependency>
            <groupId>org.springframework.security.oauth</groupId>
            <artifactId>spring-security-oauth2</artifactId>
            <version>2.3.4.RELEASE</version>
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
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
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
## 2.3 配置认证服务 AuthServerConfig
AuthServerConfig 文件
-配置
```java
package com.fishpro.oauthdemo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.oauth2.config.annotation.configurers.ClientDetailsServiceConfigurer;
import org.springframework.security.oauth2.config.annotation.web.configuration.AuthorizationServerConfigurerAdapter;
import org.springframework.security.oauth2.config.annotation.web.configuration.EnableAuthorizationServer;
import org.springframework.security.oauth2.config.annotation.web.configurers.AuthorizationServerEndpointsConfigurer;
import org.springframework.security.oauth2.config.annotation.web.configurers.AuthorizationServerSecurityConfigurer;
import org.springframework.security.oauth2.provider.approval.ApprovalStore;
import org.springframework.security.oauth2.provider.approval.TokenApprovalStore;
import org.springframework.security.oauth2.provider.token.TokenStore;
import org.springframework.security.oauth2.provider.token.store.InMemoryTokenStore;


@Configuration
@EnableAuthorizationServer
public class AuthServerConfig extends AuthorizationServerConfigurerAdapter{

    @Autowired
    private TokenStore tokenStore;

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    private ApprovalStore approvalStore;

    /**
     * 配置客户端信息
     * */
    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        //添加客户端信息 使用内存存储OAuth客户端信息
        clients.inMemory()
                // client_id
                .withClient("client")
                // client_secret
                .secret("secret")
                // 该client允许的授权类型，不同的类型，则获得token的方式不一样。
                .authorizedGrantTypes("authorization_code","implicit","refresh_token")
                .resourceIds("resourceId")
                //回调uri，在authorization_code与implicit授权方式时，用以接收服务器的返回信息
                .redirectUris("http://localhost:8090/")
                // 允许的授权范围
                .scopes("ADMIN","TEST");
    }

    /**
     * 配置 endpoints 信息
     * */
    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        endpoints.tokenStore(tokenStore).approvalStore(approvalStore)
                .authenticationManager(authenticationManager);
    }

    /**
     * 配置认证服务端信息
     * */
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {
        security.realm("OAuth2-Sample")
                .allowFormAuthenticationForClients()
                .tokenKeyAccess("permitAll()")
                .checkTokenAccess("isAuthenticated()");
    }

    /**
     * 获取一个token token保存在内存中（也可以保存在数据库、Redis中）。
     * 如果保存在中间件（数据库、Redis），那么资源服务器与认证服务器可以不在同一个工程中。
     * 注意：如果不保存access_token，则没法通过access_token取得用户信息
     * */
    @Bean
    public TokenStore tokenStore() {
        return new InMemoryTokenStore();
    }

    /**
     * 获取一个 ApprovalStore (ApprovalStore 具有三种策略 这里使用 TokenApprovalStore 存储在 oauth_access_token 表中)
     * */
    @Bean
    public ApprovalStore approvalStore() throws Exception {
        TokenApprovalStore store = new TokenApprovalStore();
        store.setTokenStore(tokenStore);
        return store;
    }
}


```
## 2.4 配置 资源服务

## 2.5 配置 Spring Boot Security 
- 注意 Spring Boot 2.x 中的密码模式需要 PasswordEncoder
- 模拟两个用户名密码并赋予相关的角色
- 配置目录/默认可以访问，其他都需要授权访问
```java
public class MyPasswordEncoder implements PasswordEncoder{

    @Override
    public String encode(CharSequence charSequence) {
        return charSequence.toString();
    }

    @Override
    public boolean matches(CharSequence charSequence, String s) {
        return s.equals(charSequence.toString());
    }

}
```
Spring Boot Security 配置文件 WebSecurityConfig
```java
package com.fishpro.oauthdemo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    /**
     * 注意：Spring Security 报There is no PasswordEncoder mapped for the id "null"
     * spring boot 2.0之后会报这个错误 新建一个MyPasswordEncoder
     * */
    @Autowired
    public void globalUserDetails(AuthenticationManagerBuilder auth) throws Exception {
        // 这里认证两个用户 fishpro 密码 123456 角色 ADMIN
        // 用户 test 密码 123456 角色 TEST 注意使用了 内存保存数据，避免一个示例涉及过多的知识点
        auth.inMemoryAuthentication().passwordEncoder(new MyPasswordEncoder()).withUser("fishpro").password("123456").roles("ADMIN").and()
                .withUser("test").password("`123456").roles("TEST");
    }

    /**
     * Spring Security 核心配置 设置
     * */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.formLogin() //登记界面，默认是permit All
                .and()
                .authorizeRequests().antMatchers("/","/home").permitAll() //不用身份认证可以访问
                .and()
                .authorizeRequests().anyRequest().authenticated() //其它的请求要求必须有身份认证
                .and()
                .csrf() //防止CSRF（跨站请求伪造）配置
                .requireCsrfProtectionMatcher(new AntPathRequestMatcher("/oauth/authorize")).disable();
    }

    //向 Spring 中注入 AuthenticationManager
    @Override
    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```

## 2.6 模拟OAuth2 访问