---
title: Jenkins更新主题
permalink: 
date: 2020-09-10 11:42:00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

## 一、选择主题
主题制作网站 jenkins-material-theme

**1、选择主题颜色**
可以选择自己喜欢的任何颜色,这里紫色只做演示

![color](/images/20200910-1.png)

**2、上传logo**

要求png格式图片,最小高度40px

![color](/images/20200910-2.png)

**3、下载主题**

上传好logo后就可以下载插件主题

1. 下载的主题文件名为: `jenkins-material-theme.css`

![color](/images/20200910-3.png)

**4、配置css文件**

在Jenkins安装路径的userContent目录下新建layout文件夹

1. 将`jenkins-material-theme.css`文件复制到该目录下
2. 在该目录下新建title.css文件,其中修改代码里面的content就可以改变Jenkins的Title

![color](/images/20200910-4.png)

```css
#title.css内容如下：

#jenkins-name-icon {
    display: none;
}

.logo:after {
    content: "Jenkins of Chaos-Notebook";
    text-transform:none;
    font-weight: bold;
    font-size: 30px;
    color: White;
    line-height: 40px;
    margin-left: 40px;
}
```

## 二、主题插件配置

**1、 安装插件**

> [Simple Theme](https://plugins.jenkins.io/simple-theme-plugin/)

**2、配置插件**

Configure System -> Theme, 新增两个Css Url

![color](/images/20200910-5.png)

添加`jenkins-material-theme.css`和`Title.css`的url

1. http://localhost:8080/userContent/layout/jenkins-material-theme.css
2. http://localhost:8080/userContent/layout/title.css

![color](/images/20200910-6.png)

## 三、新的主题

查看新的主题效果

**1、界面整体UI**

![color](/images/20200910-7.png)