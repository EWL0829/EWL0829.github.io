---
layout:     post
title:      JavaScript中的类型转换
subtitle:   类型转换
date:       2017-11-06
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - JavaScript   
---

ECMAScript6 中新加入了一种类型（Symbol）,暂且搁置，之后再说，今天主要说一说ES5 中的几种数据类型
* Number
* String
* Boolean
* Object
* undefined
* Null
___
#### 既然有类型，那么首先就谈一谈如何区分类型

上代码：
```
var num = 1,
    str = 'string',
    bool = false,
    a,
    b = null,
    obj = {name:'obj'},
    foo = function () {
      alert('hello world');
    };
```

分别使用typeof来检测他们的类型，会返回什么呢？

![image.png](http://upload-images.jianshu.io/upload_images/7930564-84875f912c848c4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先有这么几点疑问，javascript中不是函数都是对象吗，也没有说函数是单独的一种类型啊，为什么会返回function呢？其次，为什么null是object呢？
在不同的浏览器中，对于function的typeof操作返回的值不尽相同，Safari5之前版本以及Chrome版本 61.0.3163.100中为function，其他浏览器均返回object。
并且null被认为是一个空的对象引用，似乎也没什么不对。
但是我要区分function和object的时候怎么办？这时候就可以使用Instanceof

![image.png](http://upload-images.jianshu.io/upload_images/7930564-25b13f9b72992f28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####类型转换
尤其是涉及到对象的类型转换，很烦这个东西
举个栗子：
```
[] == ![];//true
```
夭寿了啊，怎么这样？但仔细想一想，[]是啥，是个空数组，数组是典型的引用类型，引用类型就涉及到了传递方式的问题，在这个问题中涉及到了强制转换和自动转换的知识点。这个问题属于强制转换，而这个转换过程正是决定返回true还是false的关键。
使用==操作符时操作数都会被强制转换为合适的类型，基本的转换规则如下：
* 一个操作数为布尔值，那么两个操作数就被转为数值，false -> 0 , true -> 1,之后再比较相等性
* 一个操作数是字符串，一个是数值，先将字符串转为数值，再进行比较
* 一个操作数是对象那个，另一个不是，那么就先调用valueOf(),返回值如果是基本类型那么就继续比较，如果不是那就调用toString()（比如第二个式子，![]进行了自动转换类型,返回的当然就不再是对象，那么一个对象一个非对象的比较就回到了我们这第三种情况， 具体的看下面）
___
自动转换发生的条件：
1. 不同类型做相互运算的时候
2. 对非布尔值类型的数据求布尔值
3. 对非数值类型的数据使用一元运算符

拿上面的**[] == ![]**举例子，首先，!是逻辑非，对于一个对象而言，使用!就直接返回false(来自javascript高级程序设计44页)

[] == false;好的，回到了第一条转换规则，[]转成布尔值是啥呢？可不就是false吗？先用toString()将[]转成"",那么空字符串的布尔值就一目了然了，false== false， 结束了。


[阮大神---数据类型转换](http://javascript.ruanyifeng.com/grammar/conversion.html)
