---
title: Jenkins触发其他项目构建
permalink: 
date: 2020-12-22 18:39:00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

**说明**

两个项目他们在 jenkins 上的任务分别是 job1 和 job2 , 在构建 job2 的时候触发 job1 的自动构建。



**job1构建代码**

```shell
pipeline {
    agent {label "XXXX1"}
    parameters {
        booleanParam(name: 'enable',defaultValue: true,description: 'Checkbox parameter')
        string(name: 'name',defaultValue: 'licong!',description: 'what is your name!')
    }
    stages {
        stage("one") {
            steps {
                echo "${params['enable']}"
                echo "${params['name']}"
            }
        }
    } 
}
```



**job2构建代码**

```shell
pipeline {
    agent {label "XXX2"}
    stages {
        stage("stage one") {
            steps {
                echo "done something"
            }
        }
        stage("stage two") {
            when {
                expression {true} 
            }
            steps {
                 build job: './job1', parameters: [string(name: 'name', value: "demo"),booleanParam(name: 'enable' , value: false)]
            }
        }
    } 
}
```

