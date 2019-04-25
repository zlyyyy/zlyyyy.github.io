---
title: react基础回顾
date: 2019-01-26 17:06:23
tags:
- react
categories: 前端
---
### 开发环境搭建-这里使用yarn
#### 安装yarn
> 特点
1. 速度超快。 

    Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载。 同时利用并行下载以最大化资源利用率，因此安装速度更快。
2. 超级安全。 

    在执行代码之前，Yarn 会通过算法校验每个安装包的完整性。

<!--more-->
3. 超级可靠。 

    使用详细、简洁的锁文件格式和明确的安装算法，Yarn 能够保证在不同系统上无差异的工作。
> 安装
``` JavaScript
// 1、下载node.js，使用npm安装
npm install -g yarn 
查看版本：yarn --version
// 2、安装node.js,下载yarn的安装程序
// 下载地址 https://yarn.bootcss.com/docs/install/#windows-stable
```
#### 创建项目
安装Node> = 6和npm> = 5.2
``` JavaScript
// 已安装Node> = 6和npm> = 5.2
// 老方法
npm install -g create-react-app
create-react-app my-app
// npx方式
npx create-react-app my-app
```
``` JavaScript
// npx介绍-可忽略
// npm v5.2.0引入的一条命令（npx），引入这个命令的目的是为了提升开发者使用包内提供的命令行工具的体验。
// 这条命令会临时安装 create-react-app 包，命令完成后create-react-app 会删掉，不会出现在 global 中。下次再执行，还是会重新临时安装。
// npx 会帮你执行依赖包里的二进制文件。
// 举例来说，之前我们可能会写这样的命令：
npm i -D webpack
./node_modules/.bin/webpack -v

// 如果你对 bash 比较熟，可能会写成这样：
npm i -D webpack
`npm bin`/webpack -v

// 有了 npx，你只需要这样：
npm i -D webpack
npx webpack -v

// 也就是说 npx 会自动查找当前依赖包中的可执行文件，如果找不到，就会去 PATH 里找。如果依然找不到，就会帮你安装！
// npx 甚至支持运行远程仓库的可执行文件：
npx github:piuccio/cowsay hello

// 再比如 npx http-server 可以一句话帮你开启一个静态服务器！（第一次运行会稍微慢一些）
npx http-server

// 指定node版本来运行npm scripts：
npx -p node@8 npm run build

// 主要特点：
// 1、临时安装可执行依赖包，不用全局安装，不用担心长期的污染。
// 2、可以执行依赖包中的命令，安装完成自动运行。
// 3、自动加载node_modules中依赖包，不用指定$PATH。
// 4、可以指定node版本、命令的版本，解决了不同项目使用不同版本的命令的问题。
```
### 启动项目
``` JavaScript
// cd到项目目录下
cd my-app
// 启动项目
npm start 
//  或者 yarn start
// 浏览器访问http://localhost:3000/，即可看到项目运行效果
```
### 文件结构解析-自动创建的初始结构
```
.
├── node_modules                            // 依赖包文件
├── public
│   ├──  favicon.ico                        // 浏览器tab上图标
│   ├──  index.html                         // 项目入口文件，首页html模板
│   └──  manifest.json
├── src                                     // 源码目录
│   ├──  App.css                            // 样式
│   ├──  App.js                             // 组件
│   ├──  App.test.js                        // 自动化测试文件
│   ├──  index.css                          // 样式
│   ├──  index.js                           // 项目核心内容
│   ├──  logo.svg                           // 引入的图片
│   └──  serviceWorker.js
├── .gitignore                              // 管理当前文件夹下的文件的Git提交行为 
├── package.json                            // node包文件
├── README.md                               // 项目说明
├── yarn.lock                               // 项目依赖包版本号
.

```
### 基础知识
#### ES6 中的 bind(this)
> 在使用 React 中的 Class extends写法，如果 onClick 绑定一个方法就需要 bind(this)，如果使用React.createClass 方法就不需要

解析：

    React.createClass 是ES5 的写法默认绑定了 bind 写法
    在 ES6 中新增了class，绑定的方法需要绑定 this，如果是箭头函数就不需要绑定 this

示例：
1. 第一种写法：
``` JavaScript
handleClick(e){
    console.log(this)
}
render(){
    return (
        <div>
            <h1 onClick={this._handleClick.bind(this)}>点击<h1>
        </div>
    )
}
```
2. 第二种写法：
``` JavaScript
constructor(props){
    super(props)
    this.handleClick=this.handleClick.bind(this)
}
handleClick(e){
    console.log(this)
}
render(){
    return(
        <div>
            <h1 onClick={this.handleClick}>点击</h1>
        </div>
    )
}
```
3. 第三种写法：
``` JavaScript
handleClick=(e)=>{
    //使用箭头函数
    console.log(this)
}
render(){
    return (
        <div>
            <h1 onClick={this.handleClick}>点击</h1>
        </div>
    )
}
```
#### JSX语法细节
1. 注释写法
``` JavaScript
render(){
    return (
        {/*注释内容*/}
        或者
        {
            // 注释内容
        }
        <div>
            <h1 onClick={this.handleClick}>点击</h1>
        </div>
    )
}
```
2. 元素添加class
> class会被认为react的类，座椅这里使用className而不是class
``` JavaScript
render(){
    return (
        <div className='test'>
            <h1 onClick={this.handleClick}>点击</h1>
        </div>
    )
}
```
3. dangerouslySetInnerHTML

在react中，通过富文本编辑器进行操作后的内容，会保留原有的标签样式，并不能正确展示。
>在显示时，将内容写入__html对象中即可。具体如下：
``` JavaScript
<div dangerouslySetInnerHTML = {{ __html: checkMessages.details }} />
```
>如果是直接调用接口中的值，则是以上的写法，如果是单纯的显示固定的内容，用如下的写法：
``` JavaScript
<div dangerouslySetInnerHTML={{ __html: '<div>123</div>' }} />
```
原理:
1. dangerouslySetInnerHTMl 是React标签的一个属性，类似于angular的ng-bind；
2. 有2个{ { } }，第一{ }代表jsx语法开始，第二个是代表dangerouslySetInnerHTML接收的是一个对象键值对;
3. 既可以插入DOM，又可以插入字符串；
4. 不合时宜的使用 innerHTML 可能会导致 cross-site scripting (XSS) 攻击。 净化用户的输入来显示的时候，经常会出现错误，不合适的净化也是导致网页攻击的原因之一。dangerouslySetInnerHTML 这个 prop 的命名是故意这么设计的，以此来警告，它的 prop 值（ 一个对象而不是字符串 ）应该被用来表明净化后的数据。

4. htmlFor
>由于 JSX 就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。
``` JavaScript
<label htmlFor="test"></label>
<input id="test" />
```