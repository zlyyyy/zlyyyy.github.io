---
title: webpack4.x开发环境配置
date: 2018-09-12 10:54:37
tags:
- webpack
- 前端自动化
categories: 前端
---
webpack4.x学习，是总结也是笔记，网上的各种答案杂又乱，基于自己学习以及综合其他各种答案自己梳理总结了一篇完整的webpack4.x配置使用。
这里我安装的webpack版本为<font color=#ff1700>4.17.2</font>，webpack-cli版本为<font color=#ff1700>3.1.0</font>
#### 一、全局安装webpack
``` bash
npm install webpack webpack-cli -g
```
#### 二、创建项目
随便哪个盘先建个<font color=#ff1700>webpack-test</font>的项目文件夹，命令行进入项目文件夹，开始项目初始化
``` bash
npm init
```
<!--more-->
一路回车，最终在项目根目录下生成<font color=#ff1700>package.json</font>文件
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
创建项目架构与文件
![文件架构](http://wx1.sinaimg.cn/large/a0bac2d9gy1fvcd0gxr8mj208k04ujrb.jpg)
尝试打包
``` bash
webpack test.js test.bundle.js
```
运行报错
``` bash
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/

ERROR in multi test.js test.bundle.js
Module not found: Error: Can't resolve 'test.bundle.js' in 'F:\webpack-test'
 @ multi test.js test.bundle.js main[1]

ERROR in multi test.js test.bundle.js
Module not found: Error: Can't resolve 'test.js' in 'F:\webpack-test'
 @ multi test.js test.bundle.js main[0]
```
> 配置警告
“mode”选项尚未设置，WebPACK将回落到<font color=#ff1700>“production”</font>这个值。将<font color=#ff1700>“mode”</font>选项设置为<font color=#ff1700>“development”</font>或<font color=#ff1700>“production”</font>，以启用每个环境的默认值。
您也可以将其设置为“NONE”以禁用任何默认行为。了解更多：HTTPS://WebPACK.JS.Org/Vistus/MODE/

第一个问题没有配置webpack的mode选项，默认有<font color=#ff1700>production</font>和<font color=#ff1700>development</font>两种模式可以设置，我们设置为<font color=#ff1700>development</font>模式测试，命令行输入
``` bash
webpack --mode development
```
依旧报错
``` bash
ERROR in Entry module not found: Error: Can't resolve './src' in 'F:\webpack-test'
```
> 未找到输入模块中的错误：无法在'F:\webpack-test'解析'./src'

#### 五、创建入口文件

> 解决上述报错问题，将test.js移动到src目录下，并重命名为index.js

解析：webpack4.x是以项目根目录下的<font color=#ff1700>'./src'</font>作为入口，但我们的项目中缺乏该路径，因此我们在根目录下创建src文件夹，事实上webpack4.x以<font color=#ff1700>'./src/index.js'</font>作为入口，单单创建<font color=#ff1700>src</font>文件而没有<font color=#ff1700>index.js</font>文件仍然会报错,所以我们会有上述操作
现在我们再次打包
``` bash
webpack index.js index.bundle.js
```
依旧报错。

> 解析：webpack4.x的打包已经不能用webpack 文件a 文件b的方式，而是直接运行<font color=#ff1700>webpack --mode development</font>或者<font color=#ff1700>webpack --mode production</font>，这样便会默认进行打包，入口文件是<font color=#ff1700>'./src/index.js'</font>，输出路径是<font color=#ff1700>'./dist/main.js'</font>，其中src目录即<font color=#ff1700>index.js</font>文件需要手动创建，而dist目录及<font color=#ff1700>main.js</font>会自动生成。 

所以我们运行
``` bash
webpack --mode development
```
或者
``` bash
webpack --mode production
```
这时dist目录下已经有了main.js，表示我们已经打包成功
为了简化操作，在package.json中scripts中添加：
``` JavaScript
"dev":"webpack --mode development",
"build":"webpack --mode production"
```
之后打包只需要在命令行执行<font color=#ff1700>npm run dev(等于webpack --mode development)</font>，执行<font color=#ff1700>npm run build(等于webpack --mode production)</font>
删除之前dist文件夹下的main.js，命令行执行
``` bash
npm run dev
```
dist目录下已经生成了main.js，main.js文件已经打包了src目录下index.js文件的代码

##### dev与build区别
我们再次执行<font color=#ff1700>npm run build</font>
``` bash
npm run build
```
结果如下图所示
![zlyyyy](http://wx3.sinaimg.cn/large/a0bac2d9gy1fvci0siz5mj20ag0e8dgl.jpg)
执行 <font color=#ff1700>npm run dev</font> 打包的是未压缩的代码，而 npm run build 是压缩后的代码。
> + <font color=#ff1700>webpack --mode production</font> 生产模式下：启用了 代码压缩、作用域提升（scope hoisting）、 tree-shaking，使代码最精简。
+ <font color=#ff1700>webpack --mode development</font> 开发模式下：相较于更小体积的代码，提供的是打包速度上的优化。

#### 六、配置其他的参数
webpack官网的命令行接口，顾名思义就是webpack指令的其他参数，只需要在<font color=#ff1700>webpack –mode production/development</font>后加上其他参数即可，如：
``` bash
webpack --mode development --watch --progress --display-modules --colors --display-reasons
```
我们把这段写入package.json的scripts之中，这样就不用每次打这么长一串了。
> <font color=#ff1700>--watch</font>(观察文件系统的变化)，<font color=#ff1700>--progress</font>(显示打包过程中的进度)，<font color=#ff1700>--display-modules</font>(在输出中显示所有模块，包括被排除的模块)，<font color=#ff1700>--color, --colors</font>(开启/关闭控制台的颜色)，<font color=#ff1700>--display-reasons</font>(显示模块包含在输出中的原因)，更多配置可以参阅<font color=#ff1700>[webpack官网](https://www.webpackjs.com/)</font>
#### 七、总结
注意事项：
> 1、webpack-cli必须要全局安装，否则不能使用webpack指令。
 2、webpack也必须要全局安装，否则也不能使用webpack指令。 
 3、webpack4.x中<font color=#ff1700>webpack.config.js这样的配置文件不是必须的。 
 4、默认入口<font color=#ff1700>(entry)</font>文件是./src/index.js，默认输出<font color=#ff1700>(output)</font>文件./dist/main.js。

配置步骤：
> 1、创建工程目录； 
2、初始化工程目录：<font color=#ff1700>npm init</font>。 
3、全局安装<font color=#ff1700>webpack-cli</font>。 
4、全局安装<font color=#ff1700>webpack</font>。 
5、<font color=#ff1700>webpack –mode development/production</font>进行打包，可在<font color=#ff1700>package.json</font>中配置<font color=#ff1700>dev</font>和<font color=#ff1700>build</font>的脚本，便只需运行<font color=#ff1700>npm run dev/build</font>，作用相同。 
6、在<font color=#ff1700>webpack –mode development/production</font>可串联设置其他参数。

这只是webpack的基本环境配置，后续会基于实际项目使用深入学习。



