---
layout: post
title: Spring Boot 2.x 入门前的准备-IntelliJ IDEA 开发工具的安装与使用
categories: springboot
description: Spring Boot 2.x 入门前的准备-IntelliJ IDEA 开发工具的安装与使用
keywords: springboot,idea
---


**目录**

* TOC
{:toc}


常用的用于开发 spring boot 项目的开发工具有 eclipse 和 IntelliJ IDEA 两种，最近有声音提出 visual code 也开始流行开发 java，而且确实如此， vs code 是一个很有潜力的开发工具。

本章，主要讲述在 window 和 mac 操作系统环境下如何安装 IntelliJ IDEA 。注意 IntelliJ IDEA 是收费的，也有用于免费的版本供大家开发学习。

在安装 IntelliJ IDEA 之前，你应该知晓如何安装 Java JDK [Spring Boot 2.x 入门前的准备-安装 Java JDK](https://www.cnblogs.com/fishpro/p/spring-knowledge-graph-1-window-mac-install-jdk.html) 可以帮助你了解和安装 Java JDK。

# 1 下载 IntelliJ IDEA
可以从一些渠道下载  IntelliJ IDEA

1. 从官方网站下载 [IntelliJ IDEA ](https://www.jetbrains.com/idea/)，选择 `download` ，会进入 Download IntelliJ IDEA 界面，界面上有两个选择 `Ultimate` 和 `Community` 两个版本，不花钱可以选择 `Community`。 但你知道的 免费版本不能用于 Web 开发，就是说开发不了Spring Boot 程序。

2. 从网友提供的 IntelliJ IDEA ，这个大家可以从谷歌或者百度搜索引擎搜索。例如CSDN有网友贴出 [CSDN IntelliJ IDEA](https://blog.csdn.net/shengshengshiwo/article/details/79599761)

# 2 破解版本安装
官方下载版本直接安装就好了。大部分人用于学习目的，也可以从网上下载个破解版本。破解版本分 window 与 mac 破解方法，大部分步骤是一致的。
 ## 2.1 window 下的破解 2018.3.1最新版破解
 1. 官网下载IDEA 2018.3.1的[商业版本](https://download.jetbrains.com/idea/ideaIU-2018.3.1.exe)
 2. 破解jar下载 JetbrainsIdesCrack-3.4-release-enc.jar [点我去下载](https://pan.baidu.com/s/1mPk08hYrdLhdk4IunANX_g)
 3.把这个破解补丁 JetbrainsIdesCrack-3.4-release-enc.jar 放到安装目录（如果是zip解压版，放到解压目录）的bin目录
 4. 找到idea.exe.vmoptions和idea64.exe.vmoptions，使用记事本或 nodepad++ 之类的编辑器打开。
 5. 在最后一行增加：
 ```bash
 -javaagent:C:/JetBrains/IntelliJ IDEA 2018.3.1/bin/JetbrainsIdesCrack-3.4-release-enc.jar
```
6.启动安装的 IDEA 输入
```
ThisCrackLicenseId-{
"licenseId":"ThisCrackLicenseId",
"licenseeName":"you",
"assigneeName":"good",
"assigneeEmail":"bukengnikengshui@126.com",
"licenseRestriction":"For This Crack, Only Test! Please support genuine!!!",
"checkConcurrentUse":false,
"products":[
{"code":"II","paidUpTo":"2099-12-31"},
{"code":"DM","paidUpTo":"2099-12-31"},
{"code":"AC","paidUpTo":"2099-12-31"},
{"code":"RS0","paidUpTo":"2099-12-31"},
{"code":"WS","paidUpTo":"2099-12-31"},
{"code":"DPN","paidUpTo":"2099-12-31"},
{"code":"RC","paidUpTo":"2099-12-31"},
{"code":"PS","paidUpTo":"2099-12-31"},
{"code":"DC","paidUpTo":"2099-12-31"},
{"code":"RM","paidUpTo":"2099-12-31"},
{"code":"CL","paidUpTo":"2099-12-31"},
{"code":"PC","paidUpTo":"2099-12-31"}
],
"hash":"2911276/0",
"gracePeriodDays":7,
"autoProlongated":false}
```


## 2.2 window 下的破解 2018.2.2最新版破解
1. 官网下载IDEA 2018.2.2的商业版本
2. 破解jar下载：JetbrainsCrack-3.1-release-enc.jar [点我去下载](https://pan.baidu.com/s/1jRvUMHIY6tsqqXo7uc89KA)
3. 把这个破解补丁JetbrainsCrack-3.1-release-enc.jar放到安装目录的bin目录
4. 修改bin目录里面的两个配置文件idea64.exe.vmoptions（64位系统）、idea.exe.vmoptions（32位系统），记得添加这一行内容的时候前后都要留一行空行（注意路径中是反斜杠）
```
-javaagent:C:/JetBrains/IntelliJ IDEA 2018.2.2/bin/JetbrainsCrack-3.1-release-enc.jar
```

5. 启动idea，选择注册码激活（Activation code），输入以下注册码： 
```
K71U8DBPNE-eyJsaWNlbnNlSWQiOiJLNzFVOERCUE5FIiwibGljZW5zZWVOYW1lIjoibGFuIHl1IiwiYXNzaWduZWVOYW1lIjoiIiwiYXNzaWduZWVFbWFpbCI6IiIsImxpY2Vuc2VSZXN0cmljdGlvbiI6IkZvciBlZHVjYXRpb25hbCB1c2Ugb25seSIsImNoZWNrQ29uY3VycmVudFVzZSI6ZmFsc2UsInByb2R1Y3RzIjpbeyJjb2RlIjoiSUkiLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifSx7ImNvZGUiOiJSUzAiLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifSx7ImNvZGUiOiJXUyIsInBhaWRVcFRvIjoiMjAxOS0wNS0wNCJ9LHsiY29kZSI6IlJEIiwicGFpZFVwVG8iOiIyMDE5LTA1LTA0In0seyJjb2RlIjoiUkMiLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifSx7ImNvZGUiOiJEQyIsInBhaWRVcFRvIjoiMjAxOS0wNS0wNCJ9LHsiY29kZSI6IkRCIiwicGFpZFVwVG8iOiIyMDE5LTA1LTA0In0seyJjb2RlIjoiUk0iLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifSx7ImNvZGUiOiJETSIsInBhaWRVcFRvIjoiMjAxOS0wNS0wNCJ9LHsiY29kZSI6IkFDIiwicGFpZFVwVG8iOiIyMDE5LTA1LTA0In0seyJjb2RlIjoiRFBOIiwicGFpZFVwVG8iOiIyMDE5LTA1LTA0In0seyJjb2RlIjoiR08iLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifSx7ImNvZGUiOiJQUyIsInBhaWRVcFRvIjoiMjAxOS0wNS0wNCJ9LHsiY29kZSI6IkNMIiwicGFpZFVwVG8iOiIyMDE5LTA1LTA0In0seyJjb2RlIjoiUEMiLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifSx7ImNvZGUiOiJSU1UiLCJwYWlkVXBUbyI6IjIwMTktMDUtMDQifV0sImhhc2giOiI4OTA4Mjg5LzAiLCJncmFjZVBlcmlvZERheXMiOjAsImF1dG9Qcm9sb25nYXRlZCI6ZmFsc2UsImlzQXV0b1Byb2xvbmdhdGVkIjpmYWxzZX0=-Owt3/+LdCpedvF0eQ8635yYt0+ZLtCfIHOKzSrx5hBtbKGYRPFDrdgQAK6lJjexl2emLBcUq729K1+ukY9Js0nx1NH09l9Rw4c7k9wUksLl6RWx7Hcdcma1AHolfSp79NynSMZzQQLFohNyjD+dXfXM5GYd2OTHya0zYjTNMmAJuuRsapJMP9F1z7UTpMpLMxS/JaCWdyX6qIs+funJdPF7bjzYAQBvtbz+6SANBgN36gG1B2xHhccTn6WE8vagwwSNuM70egpahcTktoHxI7uS1JGN9gKAr6nbp+8DbFz3a2wd+XoF3nSJb/d2f/6zJR8yJF8AOyb30kwg3zf5cWw==-MIIEPjCCAiagAwIBAgIBBTANBgkqhkiG9w0BAQsFADAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBMB4XDTE1MTEwMjA4MjE0OFoXDTE4MTEwMTA4MjE0OFowETEPMA0GA1UEAwwGcHJvZDN5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxcQkq+zdxlR2mmRYBPzGbUNdMN6OaXiXzxIWtMEkrJMO/5oUfQJbLLuMSMK0QHFmaI37WShyxZcfRCidwXjot4zmNBKnlyHodDij/78TmVqFl8nOeD5+07B8VEaIu7c3E1N+e1doC6wht4I4+IEmtsPAdoaj5WCQVQbrI8KeT8M9VcBIWX7fD0fhexfg3ZRt0xqwMcXGNp3DdJHiO0rCdU+Itv7EmtnSVq9jBG1usMSFvMowR25mju2JcPFp1+I4ZI+FqgR8gyG8oiNDyNEoAbsR3lOpI7grUYSvkB/xVy/VoklPCK2h0f0GJxFjnye8NT1PAywoyl7RmiAVRE/EKwIDAQABo4GZMIGWMAkGA1UdEwQCMAAwHQYDVR0OBBYEFGEpG9oZGcfLMGNBkY7SgHiMGgTcMEgGA1UdIwRBMD+AFKOetkhnQhI2Qb1t4Lm0oFKLl/GzoRykGjAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBggkA0myxg7KDeeEwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgWgMA0GCSqGSIb3DQEBCwUAA4ICAQC9WZuYgQedSuOc5TOUSrRigMw4/+wuC5EtZBfvdl4HT/8vzMW/oUlIP4YCvA0XKyBaCJ2iX+ZCDKoPfiYXiaSiH+HxAPV6J79vvouxKrWg2XV6ShFtPLP+0gPdGq3x9R3+kJbmAm8w+FOdlWqAfJrLvpzMGNeDU14YGXiZ9bVzmIQbwrBA+c/F4tlK/DV07dsNExihqFoibnqDiVNTGombaU2dDup2gwKdL81ua8EIcGNExHe82kjF4zwfadHk3bQVvbfdAwxcDy4xBjs3L4raPLU3yenSzr/OEur1+jfOxnQSmEcMXKXgrAQ9U55gwjcOFKrgOxEdek/Sk1VfOjvS+nuM4eyEruFMfaZHzoQiuw4IqgGc45ohFH0UUyjYcuFxxDSU9lMCv8qdHKm+wnPRb0l9l5vXsCBDuhAGYD6ss+Ga+aDY6f/qXZuUCEUOH3QUNbbCUlviSz6+GiRnt1kA9N2Qachl+2yBfaqUqr8h7Z2gsx5LcIf5kYNsqJ0GavXTVyWh7PYiKX4bs354ZQLUwwa/cG++2+wNWP+HtBhVxMRNTdVhSm38AknZlD+PTAsWGu9GyLmhti2EnVwGybSD2Dxmhxk3IPCkhKAK+pl0eWYGZWG3tJ9mZ7SowcXLWDFAk0lRJnKGFMTggrWjV8GYpw5bq23VmIqqDLgkNzuoog==
```
以上来自 https://blog.csdn.net/xiaocy66/article/details/83902477


 ## 2.3 mac 下的破解 破解
 1. 下载 [IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=mac),注意下载 Ultimate 版本。如果在安装过程中出现文件已损坏可做如下操作
打开终端输入spctl --master-disable，然后打开系统偏好设置，找到安全性与隐私，允许所有来源
 2. 下载用于激活的 Jar 包，[CSDN网友提供](https://pan.baidu.com/s/1NaxYrDNi2eW66epjmk10dg)，密码:aec5
 [下载2](https://pan.baidu.com/s/1sUdjF7gX_CwYyFUR-Y835Q) 密码 iapt

 3. 下载好了的 jar包后 放到 idea 的 bin 目录下，在应用程序中找到IntelliJ IDEA，然后右键 `显示包内容`，找到 `bin` 文件。
 4. 修改 bin 目录下的 idea.vmoptions 文件，在 idea.vmoptions 文件的最后一行添加如下的配置，根据你保存的文件名自行变更。
```bash
-javaagent:../bin/jetbrains-agent.jar
```
5.为了安全起见，可以修改hosts
```bash
sudo vi /etc/hosts
#修改内容
0.0.0.0 account.jetbrains.com
```
6.打开 idea，注册选择License server方式
地址填入：http://jetbrains-license-server

最后提示：破解仅供学习使用，如果不差钱，希望支持正版

# 3 IDEA 的基础设置
`IDEA` 最基础的就是 `JDK` 的设置和 `Maven` 的设置和版本控制的设置，这三项基本都是学习 Spring Boot 的必须要设置，其他设置根据个人喜好设置，比如自动编译、字体大小、一些快捷键等。`Maven` 的设置我们单独一章讲解。这里主要讲解 `IDEA` 的基础使用。

大部分开发者开发工具的使用大同小异，主要包括的操作是。
## 3.1 配置全局 JDK 
通常 安装了 IDEA 是默认配置了 Java JDK ，当前一般是 Java JDK 1.8 版本。没有你的 IDEA 没有 Java JDK 环境，那么具体操作
1. 顶部工具栏  File ->Other Settins -> Default Project Structure -> SDKs -> JDK
2. 在弹出对话框中选择 jdk1.8，点击保存即可。

## 3.2 配置全局 Maven
默认安装了 IDEA ，也就安装了 Maven，当前版本集成了 Maven2 和 Maven3 版本。如果没有安装 Maven，那么具体操作
1. 顶部工具栏  File ->Other Settings -> Default Settings -> Build & Tools -> Maven
2. 在弹出的设置框中设置
```
Maven home directory：填写具体的maven路径，例如 /Users/jiaojunkang/Software/apache-maven-3.5.3，也可以通过下拉选择默认的 Maven 版本也可以。

```
 
## 3.3 配置版本控制 Git/Svn 
1. 顶部工具栏  File ->Other Settings -> Default Settings -> Version Control -> Git
2. IDEA默认集成了对Git/Svn的支持  直接设置执行程序，右边Test提示成功即可。