---
title: 两个设备同步一个git项目
date: 2023-09-03 22:12:40
description: 在两个不同的地方使用不同的设备维护同一个项目是很常见的事情，相当于两个人一起开发一个项目，有助于锻炼协同合作能力
tags: 
  - git
  - 项目同步
categories:
swiper_index: 6
swiper_desc: 两个设备同步一个git项目
swiper_cover: https://tse1-mm.cn.bing.net/th/id/OIP-C.R1Bi3fNDyYjLsXdg9OqoIAHaEo?w=315&h=196&c=7&r=0&o=5&dpr=1.3&pid=1.7
cover: https://tse1-mm.cn.bing.net/th/id/OIP-C.R1Bi3fNDyYjLsXdg9OqoIAHaEo?w=315&h=196&c=7&r=0&o=5&dpr=1.3&pid=1.7
top_img: https://tse1-mm.cn.bing.net/th/id/OIP-C.R1Bi3fNDyYjLsXdg9OqoIAHaEo?w=315&h=196&c=7&r=0&o=5&dpr=1.3&pid=1.7
---

## 同步开发

> 关于两台电脑同步同一个项目相关简介，因为我有这个需求，所以就直接干了

##### 首先就是要有两个gitee账号

这个自己创建吧

### 第一台电脑

##### 然后就是一个账号创建项目

```BASH
# git 创建仓库流程
cd ./project
# git 初始化
git init 
# 创建初始文件 readme.md (test)
touch README.md
# 添加文件到暂存区
git add README.md
# 将暂存区中的文件添加到仓库中
git commit -m "提交说明"
# 添加远程仓库(gitee)
git remote add origin 仓库地址
# 将整个项目推送到远程仓库
git push -u origin "master"
```

##### 如果有别的项目 已经开发好了的项目，直接复制到刚刚那个文件夹内重复添加推送操作

```BASH
# 添加所有项目文件到暂存区
git add .
# 将暂存区中的文件添加到仓库中
git commit -m "提交说明"
# 将整个项目推送到远程仓库
git push -u origin "master
```

##### 或者直接添加该项目到远程仓库内 (还没试过这样，不知道会不会报错)

```BASH
cd /project
# 添加远程仓库(gitee)
git remote add origin 仓库地址
# 直接将整个项目推送到远程仓库
git push -u origin "master"
```

### 第二台电脑

> 注意：执行之前确保是第二个账号并且在第一个账号的仓库成员中

```BASH
# 项目文件夹内 初始化git
git init
# 添加远程仓库(gitee)
git remote add origin 仓库地址
# 查看远程分支
git remote -v 
# 克隆仓库下来 (这是在配置好邮箱密码前提下)
git clone 仓库地址
# 完成之后项目里面就有一个文件夹 是该项目的文件夹
cd /项目文件夹
# git 初始化
git init
# 可以修改文件 添加文件操作
touch a.txt
# 添加所有项目文件到暂存区
git add .
# 将暂存区中的文件添加到仓库中
git commit -m "提交说明"
# 将整个项目推送到远程仓库
git push -u origin "master"
```

> 到此 两个电脑就全部可以修改同一个项目了，但是还没结束，因为修改之后怎么同步到本地项目呢

### 同步到本地项目

> 有两种方式

##### 方式一

```BASH
# 查看远程仓库
git remote -v
# 从远程仓库获取最新版本到本地
git fetch origin master:newVersion
# 比较本地仓库和远程仓库的不同
git diff newVersion
# 合并newVersion分支到master分支
git merge newVersion
```

如果newVersion不再使用，可以删掉

```BASH
git branch -d newVersion
```

##### 方式二

直接pull 不过有个弊端 不知道更新了什么

```BASH
git pull
```
