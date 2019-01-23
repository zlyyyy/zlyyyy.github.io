---
title: linux服务器配置
date: 2019-01-23 10:53:37
tags:
- 服务器配置
categories: 前端
---
### linux服务器配置
> 新购服务器配置
``` JavaScript
//下载node
wget https://nodejs.org/download/release/v8.11.1/node-v8.11.1-linux-x64.tar.gz
//解压node
tar -xzvf node-v8.11.1-linux-x64.tar.gz
//移动
mv node-v8.11.1-linux-x64 /usr/local/node
//cd到/usr/local/node/bin 做软链-为了全局访问
//若提示Permission denied，前面加sudo
ln -s /usr/local/node/bin/node /usr/local/bin/
ln -s /usr/local/node/bin/npm /usr/local/bin/

//yarn安装
//参考官网 https://yarnpkg.com/zh-Hans/docs/install#debian-stable

//git安装
sudo apt install git
//检测是否安装成功
git --version
//git配置
git config --global user.name '名字'
git config --global user.email '邮箱'
//生成ssh秘钥
ssh-keygen -t rsa -C '邮箱'
//查看秘钥
~/.ssh/id_rsa.pub

//安装Nginx
//参考地址https://cloud.tencent.com/developer/article/1353638
sudo apt-get update
sudo apt-get install nginx
```
