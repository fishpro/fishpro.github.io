---
layout: post
title: Spring Boot 使用 Mybatis整合 Quartz 与 Mysql 数据库使用教程
categories: SpringBoot
description: Spring Boot 使用 Mybatis整合 Quartz 与 Mysql 数据库使用教程
keywords: Quartz,Mybatis,SpringBoot
---

上一张主要介绍了 [Spring Boot如何使用Quartz](https://www.cnblogs.com/fishpro/p/spring-boot-study-quartz.html)，非常简单使用，本章也是个简单的示例，演示如何把多个定时任务保存到数据库中，根据需要来实现定时任务管理。本章使用到
1. mysql 5.6+ 
2. spring boot 2.x
3. java jdk 1.8+
4. mybatis 数据库访问
5. quartz
6. thymeleaf 作为任务管理


[本项目源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-quartzdb)

# 1 Quartz 主要 Api 介绍

|名称|说明|
|---|---|
|Scheduler| - 与调度程序交互的主要API。|
|Job |- 由希望由调度程序执行的组件实现的接口。|
|JobDetail |- 用于定义作业的实例。|
|Trigger| - 定义执行给定作业的计划的组件。|
|JobBuilder| - 用于定义/构建JobDetail实例，用于定义作业的实例。|
|TriggerBuilder| - 用于定义/构建触发器实例。|

![quartz关系](https://images.cnblogs.com/cnblogs_com/fishpro/1453719/o_quartz2.jpg)

如上图

- 调度器容器是所有布局的基础
- Job 实例化后必须装配到 Scheduler 中
- Trigger 实例化后必须装配到 Scheduler 中
- 一个Job 可以由多个 Trigger 

Quartz 执行顺序 

![quartz关系](https://images.cnblogs.com/cnblogs_com/fishpro/1453719/o_quartz1.jpg)

## 1.1 调度器 Scheduler

## 1.2 任务 Job
一个 job 就是一个实现了 Job 接口的类，该接口只有一个方法 execute：
```java
package org.quartz;

  public interface Job {
    public void execute(JobExecutionContext context)
      throws JobExecutionException;
  }
```
## 1.3 JobDetail
JobDetail 是用于装载 Job 类的实例对象，用于传递给 Scheduler（调度器）。

## 1.4 调度器 Trigger
Trigger用于触发Job的执行。当你创建一个Job的时候，就需要为你的Job配置一个或多个Trigger（触发器）。Quartz自带了各种不同类型的Trigger，最常用的主要是SimpleTrigger和CronTrigger

SimpleTrigger 主要用于一次性执行的 Job（只在某个特定的时间点执行一次）

CronTrigger 用于指定重复执行的任务，可按照 年、月、日、时、分、秒 来指定。CronTrigger 使用 Cron Expressions 来配置任务。

```java
trigger = newTrigger()
    .withIdentity("trigger3", "group1")
    .withSchedule(cronSchedule("0 0/2 8-17 * * ?"))
    .forJob("myJob", "group1")
    .build();
```



## 1.5 事件监听 TriggerListeners JobListeners

通过向 Scheduler 中注册 TriggerListeners 或 JobListeners 实现事件的监听。

```java
scheduler.getListenerManager().addJobListener(myJobListener, jobKeyEquals(jobKey("myJobName", "myJobGroup")));
```

```java
scheduler.getListenerManager().addJobListener(myJobListener, jobGroupEquals("myJobGroup"));
```
## 1.6 调度器监听 SchedulerListeners
仅接收来自Scheduler自身的消息，job/trigger的增加、job/trigger的删除、scheduler内部发生的严重错误以及scheduler关闭的消息等，chedulerListener也是注册到scheduler的ListenerManager上的。

添加一个 SchedulerListeners

```java
scheduler.getListenerManager().addSchedulerListener(mySchedListener);
```

删除一个 SchedulerListeners

```java
scheduler.getListenerManager().removeSchedulerListener(mySchedListener);
```
## 1.7 任务存储 Job Stores
JobStore 负责调度生命周期内所有的数据跟踪。（**可以这么理解，他的数据存储在哪里**）
### 1.7.1 内存存储 RAMJobStore
RAMJobStore 是使用最简单的 JobStore，它也是性能最高的。使用配置
```
org.quartz.jobStore.class = org.quartz.simpl.RAMJobStore
```
|名称|说明|
|---|---|
|优点|性能最高，配置简单，上手快|
|缺点|应用程序结束（或崩溃）时，所有调度信息都将丢失 |


### 1.7.2 数据库存储 JDBC JobStore
JDBC JobStore 它通过JDBC将其所有数据保存在数据库中，支持 Oracle，PostgreSQL，MySQL，MS SQLServer，HSQLDB和DB2 。您可以在Quartz发行版的“docs / dbTables”目录中找到表创建SQL脚本。
 
配置Quartz以使用JobStoreTx
```
org.quartz.jobStore.class = org.quartz.impl.jdbcjobstore.JobStoreTX
```

配置JDBCJobStore以使用DriverDelegate
```
org.quartz.jobStore.driverDelegateClass = org.quartz.impl.jdbcjobstore.StdJDBCDelegate
```

使用表前缀配置JDBCJobStore
```
org.quartz.jobStore.tablePrefix = QRTZ_
```

使用要使用的DataSource的名称配置JDBCJobStore
```
org.quartz.jobStore.dataSource = myDS
```

### 1.7.3 TerracottaJobStore
TerracottaJobStore提供了一种缩放和鲁棒性的手段，而不使用数据库。这意味着您的数据库可以免受Quartz的负载，可以将其所有资源保存为应用程序的其余部分。

配置Quartz以使用TerracottaJobStore
```
org.quartz.jobStore.class = org.terracotta.quartz.TerracottaJobStore
org.quartz.jobStore.tcConfigUrl = localhost:9510
```

# 2 Spring Boot 示例
本示例演示 Quartz 与 Mysql 集合使用详细示例。
## 2.1 数据库

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for sys_task
-- ----------------------------
DROP TABLE IF EXISTS `sys_task`;
CREATE TABLE `sys_task` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `cron_expression` varchar(255) DEFAULT NULL COMMENT 'cron表达式',
  `method_name` varchar(255) DEFAULT NULL COMMENT '任务调用的方法名',
  `is_concurrent` varchar(255) DEFAULT NULL COMMENT '任务是否有状态',
  `description` varchar(255) DEFAULT NULL COMMENT '任务描述',
  `update_by` varchar(64) DEFAULT NULL COMMENT '更新者',
  `bean_class` varchar(255) DEFAULT NULL COMMENT '任务执行时调用哪个类的方法 包名+类名',
  `create_date` datetime DEFAULT NULL COMMENT '创建时间',
  `job_status` varchar(255) DEFAULT NULL COMMENT '任务状态',
  `job_group` varchar(255) DEFAULT NULL COMMENT '任务分组',
  `update_date` datetime DEFAULT NULL COMMENT '更新时间',
  `create_by` varchar(64) DEFAULT NULL COMMENT '创建者',
  `spring_bean` varchar(255) DEFAULT NULL COMMENT 'Spring bean',
  `job_name` varchar(255) DEFAULT NULL COMMENT '任务名',
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=8 DEFAULT CHARSET=utf8;

SET FOREIGN_KEY_CHECKS = 1;
```

## 2.2 新建 Spring Boot Maven 示例工程项目 
注意：是用来 IDEA 开发工具
1. File > New > Project，如下图选择 `Spring Initializr` 然后点击 【Next】下一步
2. 填写 `GroupId`（包名）、`Artifact`（项目名） 即可。点击 下一步
    groupId=com.fishpro   
    artifactId=quartzdb
3. 选择依赖 `Spring Web Starter` 前面打钩。
4. 项目名设置为 `spring-boot-study-quartzdb`.

## 2.3 引入依赖 Pom.xml

```xml

```

## 2.4 配置 Quratz

## 2.5 

## 2.6 

-----
参考：
[http://www.quartz-scheduler.org/](http://www.quartz-scheduler.org/documentation/2.4.0-SNAPSHOT/quick-start-guide.html)
