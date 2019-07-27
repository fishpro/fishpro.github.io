---
layout: post
title: Spring Boot 学习 IDEA 下的 github 创建提交与修改
categories: springboot
description:  Spring Boot 学习 IDEA 下的 github 创建提交与修改
keywords: springboot,idea,github
---

本章假定你已经安装了 git 客户端，本文仅仅使用与 Mac 环境下，未在 Window下实验，但 IDEA 在 Window 和 Mac 下软件的使用方法是一致的。 


**目录**

* TOC
{:toc}


# 1 配置账号
IDEA 需要配置 git 和 github 两个配置。
## 1.1 配置 git
1. 点击 IntelliJ IDEA-EAP > Preferences > Version Control > Git 注意有说菜单是 Setting > Version Control > Git 大家自行寻找对应的配置。
2. Path to Git executable: 选项中 填写 git 所在路径
在 mac 中找不到路径 在终端输入 `whereis git` 会显示 git 所在目录，我这里是 `/usr/bin/git` 点击【apply】应用此设置
![IDEA中设置git](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_github0.png)

## 1.2 配置 github
1. 点击 IntelliJ IDEA-EAP > Preferences > Version Control > github 注意有说菜单是 Setting > Version Control > Git 大家自行寻找对应的配置。
2. 在右侧输入对应参数
- host 输入 github.com
- auth type 选择 password
- login 输入 github 用户名
- password 输入 github 密码
- 设置完成后可点击 test 按钮进行测试
- 点击【apply】应用此设置
![配置github](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_github1.png)

# 2 初次提交代码到 Github
第一次提交到 Github 有个在 Github 建立仓库的过程，
1. 添加 IDEA 顶部菜单 VCS > Import into Verstion Control > Share Project on Github
2. 进入弹窗 填写
- New respository name: 在 github 上的名字，这个名字必须是 github 上还没有的
- private 如果是私有的不公开的，就打钩
- Remote name :这个默认 origin 不用动
- Description 初次的描述就是 github 上的项目描述
- 填写好后点击 share 按钮
![Share Project on Github](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_github2.png)
3. 在弹出的提交窗口提交你需要提交的文件，这里主要要去掉 .ideal .mvn .mvnw 文件 不用提交，可以在 Commit Message 里面提交本次提交的描述
4. 在您的 github 主页查看提交的仓库，可以看到新建的仓库

![Share Project on Github](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_github3.png)


# 3 修改代码并提交
1. 右键需要提交的文件夹或者包名或者文件名
2. 选择 git > add 如果您的项目文件（灰色状态）还没有加入到 git 配置库，默认都是 IDEA 自动加入的
3. 选择 git > commit File 先向本地 git 仓库提交 **注意必须先提交到本地 git 仓库，才能提交到远程 github 仓库**
4. 选择 git > respository > push 向远程 github 仓库提交本地仓库内容

**总结**
- github 依赖于 git，必须先安装 git 环境
- 提交到 github 仓库顺序是，从 项目文件 -> add 到 项目 git 仓库文件中 ->提交到本地git仓库 -> 从 git 仓库提交大 github 仓库
