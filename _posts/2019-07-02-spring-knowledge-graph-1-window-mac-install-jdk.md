---
layout: post
title: Spring Boot 2.x 入门前的准备-安装 Java JDK
categories: springboot
description: Spring Boot 2.x 入门前的准备-安装 Java JDK
keywords: springboot,jdk
---


**目录**

* TOC
{:toc}

 

本章节介绍在以 `window7`、`window10` 为代表的 `window` 和 `mac book` 下安装 `Java` 编译和开发环境JDK 1.8，在 `window` 上安装 `Java JDK` 的步骤，本章中没有难点，主要在于对 `window` 环境是否熟悉，知道 `window` 环境变量是怎么回事。
1. 下载 Java JDK
2. 安装 Java JDK
3. 设置 Java 环境变量
4. 测试是否安装成功

# 1 下载Java JDK 1.8
可以从官方网站上下载 `JDK 1.8`（也就是 `Java 8`）[Java JDK下载地址](https://www.oracle.com/technetwork/java/javase/downloads/index.html) 

如果在官网下载困难，也有热心网友在 CSDN 中上传了版本，[CSDN中的 Java JDK 下载](https://download.csdn.net/download/weixin_40354683/10971208)

## 1.1 下载适合 `window` 的安装包 

本文使用的是 jdk-8u161-windows-x64。
`window 7` 和 `window 10`  的操作是一样的。
1. 找到 `Java SE 8U161` 点击下载，当然其他 `Java SE 8Uxxx` 的版本也是可以的。
2. 点击 `Accept License Agreement`
3. 选择要下载的版本，对应 `window` 64位的是 jdk-8u161-windows-x64.exe
对应window 32位的是 jdk-8u161-windows-i586.exe 
4. 注意官方是要求注册账号号才能下载，如果网页跳转到登录页面，则自己注册一个oracle账号。

```
为什么是Java JDK 1.8，因为我们后面学习的Spring Boot 2.x 最低的要求就是 Java JDK 1.8 及以后版本。Oracle 针对 Java 8（JDK 1.8）修改开源协议版本，
```

## 1.2 下载适合 `mac book` 的安装包 

1. 找到 `Java SE 8U161` 点击下载，当然其他 `Java SE 8Uxxx` 的版本也是可以的。
2. 点击 `Accept License Agreement`
3. 选择要下载的版本，对应 `Mac OS X x64` 64位的是 jdk-8u161-macosx-x64.dmg 
4. 注意官方是要求注册账号号才能下载，如果网页跳转到登录页面，则自己注册一个oracle账号。

# 2 安装 Java JDK
在 `window` 上安装比较简单，直接双击exe文件即可安装,直接点击 下一步 即可。默认JDK安装在 C 盘的 C:\Program Files\Java\jre1.8.0_161

`window 7` 和 `window 10` 的操作是一样的。

在 `mac book` 中双击或打开 jdk-8u161-macosx-x64.dmg 进行安装

# 3 设置 Java 环境变量
所谓**环境变量**，就是我们不用切换到指定的 Java JDK 目录，就能够使用 Java 等命令行命令。

当我们安装好 Java JDK，我们在 `开始` > `运行` 中输入 `cmd` 弹出 Command 命令窗口， 输入 Java 显示
``` bash
C:\User\Jiaojunkang>java
java 不是内部或外部命令，也不是可运行的程序
```
## 3.1 window 7 Java 环境变量
1. 右键 `我的电脑` 点击 `属性` ，选择 `高级系统设置` 点击 `环境变量...`
2. 在系统变量里点击新建，变量名填写 `JAVA_HOME`，变量值填写 `Java JDK` 的安装路径，例如 `C:\Program Files (x86)\Java\jre1.8.0_161`
3. 在系统变量里点击新建变量名填写 `CLASSPATH`，变量值填写“.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar”。注意不要忘记前面的点和中间的分号。
4. 加入系统 `Path` 变量（此步骤最重要），在系统变量里找到 `Path` 变量，这是系统自带的，不用新建。双击 `Path` ，由于原来的变量值已经存在，故应在已有的变量后加上“;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin”。注意前面的分号。
5. 至此，应有的环境变量已经配置完毕。验证的方法：在运行框或者按 Ctrl +R 组合键弹出运行框中输入 `cmd` 命令，回车后输入 `java -version`，按回车出现以下画面.

## 3.2 window 10 Java 环境变量
`window 10` 版本由于优化了系统变量，比 `window 7` 相对简单一点。在追加到 系统变量 `Path` 中环境是不一样的

1. 右键 `我的电脑` 点击 `属性` ，选择 `高级系统设置` 点击 `环境变量...`
2. 在系统变量里点击新建，变量名填写 `JAVA_HOME`，变量值填写 `Java JDK` 的安装路径，例如 `C:\Program Files\Java\jre1.8.0_161`
3. 在系统变量里点击新建变量名填写 `CLASSPATH`，变量值填写“.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar”。注意不要忘记前面的点和中间的分号。
4. 双击 `Path`，点击 `新建`，添加“%JAVA_HOME%\bin”；再次点击 `新建`，添加“%JAVA_HOME%\jre\bin”。
5. 至此，应有的环境变量已经配置完毕。验证的方法：在运行框或者按 Ctrl +R 组合键弹出运行框中输入 `cmd` 命令，回车后输入 `java -version`，按回车出现以下画面.
   
## 3.3 mac book Java 环境变量

1. 检测是否安装了 Java，打开终端，输入 java -version ，如果没有安装过jdk就好提速安装jdk
```bash
No Java runtime present,requesting install.
```
如果安装了 java 就会显示
```bash
java version "1.8.0_161"
```

2. 编辑环境变量，在终端输入 
```bash
sudo vim /etc/profile
```
`sudo` 为 `root` 权限，如果需要输入密码，就输入开机密码。
在vim编辑界面中按下 i
输入
```bash
JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_161.jdk/Contents/Home"
export JAVA_HOME
CLASS_PATH="$JAVA_HOME/lib"
PATH=".$PATH:$JAVA_HOME/bin"
```
按 `ESC`，进入保存
输入 `:wq!` 保存
3. 输入 `source /etc/profile` 是的设置立即生效
```bash
source /etc/profile
```
4. 检测环境变量 `JAVA_HOME`
```bash
 echo $JAVA_HOME
```
如果输出了路径字符串表示成功了。


# 4 问题
Q:如果在 一个 `window` 操作系统中设置多个 `Java JDK` 版本

A:有的时候,我们按照的基于 `Java` 的软件自带了 `Java` 版本，那么不同的 `Java` 软件可能自带的版本不一样，那么他们是怎么共存于一个 `window` 操作系统中的呢。在环境变量下有如何使用不同版本的 `Java JDK`。

安装不同的 `Java JDK` 直接点击安装文件安装即可，如果需要在cmd命令框中实现不同的 `Java JDK` 版本，只有去修改 `JAVA_HOME` 变量。


Q:如何使用指定的 `Java SDK` 执行 `jar` 程序

A:例如 `window` 系统里面已经安装了 `jdk 1.6` 那么，我们运行的 `jar` 只能运行在 `jdk 1.8` 之上，我们如何做呢?

1. 首先我们需要安装对应的 Java JDK 版本 jdk 1.8 
2. 其次我们之间在jdk 1.8的安装目录下建立 bat 文件
3. 在 bat 文件中 增加执行命令 java -jar 指定路径

