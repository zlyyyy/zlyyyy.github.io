---
title: vue中sass配置
date: 2018-09-07 11:04:43
tags:
- sass
- css预编译
categories: 前端
---
# vue中sass配置
> 做了个vue项目练手，官方的脚手架中没有sass的模板的编译，想在其中使用sass怎么办？
#### 1、首先下载sass的依赖包
``` bash
npm install sass-loader --save-dev
//sass-loader依赖于node-sass
npm install node-sass --save-dev 
//安装style-loader 与vue-style-loader 是一样的
npm install style-loader --save-dev
```
快捷方式
``` bash
npm install sass-loader node-sass vue-style-loader --D
```
<!--more-->
#### 2、在build文件夹下的webpack.base.conf.js的rules里面添加配置
``` JavaScript
module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test'), resolve('node_modules/webpack-dev-server/client')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('media/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      },
      //添加下面这一段代码
      {
        test: /\.sass$/,
        loaders: ['style', 'css', 'sass']
      },
    ]
  }
```
#### 3、在需要用到sass的地方修改style标签
``` JavaScript
<style lang="scss" scoped="" type="text/css">
//你的sass语言

$font-color: #333;

body {
  color: $font-colo;  //编译后为color:#333，类似于js的变量
}

</style>
```