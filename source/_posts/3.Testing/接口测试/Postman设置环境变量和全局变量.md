---
title: Postman设置环境变量和全局变量
permalink: 
date: 2019-07-30 17:42:00
categories:
  - 3.Testing
  - 接口
tags:
  - 接口
---

## 一：设置环境变量

### 1. postman通过变换环境变量来快速变换环境地址

![postman](/images/20190730-1.png)

### 2. 现可以将localhost:80信息添加至环境

![postman](/images/20190730-2.png)

### 3. 点击确定后，在首页可看到已添加的环境变量信息及设置的变量信息

![postman](/images/20190730-3.png)

## 二：设置全局变量

### 1.设置全局变量 

进入全局变量设置页面；

![postman](/images/20190730-4.png)

### 2.设置变量值

key填`token`，value填`123456`（填具体token的值），点右下角`Save`保存全局变量。如有多个可以全部填好再保存。（全局变量值可用js获取实现）

![postman](/images/20190730-5.png)

### 3.获取变量值

在`Headers`中添加一个header，key填`token`（接口自行规定），value为`{{token}}`（刚刚在global设置的key）。鼠标移动到上面，会显示具体的value值。也可以点右上角的小眼睛，看所有的全局变量。

![postman](/images/20190730-6.png)

### 4.查看日志

View--Show Postman Console