---
title: docker常用命令
abbrlink: 1956304856
date: 2022-04-28 22:28:36
tags: 学习笔记
---
docker新手笔记
<!--more-->
#### 对于docker的理解

docker容器类似于虚拟机，docker镜像类似于ISO镜像，镜像可以在容器中运行

#### 常见的docker命令

拉取远程镜像
```shell bash
docker pull <imageName>:<tag>
```

将docker镜像挂在到服务器中运行容器，并定义容器和端口
```shell bash
docker run -it -v <服务器目录>:<容器目录> -p <服务器端口>:<容器端口> --name <容器命名> <imageName>:<tag> /bin/bash
```

查看正在运行中的容器和容器操作记录
```shell bash
docker ps -a
docker ps
```

查看当前的docker镜像
```shell bash
docker images
```

运行和停止docker容器
```shell bash
docker start <containerId>
docker restart <containerId>
docker stop <containerId>
```

连接正在运行中的容器
```shell bash
docker attach <containerId>
```
