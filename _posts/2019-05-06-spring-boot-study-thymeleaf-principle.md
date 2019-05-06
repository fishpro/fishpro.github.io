---
layout: post
title: Spring Boot 2.x教程-Thymeleaf 原理是什么
categories: SpringBoot
description: Spring Boot 2.x教程-Thymeleaf 原理是什么
keywords: SpringBoot, Spring, Thymeleaf
---

如要要理清楚 Thymeleaf 的原理，那么就要从模板引擎的原理说起。Thymeleaf只不过是众多模板中的一员，功能是一致的。

例如 JSP 也是一种模板。

# 1 Thymeleaf 模板原理
模板的诞生是为了将显示与数据分离，模板技术多种多样，但其本质是将模板文件和数据通过模板引擎生成最终的 HTML 代码。Thymeleaf 亦是如此。

1. 没有模板
   
&emsp;&emsp;没有模板前，我们可以直接用后端语言输出HTML前端，并让浏览器渲染。

2. 有了模板

&emsp;&emsp;在有了模板后，后端语言只要输出数据（例如 XML 格式、JSON 格式），把数据交给模板引擎，模板引擎根据模板文件和数据进行渲染成HTML文档。

所谓模板引擎，需要把模板文件、数据解析到前端HTML文档流展示给用户看。
 
&emsp;&emsp;目前 Thymeleaf 是面向 Web 和独立环境的现代服务器端 Java 模板引擎，能够处理 HTML，XML，JavaScript，CSS 设置纯文本。

# 2 Thymeleaf 模板引擎
Thymeleaf 可以处理 6 种类型的ember
- HTML
- XML
- TEXT
- JAVASCRIPT
- CSS
- RAW

Thymeleaf 支持HTML、JAVASCRIPT、CSS 这就意味着他完全兼容HTML代码。

目前 Thymeleaf 可对下面几种类型进行解析

1. 文本
2. 属性
3. 循环迭代
4. 条件判断
5. 模板布局
6. 局部变量
7. 属性优先级
8. 注释
9. 内联
10. 文本模板模式
11. 其他

我们以一个简单的 [Spring Boot 2.x教程-如何使用 Thymeleaf](http://www.fishpro.com.cn/2019/05/05/spring-boot-study-thymeleaf-how-to-use/) 示例来输出来看看 Thymeleaf 是怎么工作的。

## 2.1 Thymeleaf 的 简单示例 
[源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-thymeleaf)


## 2.2 Thymeleaf 工作流程
我们需要提出几个问题
1. 当在控制层输出对象，这些对象是怎么被引擎识别

&emsp;&emsp;在 Controller 层我们输出了对象 user 和 users 两个对象。这两个对象则被模板引擎托管。
1. 引擎加载模版文件的时候，如何识别需要替换的值

&emsp;&emsp;模板引擎根据 Controller 中的模板路径 /demo/simple 找到相应的模板文件，模板文件中使用正则表达式查找模板标签语言，Thymeleaf 模板引擎采用 th: 开头。例如 th:value, th:text 。

2. 最后引擎是如何吧控制层对象转化成 html 等前段语言输出的

&emsp;&emsp;模板引擎找到了模板文件中的模板表达式，模板引擎根据模板表达式中的内容来实现一种算法，算法把 Controller 输出的对象匹配到模板表达式中。



---
引用

[浅谈模板引擎](https://www.cnblogs.com/dojo-lzz/p/5518474.html)


