---
layout: post
title: Spring Boot Mybatis 多数据源动态切换数据源使用教程
categories: SpringBoot
description: Spring Boot Mybatis 多数据源动态切换数据源使用教程
keywords: Mybatis,SpringBoot,多数据源
---

实际业务中，经常会出现**多个数据源的场景**，例如读写分离，我们需要对主库进行写，对从库进行读操作，这个时候可能操作多个数据源，也可能我们的库是按照月动态增加的，需要每个月都切换数据源，这个时候，就需要我们用到本章的知识。

总结一下场景
1. 一个工程访问多个不同数据库 例如同时访问 mysql、oracle 等
2. 一个工程访问同一类型数据库，但是有多个数据源
3. 一个工程中访问同一个表，但表按照一定规则放在不同服务器的数据库中
4. 主从读写分离库应用
5. 分表分库应用

![图多数据源动态切换原理]()

---
参考
- https://blog.csdn.net/ylforever/article/details/79600631
- https://www.jianshu.com/p/cac4759b2684
- 