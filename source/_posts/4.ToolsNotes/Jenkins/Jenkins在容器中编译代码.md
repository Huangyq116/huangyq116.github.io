---
title: Jenkins在容器中编译代码
permalink: 
date: 2020-12-22 18:26:00
categories:
  - 4. ToolsNotes
  - Jenkins
tags:
  - Jenkins
---

**1、声明式**

```shell
stages{
    stage("go build") {
        agent {
            image "golang:1.12.1"
        }
        steps {
            sh """
                go build
            """
        }
    }
}
```

**2、脚本式**

```shell
stages {
    stage("go build") {
        steps {
            script {
                docker.image("golang:1.12.1").inside() {
                    sh """
                        go build
                    """
                }
            }
        }
    }
}
```

**3、前端**

```shell
stage("npm build") {
    agent {
        docker {
            image "XXX/node:1.1.3"
        }
    }
    steps {
        sh """
            npm run build
        """
    }
}
```

