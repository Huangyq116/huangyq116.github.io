---
title: Jenkins-pipeline参数化显示及插件
permalink: 
date: 2019-10-12 11:30:00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

## 1、Git Parameter

```shell
gitParameter branch: '',

branchFilter: '.*',  #分支过滤

defaultValue: 'master',

description: '选择代码分支',

name: 'branch',

quickFilterEnabled: false,   #快速搜索

selectedValue: 'NONE',

sortMode: 'NONE',  #排序

tagFilter: '*',   #tag过滤

type: 'PT_BRANCH' 
```

## 2、when判断

[参考](https://www.cnblogs.com/zoujiaojiao/p/13219057.html)