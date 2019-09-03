---
layout: post
title: 如何学习好一个新的SpringBoot知识点
categories: SpringBoot
description: 如何学习好一个新的SpringBoot知识点
keywords: SpringBoot
---

*“ 今天信息快速迭代，新技术层出不穷，作为码农大军一员，保持时刻学习的态势是一个热衷于编程的人的常态，那么如何学习好一个技术知识点呢。本章以SpringBoot点总结”*

**目录**

* TOC
{:toc}


# 1 如何学习使用新技术

使用新的技术点，通常我们只要解决问题也就够了，不能因为项目用到这个技术，就对这个技术刨根问底，感觉有点本末倒置。

如何快速使用一个知识点，通常来说看官方文档，奈何官方文档大多是英文，而且外国人的习惯一般不一样，有的不好理解。

比如spring boot，本身它是有规范的，插件怎么用，如何集成，所有插件都一样。

那么我们就有可能总结出一套插件学习的方法论。

1. *如何集成到springboot*
2. *插件本身需要什么配置*
3. *用代码展示实例*

也就以上三点就能完成一个插件从配置到使用的全部过程。


# 2 如何巩固

使用一个插件到深入理解一个插件是两回事，按照上面流程，所有的插件用起来都很简单，那么为什么当我们深入需求的时候，反而有不少大坑等着我们。

我觉得有两点
1. *插件不够成熟*
2. *我们不够熟悉，没有了解到插件的所有重要特性，在配置管理缺少东西*

这就引申出如何巩固的问题，我觉得无非涉及几个问题

1. 插件实现的原理
比如我们学习shiro如果不了解原理，就很难照着代码依葫芦画瓢。写的时候处处有疑问。

2. 插件的api有哪些
比如我学习使用 thymeleaf 的时候，使用很简单，但是碰到复杂场景，需要用到表达式，由于不了解他的其他表达式，难免很生疏。

3. 性能问题
对于请求、并发量大的场景，对使用的第三方插件需要真正得深入了解。就需要了解他的性能问题。

4. 不断的练习
  熟读唐诗三百首，不会做诗也会吟。熟能生巧嘛，就是这个道理，没事写写博客，写写教程。

# 3 如何深入学习

大部分技术点是不需要我们去深入了解，因为应用层面的技术，能好熟练应用就可以。

深入了解应该是那种对原理，源代码有完美情节的人来说。

还有一种确实是开源软件没有能好解决的问题，只好研究源代码来解决，这个时候则必须深入研究，并完全掌握。

例如，阿里就有不少项目属于二次开发。

那么如何深入学习，通常

1. *熟读英文文档*
2. *阅读源码*
3. *向别人传授知识*


所以回到文章标题，我们对一个技术知识点大部分分三个阶段就能完全掌握
1. *如何使用*
2. *如何巩固*
3. *如何深入*

实际上我很少到第三个阶段，因为我大部分是应用技术，快速实现业务需求。