---
layout: post
title: Spring核心控制反转与依赖注入
categories: spring
description: Spring核心控制反转与依赖注入
keywords: Spring Ioc
---


**目录**

* TOC
{:toc}

# 1 如何理解控制反转（Ioc）/依赖注入（DI）

Spring 构件是在 Ioc（控制反转）和 Aop 的基础上，他是控制反转中所说的容器方，所以我们又说 Spring 容器。

Ioc（控制反转）这个词定义的很抽象，像 ”只能意会不能言传“ 那种，搞得很多人不求甚解，就这么稀里糊涂的跟踪念叨念叨，也不知道什么意思。

本来我以为是国人在翻译的时候，没找到准确的词语，后来外国人也受不了啦，Ioc （Inversion of Control）就是个鸡肋，于是大神 Martin Fowler 在家苦思冥想，”究竟哪些方便被控制反转了？哦是依赖的对象获得权被被人控制了“，

于是他又给他起了个名字叫DI （Dependency Injection），翻译过来叫做依赖注入。这下比之前的要好理解了，当然也是要琢磨一番的。

Ioc本质上是什么呢？
```
> 是对对象的管理权的转移
```
我们常见的操作是什么，增删改查，那么我们正常是吧正删改查放到Service结尾的类中，代码如下，
```java
public class UserService{
    int add();
    int upadte();
    int delete();
    List<UserDO> list(Map<String,Object> map);
}
```
**没有Ioc的对象管理**

没有使用Ioc之前，我们是这样的调用的,我们在构造函数中进行对象的创建。
```java
public class UserController{
    private UserService userService;
    public UserController(){
        userService=new UserService();
    }
    String index(){
        
    }
}
```

**使用Ioc后的对象管理**

使用Ioc后，则不需要再自己去创建对象（new 一个对象），我们根本看不到创建对象的过程。
```java
public class UserController{
    @Autowired
    private UserService userService; 
    String index(){
        
    }
}
```

*通过以上示例，我们注意到，Ioc实际是对象管理权的转移，很多人以为Ioc是一种设计模式，实际上并不是那么回事，他只是一种设计思想*

***Ioc能够实现解耦吗？***

实际上通过接口来设计，就是一种解耦，我们也可以通过工厂模式来实现解耦，这些方法并不是通过Ioc来实现，**Ioc只是管理权的转移**。

***Ioc对象管理权转移到哪里去？***

在Spring 或者 Spring Boot 中，对象的管理权转移到了 Spring 容器中，实际上对象的使用者并不需要关系，谁来管理对象。


# 2 依赖注入的三种方式
1. 构造函数注入
2. 属性注入
3. 接口注入

## 2.1 构造函数注入
构造函数注入，就是通过构造函数，给对象使用的类注入（传递）对象（参数）。如下图代码，通过构造函数 UserController中的参数 UserService _userService 来实现对象 UserService 的注入。
```java
public class UserController{
    private UserService userService;
    public UserController(UserService _userService){
        userService=_userService;
    }
    String index(){
        
    }
}
```

## 2.2 属性注入
通过属性的 set 方法注入到属性
```java
public class UserController{
    private UserService userService;
    private UserService setUserService(UserService _userService){
            return _userService;
    }
    public UserController(){
        userService=setUserService();
    }
    String index(){
        
    }
}
```

## 2.3 接口注入




# 3 Spring 中的依赖注入 
