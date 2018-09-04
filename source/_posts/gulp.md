---
title: gulp安装和简单使用
date: 2018-09-04 10:19:09
tags:
- gulp
- 前端自动化
categories: 前端
---

#### 一、安装nodejs

+ [nodejs官网](https://nodejs.org/en/)下载安装文件安装
+ 安装完成后，终端输入<font color=#ff1700>node-v</font>打印出nodejs的版本号，这时候nodejs已经安装成功
+ 新版本的nodejs集成了npm。终端输入<font color=#ff1700>npm-v</font>打印出npm的版本号

#### 二、安装gulp(全局)

+ 终端输入
``` JavaScript
npm install gulp -g
```
+ 测试安装是否成功，终端输入<font color=#ff1700>gulp-v</font>

#### 三、配置项目

> package.json是基于node.js项目必不可少的配置文件，用于存放项目根目录的普通json文件重点内容

+ 在终端进入文件根目录，执行<font color=#ff1700>npm init</font>，一路回车，最终在项目根目录下生成package.json文件
<!--more-->
``` JavaScript
{
    "name": "gulp_test", /*项目名,切记这里命名不要与模块一样，如命名为gulp，要地安装gulp时就会出错*/
    "version": "1.0.0", /*版本号*/
    "description": "", /*描述*/
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "", /*作者*/
    "license": "ISC" /*项目许可协议*/
}
```

#### 四、本地安装gulp及gulp插件

<font size=4>**本地安装gulp**</font>

全局安装gulp是为了执行gulp任务，本地安装gulp是为了调用gulp插件的功能进入你的项目文件路径中

``` JavaScript
npm install gulp --save-dev
```
> --svae-dev 的作用就是将安装的模块写入package.json中

安装完成后项目变化
+ node_modules文件夹>>gulp模块被下载到这个文件夹中
+ package.json中写入了<font color=#ff1700>devDependencies</font>字段，并在该字段下填充了gulp模块名

<font size=4>**本地安装gulp插件**</font>

这里我们安装使用最多的必备的几个插件
1. 删除文件--del
1. 语法检查--gulp-jshint
2. 合并文件--gulp-concat
3. 压缩代码--gulp-uglify
4. 文件重命名--gulp-rename
5. css压缩--gulp-minify-css

``` JavaScript
npm install --save-dev del jshint gulp-jshint gulp-concat gulp-uglify gulp-rename gulp-minify-css
```
> npm安装插件，可以单独安装，也可以几个一起安装空格隔开。 

单独安装
``` JavaScript
npm install --save-dev del
```

上面jshint gulp-jshint说明
gulp-jshint 2.0版本必须先下载jshint，使用npm安装：
``` JavaScript
npm install jshint gulp-jshint
```

#### 五、创建gulpfile.js文件
在项目根目录下创建gulpfile.js文件
``` JavaScript
/*引入gulp及相关插件 require('node_modules里对应模块')*/
var gulp = require('gulp');
var jshint = require('gulp-jshint');//js检查
var concat = require('gulp-concat');//js合并
var uglify = require('gulp-uglify');//js压缩
var rename = require('gulp-rename');//文件重命名
var minifycss = require('gulp-minify-css');//css压缩
var del = require('del');//文件删除

//创建任务，进行js代码检查
gulp.task('jshint',function(){
  return gulp.src('js/**/*.js') //检查文件：js目录下所有的js文件
    .pipe(jshint()) //进行js检查
    .pipe(jshint.reporter('default')); //对代码进行报错提示
});

//创建任务，进行js压缩
gulp.task('script', function () {
  return gulp.src('js/**/*.js') //检查文件：js目录下所有的js文件
    .pipe(concat('all.js')) //合并all.js文件
    .pipe(gulp.dest('dist')) //合并后的文件输出的目录
    .pipe(uglify()) //压缩all.js文件
    .pipe(rename({suffix: '.min'}))//rename压缩后的文件名
    .pipe(gulp.dest('dist/js/')); //压缩后的文件输出的目录
});

//创建任务，进行css压缩
gulp.task('minifycss', function () {
  return gulp.src('css/**/*.css') //获取文件：css目录下所有的css文件
    .pipe(minifycss()) //执行压缩
    .pipe(rename({suffix: '.min'}))//rename压缩后的文件名
    .pipe(gulp.dest('dist/css/')); //压缩后的文件输出的目录
});

//实时监听文件变化，执行各个任务
gulp.task('watch', function () {
  gulp.watch('js/**/*.js', ['jshint']);
  gulp.watch('js/**/*.js', ['script']);
  gulp.watch('css/**/*.css', ['minifycss']);
});

//在编译文件之前删除一些文件
gulp.task('clean', function () {
  return del('dist/**/*');
});

//默认任务
gulp.task('default', ['clean'], function () {
  gulp.run('jshint','script', 'minifycss', 'watch');
});

```
#### 六、运行gulp
在终端输入并回车执行
``` JavaScript
gulp
```