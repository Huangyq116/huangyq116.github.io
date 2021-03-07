---
title: Jenkins自动杀掉衍生程序
permalink: 
date: 2019-12-26 11:30:00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

在执行 shell输入框中加入`BUILD_ID=dontKillMe` ，即可防止jenkins杀死启动的进程

```shell
export BUILD_ID=dontKillMe
PROJECT_LOCATION="/usr/local/project/"
HOST=$HOST

rsync -avz --delete --progress --exclude "config*" --exclude "db" ${WORKSPACE}/  root@${HOST}:$PROJECT_LOCATION
 
ssh -tt root@${HOST}  "    
     cd $PROJECT_LOCATION
     nohup ./server >server.log 2>&1 &
     sleep 10
     exit     
"
```

