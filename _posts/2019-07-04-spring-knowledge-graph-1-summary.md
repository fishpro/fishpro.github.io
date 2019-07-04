
# 1 特点
来自 `Spring Boot` 官方的一段话
 
>Spring Boot可以轻松创建可以运行的独立的，生产级的基于Spring的应用程序。我们对Spring平台和第三方库采取自己的看法，以便您尽可能轻松地使用本教程。大多数Spring Boot应用程序只需要很少的Spring配置。

>您可以使用Spring Boot来创建可以使用java -jar或更传统的war部署来启动的Java应用程序 。我们还提供了一个运行“spring script”的命令行工具。

>我们的主要目标是：

>1. 为所有Spring开发提供一个更快，更广泛的入门体验。
>2. 立即开始开发。
>3. 提供大型项目（如嵌入式服务器，安全性，指标，运行状况检查和外部配置）通用的一系列非功能性功能。
>4. 绝对不会生成代码，并且不需要XML配置。
 
 以上一段话，基本概况了 `Spring Boot`的所有的有用的特定。

 可以这么理解， 无论你会不会 `Java` ，`Spring Boot` 都为你准备好了一切，你只需要抬起你的手，在键盘上敲下 `Spring Boot` 的代码即可完成 `Spring Boot`，他是一个可用于生产环境的千万应用级框架，区别于其他框架的是，他更像一种积木程式，奇怪的是任何想要接入这个积木程式的应用都是可行的。

通过网友整理
1. 独立运行
2. 内置servlet容器（tomcat等）
3. 提供项目初始化的 Maven配置（Gradle）
4. 自动配置Spring
5. 准生产环境应用监控
6. 无代码生产的xml配置
7. 与 Docker 集成方便
8. 强大的生态系统，几乎所有的功能你只要找到对应的插件即可




# 2 系统要求
Spring Boot 2.0.0.RELEASE 需要Java 8 或 9 以及 Spring Framework 5.0.4.RELEASE或更高版本。为Maven 3.2+和Gradle 4提供了明确的构建支持。

Spring Boot 支持
1. Tomcat 8.5
2. Jetty 9.4
3. Undertow 1.4

您也可以将Spring Boot应用程序部署到任何Servlet 3.0+兼容容器

# 3 Spring Boot 的安装
Spring Boot 通其他程序一样也是个 jar 包，你可以通过复制黏贴来安装到本地，更多的是我们通过 Maven 工具或者 Gradle 来自动化配置。

**新手入门，其实完全不用管 Maven怎么安装、Gradle 怎么安装，因为这会使得你陷入另一个知识点的循环，使得自己不能专心学习 Spring Boot 。**

通常我们使用开发工具 IDEA 的时候，就已经自动配置好了一切，且当约到问题的时候，我们采取研究 Maven怎么装。
 