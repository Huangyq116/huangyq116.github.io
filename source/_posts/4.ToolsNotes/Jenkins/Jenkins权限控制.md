---
title: Jenkins的权限控制
permalink: 
date: 2020-12-14 11:30:00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

## 介绍

多人使用同一套Jenkins进行操作时，会存在任务多部署慢和误操作等不方便之处，二次开发的Jenkins可以通过业务逻辑层进行权限划分，原生的Jenkins则提供了Role-based Authorization Strategy权限插件，分为管理员和普通用户，根据目录来控制用户对job的操作。

**1.创建目录**

在Jenkins首页点击新建任务--》输入名称test--选择文件夹--点击确定---》点击保存，目录就创建好了。

![新建任务](/images/20201214-1.png)

**2.添加角色**

在Jenkins首页，点击系统管理--》选择Manage and Assign Roles--》点击Manage Roles—》在Project roles下，创建角色test，对test目录下的job有权限---》设置角色test的权限---》点击save，保存。

![添加角色](/images/20201214-2.png)

**3.分配角色**

在Jenkins首页点击系统管理--》选择Manage and Assign Roles–-》选择Assign Roles—》在Item roles下，输入用户名wangqian02，点击add，选择角色---》点击save保存

![分配角色](/images/20201214-3.png)

至此用户wangqian02仅可以查看操作test文件里的任务了。