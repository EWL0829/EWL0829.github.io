---
layout:     post
title:      关于冴羽的博客系列之underscore的防抖部分内容回顾学习
subtitle:   underscore的防抖
date:       2018-10-08
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - JavaScript   
---

#### 第一步：保证在n秒之后再响应事件，而非一点触发就进行响应操作

```
var count = 1;
var container = document.getElementById('container');

function getUserAction() {
    container.innerHTML = count++;
};
// container.onmousemove = getUserAction;
function debounce(func, wait) {
    var timeout;
    return function () {
        clearTimeout(timeout)
        timeout = setTimeout(func, wait);
    }
}

container.onmousemove = debounce(getUserAction, 1000);
```
此处使用到了setTimeout去延迟返回函数，但是还有一个问题就是this

#### 第二步：解决this指向被劫持的问题
在不使用debounce去包裹执行函数的时候，在执行函数内部打印this，返回的是DOM元素本身，如下：
![this的打印](https://upload-images.jianshu.io/upload_images/7930564-c363208787b44b30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是使用上面的debounce去包裹之后就会发现，this的指向被劫持了。
![this被劫持到了window全局变量](https://upload-images.jianshu.io/upload_images/7930564-5fdd6aff1beb07ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

回顾代码可以看出不使用debounce的时候，调用该函数的对象是container，this自然就在执行期间指向了container，故而打印出的是contianer这个DOM节点，而经历过debounce包裹之后，调用debounce函数包裹并返回一个新函数，此时的this就指向了全局，因为fn被放在了setTimeout里面，setTimeout是window下的函数。但是返回的函数的this仍然是container，所以可以考虑在setTimeout里面改变this的指向。
```
let debounce = function(fn, waitTime) {
    let timer;

    return function() {
        let self = this;
        let args = arguments;
        clearTimeout(timer);
        timer = setTimeout(function(){
            fn.apply(self, args)
        }, waitTime);
    };
};
```
![image.png](https://upload-images.jianshu.io/upload_images/7930564-9920befdffa69f80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
并且此时也可以解决事件对象传入的问题，event作为参数传入getUserAction里，使用arguments就可以一并传入apply中。

#### 第三步：优化需求
如果此时需要立即执行和延迟执行并存，即我不希望非要等到事件停止触发后才执行，我希望立刻执行函数，然后等到停止触发 n 秒后，才可以重新触发执行。
下面这段代码是冴羽老师在github里写的，最初看的时候不甚明白，仔细研读之后，有了一些理解
```
function debounce(func, wait, immediate) {

    var timeout;

    return function () {
        var context = this;
        var args = arguments;
        if (timeout) clearTimeout(timeout);
        if (immediate) {
            // 如果已经执行过，不再执行
            var callNow = !timeout;
            timeout = setTimeout(function(){
                timeout = null;
            }, wait);
            if (callNow) func.apply(context, args);
        }
        else {
            timeout = setTimeout(function(){
                func.apply(context, args)
            }, wait);
        }
    }
}
```
在固定的等待时间之后将timeout设置为null,如果下一次移动鼠标的时间短于等待时间，此时timeout还没有变成null，仍然是有值的，则再次循环整个debounce代码内容的时候就会出现callNow为false的情况，那么就不会触发func.apply的事件绑定，但如果下一次移动鼠标的事件长于等待时间，那么此时timeout的值就变成了null可以正确地走完一整个流程。


