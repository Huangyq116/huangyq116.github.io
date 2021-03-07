---
title: ThinkPHP修改Redis操作类，支持选择数据库功能及添加其他方法
permalink: 
date: 2018-09-10 12:01:00
categories:
  - 2.Programming
  - PHP
tags:
  - PHP
---

## 版本
ThinkPHP 3.2.2

## 1、官方默认不支持选择数据库功能及，现就可选择数据库功能进行说明

1. 1 config.php 配置文件中选择数据库 

　　　　 'REDIS_DBINDEX' =>1, // 选择库信息（0~16）

1. 2 Redis.class.php中修改__construct()方法

　　　　'dbindex'  => C('REDIS_DBINDEX') ? C('REDIS_DBINDEX') : 0;  //选择存库

　　　　$this->options['dbindex'] = isset($options['dbindex'])? $options['dbindex'] : 0;  //选择存库

　　　　$this->handler->select($this->options['dbindex']);  //选择存库

## 2、官方默认未实现鉴权功能，现就实现鉴权进行说明

1. 1 config.php 配置文件中增加鉴权密码

　　　　'REDIS_AUTH'=>'123456', //AUTH认证密码

1. 2 Redis.class.php中修改__construct()方法

　　　　if($this->options['auth']!=null){

　　　　　　$this->handler->auth($this->options['auth']); //说明有配置redis的认证配置密码 需要认证

　　　　}

## 3、具体代码如下

![code](/images/20180910-1.png)