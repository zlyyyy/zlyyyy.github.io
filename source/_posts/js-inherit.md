---
title: js实现继承的方式
date: 2018-08-21 15:14:33
tags:
- 继承方式
categories: 前端
---
### js实现继承的方式
> JS作为面向对象的弱类型语言，继承是其非常强大的特性之一
### 定义一个父类
``` JavaScript
//定义一个动物类
function Animal(name){
    //属性
    this.name = name || 'Animal';
    //实例方法
    this.sleep = function(){
        console.log(this.name + '正在睡觉');
    }
}
//原型方法
Animal.prototype.eat = function(food){
    console.log(this.name + '正在吃' + food)
}
```
<!--more-->

### 1、原型链继承
> 核心：将父类的实例作为子类的原型

``` JavaScript
function Cat(){
}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat'

//test
var cat = new Cat();
console.log(cat.name);//cat
console.log(cat.eat('fish'));//cat正在吃fish
console.log(cat.sleep());//cat正在睡觉
console.log(cat instanceof Animal);//true
console.log(cat instanceof Cat);//true
```
> 特点

1. 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
2. 父类新增原型方法/原型属性，子类都能访问到
3. 简单，易于实现

> 缺点

1. 要想为子类新增属性和方法，可以在Cat构造函数中，为Cat实例增加实例属性。如果要新增原型属性和方法，则必须放在new Animal()这样的语句之后执行
2. 无法实现多继承
3. 来自原型对象的引用属性是所有实例共享的
4. 创建子类实例时，无法向父类构造函数传参

### 2、构造继承
> 核心：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）

``` JavaScript
function Cat(name){
    Animal.call(this);
    this.name = name || 'Tom'
}

//test
var cat = new Cat();
console.log(cat.name);//Tom
console.log(cat.sleep());//Tom正在睡觉
console.log(cat instanceof Animal);//false
console.log(cat instanceof Cat);//true
```
> 特点

1. 解决了1中，子类实例共享父类引用属性的问题
2. 创建子类实例时，可以向父类传递参数
3. 可以实现多继承（call多个父类对象）

> 缺点

1. 实例并不是父类的实例，只是子类的实例
2. 只能继承父类的实例属性和方法，不能继承原型属性/方法
3. 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能

### 3、实例继承
> 核心：为父类实例添加新特性，作为子类实例返回

``` JavaScript
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}

//test
var cat = new Cat();
console.log(cat.name);//Tom
console.log(cat.sleep());//Tom正在睡觉
console.log(cat instanceof Animal);//true
console.log(cat instanceof Cat);//false
```
> 特点

1. 不限制调用方式，不管是new 子类()还是子类(),返回的对象具有相同的效果

> 缺点

1. 实例是父类的实例，不是子类的实例
2. 不支持多继承

### 4、拷贝继承

``` JavaScript
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}

//test
var cat = new Cat();
console.log(cat.name);//Tom
console.log(cat.sleep());//Tom正在睡觉
console.log(cat instanceof Animal);//false
console.log(cat instanceof Cat);//true
```
> 特点

1. 支持多继承

> 缺点

1. 效率较低，内存占用高（因为要拷贝父类的属性）
2. 无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）

### 5、组合继承
> 核心：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用

``` JavaScript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
//组合继承需要修复构造函数指向的。
Cat.prototype.constructor = Cat;

//test
var cat = new Cat();
console.log(cat.name);//Tom
console.log(cat.sleep());//Tom正在睡觉
console.log(cat instanceof Animal);//true
console.log(cat instanceof Cat);//true
```
> 特点

1. 弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法
2. 既是子类的实例，也是父类的实例
3. 不存在引用属性共享问题
4. 可传参
5. 函数可复用

> 缺点

1. 调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

### 6、寄生组合继承
> 核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点

``` JavaScript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
Cat.prototype.constructor = Cat; // 需要修复下构造函数

//test
var cat = new Cat();
console.log(cat.name);//Tom
console.log(cat.sleep());//Tom正在睡觉
console.log(cat instanceof Animal);//true
console.log(cat instanceof Cat);//true
```
> 特点

1. 堪称完美

> 缺点

1. 实现较为复杂
