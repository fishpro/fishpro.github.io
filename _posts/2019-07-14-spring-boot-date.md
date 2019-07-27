---
layout: post
title: Spring Boot 中的日期处理
categories: SpringBoot
description: Spring Boot 中的日期处理
keywords: SpringBoot, Spring, Date
---

从 C# 转过来，像 `Date` 日期类型就最让人看不懂，明明一个简单的类型，搞得非常的复杂，而且容易出错，以至于网上各路人才都建立了自己的日期库。

标题虽然是 **Spring Boot 中的日期处理**，实际上也是 Java 的日期类处理。`Jave8` 中提供了全新的日期类，如果你要使用基于 `Java8` 的日期类，最好在你的类中说明他只能适用于 `Jave8` 。本章的目的之一就是提供可用的基于 Java8 的日期类库。

**本章以 Jave8 为基础节主要讨论日期的两个方面**
1. 日期的各种操作包括相加、相减、每月第一天、每月最后一天、周的操作等
2. 在 Spring Boot 中同一处理日期

# 1 新建 Spring Boot 项目 

1）File > New > Project（多模块工程，则从右键 工程名称 > New > Module），选择 `Spring Initializr` 然后点击 【Next】下一步

2）填写 `GroupId`（包名）、`Artifact`（项目名） 即可。点击 下一步
groupId=com.fishpro   
artifactId=java8date
  
3)选择依赖 `Spring Web Starter` 前面打钩。

4)项目名设置为 `spring-boot-study-java8date`

# 2 Java8 中日期的基础操作
为了方便测试，我们所有的日期方法，都写在 utils/DateUtils.java 中，本示例中我们新增了2个文件
1. utils/DateUtils.java 用于保存各种日期的方法，是一个静态类
2. controller/IndexController.java 用于展示各种日期方法的效果

## 2.1 LocalDate 与 Date 类型互转
## 2.1 Uninx Date 与 Date 类型互转
## 2.1 LocalDate 的格式化
## 2.1 Date 的格式化
## 

## 2.1 获取当天的日期 getToday()

## 2.2 获取当前的年月日
## 2.3 获取某个特定的日期
## 2.4 检查两个日期是否相等
## 2.5 检查重复事件，比如说生日
## 2.6 获取当前时间
## 2.7 增加时间里面的小时数
## 2.8 获取1周后的日期
## 2.9 一年前后的日期
## 2.10 使用时钟
## 2.11 判断某个日期是在另一个日期的前面还是后面
## 2.12 处理不同的时区