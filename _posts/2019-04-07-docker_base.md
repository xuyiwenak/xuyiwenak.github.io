---
layout: post
title: docker 常用操作指令
date: 2019-05-07 15:36:06
tag: docker
---
### 1. docker 编译  
```
docker build -t <target name>
```
### 2. docker 查看当前所有镜像  
```
docker images
```
### 3. docker 查看所有容器  
```
docker ps -a
```
### 4. docker 删除镜像  
```
# 删除一个镜像
docker rmi <image name>
# 删除id为<None>的image
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
# 删除全部镜像
docker rmi $(docker images -q)
```
### 5. docker 停止容器  
```
# 停止一个容器
docker stop <container name>
# 批量停止容器
docker stop $(docker ps -q)
```
### 6. docker 移除容器  
```
# 移除一个容器
docker rm <container name>
# 删除多个容器
docker rm $(docker ps -aq)
```