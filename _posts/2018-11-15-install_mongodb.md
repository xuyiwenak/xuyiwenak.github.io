---
layout: post
title: 2018-11-15-install_mongodb.md
date: 2018-11-15 15:48:06
tag: mgo
---
## 1. 下载压缩包
```
wget https://fastdl.mongodb.org/osx/mongodb-osx-ssl-x86_64-4.0.0.tgz
```
## 2. 解压安装包
```
tar -zxvf mongodb-osx-ssl-x86_64-4.0.0.tgz 
```
## 3. 修改名字为mongo
```
mv mongodb-osx-x86_64-4.0.0 mongodb
```
## 4. 修改环境变量,重启生效
```
vim ~/.bash_profile
```
## 5. 增加mongodb的bin路径
```
export PATH="$PATH:/usr/local/mongodb/bin

```
## 6. 重启生效环境变量
```
source ~/.bash_profile
```
## 7. 增加db存储路径,如果你的数据库目录不是/data/db，可以通过 --dbpath 来指定。
```
sudo mkdir -p /data/db
```
## 8. 如果没有创建全局路径 PATH，需要进入以下目录
```
cd /usr/local/mongodb/bin
sudo ./mongod
```
## 9. 启动mongo服务后，另外启动一个终端执行
```
./mongo
```