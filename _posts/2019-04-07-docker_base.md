### 1. docker 查看当前所有镜像  
```
docker images
```
### 2. docker 查看所有容器  
```
docker ps -a
```
### 3. docker 删除镜像  
```
# 删除一个镜像
docker rmi <image name>
# 删除id为<None>的image
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
# 删除全部镜像
docker rmi $(docker images -q)
```
### 4. docker 停止容器  
```
# 停止一个容器
docker stop <container name>
# 停止全部容器
docker stop $(docker ps -q)
```
### 5. docker 移除容器  
```
# 移除一个容器
docker rm <container name>
# 删除多个容器
docker rm $(docker ps -aq)
```
### 6. docker 编译  
```
docker build -t <image name> .
```
### 7. docker 启动
```
# --rm 会在容器执行结束自动推出容器，不然列表里会越来越多
docker run -it --rm --name <container name> <image name>
```  
