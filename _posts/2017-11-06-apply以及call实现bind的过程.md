---
layout:     post
title:      apply以及call实现bind的过程
subtitle:   okcoin-web
date:       2017-11-06
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - JavaScript   
---

#### 理解
1. call
改变函数的this指向，且为执行时的this指向，第二个参数逐次传入即可，在绑定之后立即执行
2. apply
改变函数的this指向，且为执行时的this指向，第二个参数需要以数组的形式传入即可，在绑定之后立即执行
3. bind
改变函数的this指向，但是同时生成一个绑定函数，绑定函数作为返回值返回，绑定之后并不立即执行，可以给返回的绑定函数继续传入参数，并且bind的第二个参数传入的格式类似于call。

#### 尝试

**bind的特点**
1. 返回一个函数
2. 可以继续给返回的函数传入参数

**尝试**

*version1*
```
//考虑到返回的是一个函数

Function.prototype.bind = function (fn) {
    return function () {      
          return this.call(fn);
    };  
};

var foo = function () {
    console.log(this.color);
};

var bar = {
    color: 'red'
};

var func = foo.bind(bar);
func();

```
*执行结果*
![image.png](https://upload-images.jianshu.io/upload_images/7930564-5bea44d0b6b9c843.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*version2*
```
//上一步中犯了一个错误导致执行出错，关于this，需要在返回函数之前做this的缓存
Function.prototype.bind = function (fn) {
    var _this = this;
    return function () {
        return _this.apply(fn);
    };
};


var foo = function () {
    console.log(this.color);
};

var bar = {
    color: 'red'
};

var func = foo.bind(bar);
func();

```
*执行结果*
![image.png](https://upload-images.jianshu.io/upload_images/7930564-7f890d5d76bcb1c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*version3*
```
//版本2可以正确执行，但是不能传入参数

Function.prototype.bind = function (fn) {
    var _this = this,
        args  = Array.prototype.slice.call(arguments, 1);//截取第一个参数以后的所有参数，且将它们转为真正的数组
    return function () {
        return _this.apply(fn,args);
    };
};


var foo = function () {
    console.log(this.color);
    console.log(arguments);
};

var bar = {
    color: 'red'
};

var func = foo.bind(bar,1,2,3,4);
func();
```
*执行结果*
![image.png](https://upload-images.jianshu.io/upload_images/7930564-741a4f8fed160590.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*version4*
```
Function.prototype.bind = function (fn) {
    var _this = this,
        args  = Array.prototype.slice.call(arguments, 1);
    return function () {
        var finalArgs = args.concat(Array.prototype.slice.call(arguments));//保证在返回的参数里还能传入参数
        return _this.apply(fn,finalArgs);
    };
};


var foo = function () {
    console.log(this.color);
    console.log(arguments);
};

var bar = {
    color: 'red'
};

var func = foo.bind(bar,1,2,3,4);
func(11,12,13,14);
```
#### 延伸参考
以上是我自己写出来的版本，但是在写完之后去查阅了一些资料，进行了进一步的理解和学习

当把bind绑定之后的函数作为构造函数返回，则会产生一些和当前编写的版本4有差异的地方，如下。
**bind**
```
var value = 2;

var foo = {
    value: 1
};

function bar(name, age) {
    this.habit = 'shopping';
    console.log(this.value);
    console.log(name);
    console.log(age);
}

bar.prototype.friend = 'kevin';

var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18');
// undefined
// daisy
// 18
console.log(obj.habit);
console.log(obj.friend);
// shopping
// kevin
```
*执行结果*
![image.png](https://upload-images.jianshu.io/upload_images/7930564-a63e1d6d83bc9a42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**版本4加入后**
```

//定义bind
Function.prototype.bind = function (fn) {
    var _this = this,
        args  = Array.prototype.slice.call(arguments, 1);//截取第一个参数以后的所有参数，且将它们转为真正的数组
    return function () {
        var finalArgs = args.concat(Array.prototype.slice.call(arguments));
        return _this.apply(fn,finalArgs);
    };
};
var value = 2;

var foo = {
    value: 1
};

function bar(name, age) {
    this.habit = 'shopping';
    console.log(this.value);
    console.log(name);
    console.log(age);
}

bar.prototype.friend = 'kevin';

var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18');
// undefined
// daisy
// 18
console.log(obj.habit);
console.log(obj.friend);
// shopping
// kevin
```
*执行结果*
![image.png](https://upload-images.jianshu.io/upload_images/7930564-c762d5fdd7beb5d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分析：首先需要明确的是new一个对象的过程，先创建一个空对象，继承构造函数的原型，然后将this指向这个新对象，然后将该对象赋给当前的变量即可。那么首先就要明确两点：1.原型继承 2.this的指向

首先，修改this
```
Function.prototype.bind = function (fn) {
    var _this = this,
        args = Array.prototype.slice.call(arguments,1);
    var boundFn = function () {
        //判断this
        var finalArgs = args.concat(Array.prototype.slice.call(arguments));
        return _this.apply(this instanceof boundFn ? this : fn, finalArgs);//在判断之后替换掉原来的this
    };
    boundFn.prototype = this.prototype;//原型继承
    return boundFn;
};
```
*执行结果*
![image.png](https://upload-images.jianshu.io/upload_images/7930564-a3d456be042c5d44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**结论：**
可以通过判断最终的返回函数的this的真正指向，去判断真正的fn是哪一个函数，究竟是原来的bar还是后来通过构造函数实例化修改了this的obj。这一点对于最终的value判断很重要，因为实例化之后的obj没有value，但是obj通过原型继承可以访问到bar的原型中的friend。
**参考材料**
* [JavaScript深入之bind的模拟实现](https://github.com/mqyqingfeng/Blog/issues/12)

#### 总结

* 重新复习了this相关的知识点，以及巩固了对于apply和call的使用
* 计划下一次整理关于原型链的知识点，同时回顾原型继承








