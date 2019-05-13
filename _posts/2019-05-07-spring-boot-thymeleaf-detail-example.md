---
layout: post
title: Spring Boot 2.x教程-Thymeleaf 全攻略示例讲解
categories: SpringBoot
description: Spring Boot 2.x教程-Thymeleaf 全攻略示例讲解
keywords: SpringBoot, Spring, Thymeleaf
---

**Thymeleaf知识分三步讲解：**
1. 如何使用 Thymeleaf
2. Thymeleaf 原理是什么
3. Thymeleaf 全攻略示例讲解

前面两章节讲解了 如何使用 和 Thymeleaf 的原理是什么。那么这章节主要讲解 Thymeleaf 的官方  API。对官方 API 进行逐个示例展示。

# 1 使用文本
1. 在浏览器中会默认忽略所有非 HTML 属性标签，Thymeleaf 使用 th:* 这样的写法来实现 Thymeleaf 的自有标签。
2. 可以使用 HTML 标签 data- 开头来兼容未知属性，可以在 HTML 中使用 data-th-* 写法来实现。
3. 我们通常使用 th:* 来实现。这样做更为通用、简洁，因为 data- 开头的属性值只能适用于 HTML 标签。

# 2.标准表达式语法

|表达式分类|表达式| 说明
|:-: | :-: | :-: | :-: | :-: 
|变量表达式|${...}| 去上下文中的对象，使用最为广泛
选择变量表达式|\*{...}|*{...}选择表达式一般跟在th:object后，直接选择object中的属性，示例中 *(username)等于${user.username} 注意th:obejct 必须是在div 我测试p标签是取不到值的
消息表达式|#{...}|#前提是在springboot中使用本地化操作，在resources下建立i18n目录，并创建资源文件messages.properties,在其内设置变量 home.welcome=xxx，很多教程没有提及就是坑
链接网址表达式|@{...}|@特指url对象,常用语script、link标签的src地址引入
片段表达式|~{...}|以代码片段的形式展示，方便移动复制

具体见 [源代码spring-boot-thymeleaf](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-thymeleaf)



在Controller层建立SampleController类，构件一个公共使用的控制层方法和路由

```java
     /**
     * 构造一个user对象
     * */
    private UserDTO getUserDTOData(){
        UserDTO userDTO=new UserDTO();
        userDTO.setUsername("fishpro");
        userDTO.setSex("男");
        userDTO.setBirthday(new Date());
        return  userDTO;
    }
    /**
     * 注意上下文输出了welcome、user、users列表对象
     * */
    @RequestMapping("/index")
    public String index(Model model){
        model.addAttribute("welcome","您好，世界！");
        model.addAttribute("user",getUserDTOData());
        List<UserDTO> users=new ArrayList<>();
        users.add(new UserDTO("zhangsan","男",new Date()));
        users.add(new UserDTO("wangjingjing","女",new Date()));
        users.add(new UserDTO("limeimei","女",new Date()));
        users.add(new UserDTO("lisi","男",new Date()));
        model.addAttribute("users",users);

        return "/sample/index";
    }
```

在templates下建立sample/index.html文件


## 2.1 变量表达式 ${...}
选择上下文中的welcome变量
```html
<p th:text="${welcome}">welcome to our world!</p>
```


## 2.2 选择变量表达式 *{...}
选择th:object中的变量并与${...}做比较
```html
 <div th:object="${user}">
    <p th:text="*{username}">username</p>
</div>
<p th:text="${user.username}">username</p>
```
## 2.3 消息表达式 #{...}

## 2.4 链接网址表达式 @{...}
通常在link中使用或script中使用
```html
<link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet" th:href=@{/css/index.css}>
```

## 2.5 片段表达式 ~{...}

