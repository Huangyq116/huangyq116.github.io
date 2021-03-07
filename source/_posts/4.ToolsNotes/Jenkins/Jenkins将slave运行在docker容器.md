---
title: Jenkins将slave运行在docker容器
permalink: 
date: 2020-10-14 15:18 :00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

## 1、说明

Jenkins的Master-Slave架构特点可解决多并发任务的负载问题；Master节点提供WebGUI和API功能来管理运行任务，Slave节点运行Master分配的任务；这也意味着Slave节点可以分布在不同平台并且无需安装Jenkins的完整包。

## 2、配置

jenkins版本：V2.249.1

### 2.1、添加node节点配置

**1、首页-ManageJenkins-ManageNodesAndClouds页面，新建节点操作**
![addnode](/images/20201214-3.png)

**2、首页-新建自由风格任务选择该Slave节点**

![addnode](/images/20201214-4.png)

**3、运行**

![run](/images/20201214-5.png)

### 2.2、添加Docker节点信息

**1、Jenkins首页-ManageJenkins-ManagePlugins页面，下载「Docker plugin」和「Docker Slaves Plugin」两个插件**

**2、Slave机器，下载docker**

![docker](/images/20201214-6.png)

```shell
1 成功下载
2 
3 docker pull jenkins/ssh-slave
```

因为docker默认不允许外面链接的，所以要修改配置放开：

```shell
修改这个文件  /usr/lib/systemd/system/docker.service中的

ExecStart=/usr/bin/dockerd  -H fd:// --containerd=/run/containerd/containerd.sock

改成下面这个
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H fd:// --containerd=/run/containerd/containerd.sock

然后 systemctl restart docker
```

设置docker的可执行权限：

```shell
chmod 666 /var/run/docker.sock
```

**3、Jenkins首页-ManageJenkins-ManageNodesAndClouds页面，ConfigureClouds菜单下-AddANewCloud**

![docker](/images/20201214-7.png)

**4、配置DOCKER CLOUD DETAILS信息；测试Slave机器docker可访问**

![docker](/images/20201214-8.png)

 **5、配置DOCKER AGENT TEMPLATES信息**

基本信息：

![docker](/images/20201214-9.png)

容器信息：

![docker](/images/20201214-10.png)

![docker](/images/20201214-11.png)

**6、首页-新建自由风格任务选择该Slave节点：**

![docker](/images/20201214-12.png)

**7、运行：**

生成镜像过程：

![docker](/images/20201214-13.png)

执行结果：

![docker](/images/20201214-14.png)

## 4、Docker in Docker 

![docker](/images/20201214-15.png)

参考 [Jenkins Slave in Docker](https://blog.csdn.net/qq_31977125/article/details/104000507)

## 5、参考

参考 [Jenkins通过Docker-plugin部署Slave](https://blog.csdn.net/qq_31977125/article/details/82999872)

参考 [从socket权限重新认识docker架构](https://blog.csdn.net/yanggd1987/article/details/105112939)