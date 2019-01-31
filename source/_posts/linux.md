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
```
<!--more-->
``` JavaScript
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
### 生产环境配置
``` JavaScript
ubuntu@VM-0-7-ubuntu:/$ sudo mkdir develop
ubuntu@VM-0-7-ubuntu:/$ sudo mkdir product
ubuntu@VM-0-7-ubuntu:/$ sudo mv develop developer
ubuntu@VM-0-7-ubuntu:/$ cd developer/
ubuntu@VM-0-7-ubuntu:/developer$ sudo mkdir git-repository
ubuntu@VM-0-7-ubuntu:/developer$ ls
git-repository
ubuntu@VM-0-7-ubuntu:/developer$ cd git-repository/
ubuntu@VM-0-7-ubuntu:/developer/git-repository$ git clone git@github.com:zlyyyy/zzMusic.git
ubuntu@VM-0-7-ubuntu:/developer/git-repository$ sudo git clone git@github.com:zlyyyy/zzMusic.git
Cloning into 'zzMusic'...
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
ubuntu@VM-0-7-ubuntu:/developer/git-repository$ ssh-keygen -t rsa -C "zlyyyy"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
/home/ubuntu/.ssh/id_rsa already exists.
Overwrite (y/n)? u^H^H^H^H^[[D^[[B^H^H^[[2~^H^H^[[D^[[D	^H^H
ubuntu@VM-0-7-ubuntu:/developer/git-repository$ ssh-keygen -t rsa -C "zlyyyy"
Generating public/private rsa key pair.

//参考链接 https://blog.csdn.net/u014343528/article/details/48787221
cat: /User/username/.ssh/id_rsa.pub: No such file or directory
ubuntu@VM-0-7-ubuntu:/developer/git-repository$ cat /home/ubuntu/.ssh/id_rsa.pub
ubuntu@VM-0-7-ubuntu:/developer/git-repository$ sudo git clone git@github.com:zlyyyy/zzMusic.git
Cloning into 'zzMusic'...
Warning: Permanently added the RSA host key for IP address '52.74.223.119' to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.

```
``` JavaScript
s.test.com.conf 

server {
    listen 80;
    server_name s.test.cn;
    access_log /etc/nginx/logs/access.log combined;
    index index.html index.jsp index.php;

    location ~ /zzMusic/dist/view/* {
        deny all;
    }
    location / {
        root /product/front/;
        add_header Access-Control-Allow-Origin '*';
    }
}
```
``` JavaScript
music.test.com.conf
server {
    listen 80;
    server_name music.zhaoly.cn;
    access_log /etc/nginx/logs/access.log combined;
    index index.html index.jsp index.php;

    location = / {
        root /product/front/zzMusic/dist;
        index index.html;
    }
    location ~ .*\.html$ {
        root /product/front/zzMusic/dist;
        index index.html;
    }
    location ~ .*\.do$ {
        proxy_pass http://music.zhaoly.cn;
    }
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```
报错
nginx: [emerg] host not found in upstream "music.test.cn" in /usr/local/nginx/conf/vhost/yq.nginx.com.conf:19 这个错误。
解决
vim /etc/hosts

修改hosts文件，在hosts文件里面加上一句

127.0.0.1       localhost.localdomain   music.test.cn
