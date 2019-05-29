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


[源代下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-thymeleaf)

**目录**

* TOC
{:toc}

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
在后台设置变量
```java
model.addAttribute("welcome","您好，世界！");
```
在html中引用
```html
<p th:text="${welcome}">welcome to our world!</p>
```
输出效果
```
您好，世界！
```

$符号取上下文中的变量,就是后台输出的对象

## 2.2 选择变量表达式 *{...}
选择th:object中的变量并与${...}做比较

在后台设置变量
```java
model.addAttribute("user",getUserDTOData());
在html中调用
```html
<div th:object="${user}"><p th:text="*{username}">username</p></div><p th:text="${user.username}">username</p>
```
在浏览器中显示
```
fishpro

fishpro
```

\*{...}选择表达式一般跟在th:object后，直接选择object中的属性，示例中 \*(username)等于${user.username} 注意th:obejct 必须是在div 我测试p标签是取不到值的 

## 2.3 消息表达式 #{...}
注意消息表达式在spring boot中只有在国际化设置中能够生效，在示例中。

1. 新建 resources下文件夹 i18n
2. 新建资源文件 messages.properties，同时建立zh_CN、en_US国际化文件，表示为messages_zh_CN.properties,messages_en_US.properties
```bash
home.welcome=hello my name is fishpro
```

```bash
home.welcome=我的名字叫肥西帕劳
```

3. 在他们中建立相应的变量 home.welcome=i am fishpro等内容
4. 在html中使用
```html
<p th:text="#{home.welcome}">welcome!</p>
```
在浏览器中
```
hello my name is fishpro
```

*前提是在springboot中使用本地化操作，在resources下建立i18n目录，并创建资源文件messages.properties,在其内设置变量 home.welcome=xxx，很多教程没有提及就是坑*


## 2.4 链接网址表达式 @{...}
通常在link中使用或script中使用
```html
<link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet" th:href=@{/css/index.css}>
```

## 2.5 片段表达式 ~{...}

当有大量重复代码的时候，可以使用代码片段功能，common :: main 其中 common 表示 common.html 中的 th:fragment 为 main 的代码片段


## 2.6 字面量
在模板页面输入代码

### 2.6.1 纯文本

```html
<p th:text="这是th文本">这是html文本</p>
```
在浏览器输出
```
这是th文本
```
总结说明
```
纯文本就是指th:text里面的字符替换html的文本
````

### 2.6.2 数字字面量

在模板页面输入代码
```html
<p th:text="1234+1000">5678</p>
```
在浏览器输出
```
2234
```
总结说明
```
数字字面量就是指th:text里面的字符替换html的数字
````

### 2.6.3 布尔字面量
在模板页面输入代码
```html
<p th:if="${user.username=='fishpro'}">如果username等于fishpro就显示本文本</p>
```
在浏览器输出
```
如果username等于fishpro就显示本文本
```
总结说明
```
布尔字面量如果判断为true就显示本html标签里面的内容
````

### 2.6.4 NULL字面量
在模板页面输入代码
```html
<p th:if="${user==null}">如果username等于null就显示本文本</p>	
```
在浏览器输出
```
 
```
总结说明
```
NULL字面量就是指我们常说的null
```

### 2.6.5 文本符号
在模板页面输入代码
```html
<div th:class="content">haha2</div>	
```
在浏览器输出
```
 haha2
```
总结说明
```
NULL字面量就是指我们常说的null
```

## 2.7 追加文本
在模板页面输入代码
```html
<p th:text="'i am '+${user.username}">haha2</p>
```
在浏览器输出
```
 i am fishpro
```
总结说明
```
使用加号在文本和变量之间
```

## 2.8 文本替换

在模板页面输入代码
```html
<p th:text="|'i am ' ${user.username}|">haha2</p>
```
在浏览器输出
```
'i am ' fishpro
```
总结说明
```
在|号之间，所有的符号将被打印出来，如本次的单引号,只有 ${...},#{...},*{...}才能包括在|号之间
```
## 2.9 算术运算
Thymeleaf标准表达式支持 +、-、*、/、%

## 2.10 比较和等值运算符

## 2.11 条件表达式

## 2.12 默认表达式

## 2.13 哑操作符号

## 2.14 预处理

## 2.15 数据类型转换与格式化

# 3 设置属性值
# 4 循环迭代
# 5 条件判断
# 6 模板布局
# 7 局部变量
# 8 属性优先级
# 9 注释
# 10 内联
# 11 文本模板模式
# 12 模板缓存
# 13 