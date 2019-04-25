---
title: easy-mock在线部署
date: 2019-04-25 09:54:30
tags:
- 模拟数据
- mock.js
- easy-mock
categories: 前端
---
### 介绍
[Easy Mock](https://github.com/easy-mock/easy-mock) 是一个可视化，并且能快速生成模拟数据的持久化服务。
官网使用人数过多，导致官网时常崩溃，所以自己在线部署使用
### 环境
``` linux
Ubuntu 16.04.1 LTS
```
### 版本
``` linux
MongoDB 2.6.10

redis 3.0.6

npm 5.6.0

node v8.11.4
```
<!--more-->
### mongoDB安装
参考[mongoDB安装](https://www.cnblogs.com/shileima/p/7823434.html)
``` linux
sudo apt-get install mongodb

# 装好之后会自动运行mongodb程序，通过"pgrep mongo -l "查看进程是否已经启动
pgrep mongo -l
27423 mongod

# 输入mongo，回车进入数据库
MongoDB shell version: 2.6.10
connecting to: test
Welcome to the MongoDB shell
```
### redis安装
参考[redis安装](https://www.cnblogs.com/zongfa/p/7808807.html)
``` linux
sudo apt-get install redis-server

# 安装完成后，Redis服务器会自动启动，我们检查Redis服务器程序
ps -aux|grep redis
redis     4162  0.1  0.0  10676  1420 ?        Ss   23:24   0:00 /usr/bin/redis-server /etc/redis/redis.conf
conan     4172  0.0  0.0  11064   924 pts/0    S+   23:26   0:00 grep --color=auto redis
```
### 部署
#### 部署目录
``` linux
# cd到部署目录
cd /developer/git-repository

git clone https://github.com/easy-mock/easy-mock.git

cd easy-mock

# 以production方式部署，需要将 default.json 拷贝一份，命名为 production.json
cp default.json production.json

# 重点
export NODE_ENV=production

npm install

# 编译
npm run build
```
### npm run build报错building for production...Killed
>按照他人的说法是，服务器内存不够用了，这样就给他配置一个单独的内存出来就解决了

``` linux
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo /sbin/swapon /var/swap.1
 ```
### 运行
> PM2 守护应用进程
``` linux
# 启动服务，在此之前，确认已经完成了 build
NODE_ENV=production pm2 start app.js
# 查看是否启动
pm2 list
```
之后进行服务器nginx配置，重启nginx服务，便完成了easy-mock在线部署
