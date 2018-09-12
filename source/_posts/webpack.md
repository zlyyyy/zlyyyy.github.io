---
title: webpack4.0开发环境配置
date: 2018-09-12 10:54:37
tags:
- webpack
- 前端自动化
categories: 前端
---
webpack4.0学习，是总结也是笔记。
这里我安装的webpack版本为4.17.2，webpack-cli版本为3.1.0
#### 一、全局安装webpack
``` bash
npm install webpack webpack-cli -g
```
#### 二、创建项目
随便哪个盘先建个webpack-test的项目文件夹，命令行进入项目文件夹，开始项目初始化
``` bash
npm init
```
一路回车，最终在项目根目录下生成package.json文件
#### 三、项目本地安装webpack
> 将所安装的包分类到开发模式下，并写入到 package.json 配置文件
``` bash
npm install webpack webpack-cli --save-dev
```
打开package.json，看到以下三行，代表安装成功
``` JavaScript
"devDependencies": {
    "webpack": "^4.17.2",
    "webpack-cli": "^3.1.0"
  }
```
#### 四、创建项目文件
1. 创建项目架构与文件

    ├── dist                                       // webpack配置文件
    ├── node_modules                                      // 项目打包路径
    ├── src                                         // 源码目录
    │   ├── css                                  
    │   │   └──  index.js                           // 接口配置
    │   ├── js                                  // 图片文件
    │   │   └──  index.js                           // 接口配置


