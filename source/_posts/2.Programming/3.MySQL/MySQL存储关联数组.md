---
title: MySQL存储关联数组
permalink: 
date: 2018-06-07 15:56:00
categories:
  - 2.Programming
  - MySQL
tags:
  - MySQL
---

关联数组：

```php
$fruits= array("apple" => "苹果", "banana" => "香蕉","oriange" => "橘子");
```

对于这样的数据，MySQL数据库因存储类型是无法直接写入的，那有什么办法呢？



**解决方案：使用PHP自带的serialize()或者json_encode()函数序列化数据成字符串，之后从数据库里面读出来的数据还是字符串格式的，用unserialize()和json_decode()函数转换成数组就可以了**



**写入数据库之前**

```php
$fruits_serialize = serialize($fruits); // 序列化成字符串

$fruits_json = json_encode($fruits); // JSON编码数组成字符串
```



**读取数据库后**

```php
$fruits _restore = unserialize($fruits_serialize); // 反序列化成数组

$fruits _dejson = json_decode($fruits_json, true); // JSON解码成数组
```

