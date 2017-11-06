---
layout:     post
title:      小记：jQuery里parent和parents的差别
subtitle:   jQuery
date:       2017-11-06
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - JavaScript
    - jQuery    
---


今天写代码的时候碰到了一个问题，大体问题是这样的

HTML代码：
```
<!--问题就在第一行-->
<table class="cart-table" data-product-id="{{productId}}">
<!--看我上边这行-->
        <tr>
            <td class="cart-cell cell-check">
            <label for="" class="cart-label">
                {{#productChecked}}
                <input type="checkbox" class="cart-select" checked>
                {{/productChecked}}
                {{^productChecked}}
                <input type="checkbox" class="cart-select">                      
                {{/productChecked}}
            </label>
            </td>
            <td class="cart-cell cell-img">
                <a href="./detail?productId={{productId}}" class="link">![]({{imageHost}}{{productMainImage}})</a>
            </td>
            <td class="cart-cell cell-info">
                <a href="./detail.html?productId={{productId}}" class="link">{{productName}}</a>
            </td>
            <td class="cart-cell cell-price">￥{{productPrice}}</td>
            <td class="cart-cell cell-count">
                <span class="count-btn minus">-</span>
                <input class="count-input" value="{{quantity}}" data-max="{{productStock}}">
                <span class="count-btn plus">+</span>
            </td>
            <td class="cart-cell cell-total">￥{{productTotalPrice}}</td>
            <td class="cart-cell cell-opera">
                <span class="link cart-delete">删除</span>
            </td>
        </tr>
    </table>
```
JS代码：
```
 var $this = $(this), productId = $this.parents('.cart-table').data('product-id');
```
点击一个多选框，并读取其类名为**cart-table**的祖先元素的自定义属性 data-product-id,然而最开始犯错的地方就是我用了parent 而没有用parents。代码执行之后productId的值变成了**undefined**。
___
然后我就做了几次尝试，如下

HTML:
```
<div class="div1" data-name="myName">
    <ul class="ul1">
        <li class="li1"></li>
    </ul>
</div>
```

JS:
```
$(document).ready(function () {
        var name = $('li').parent('.div1').data('name');
        document.writeln(name);
    });
```

期待的结果是myName，然而

![image.png](http://upload-images.jianshu.io/upload_images/7930564-b4b3247b71753967.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

喜闻乐见，我就去问了度娘和谷歌，然后改了一下，

JS:
```
$(document).ready(function () {
        var name = $('li').parents('.div1').data('name');
        document.writeln(name);
    });
```
then，成功了。

![image.png](http://upload-images.jianshu.io/upload_images/7930564-7863fcbbb409f63f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

___
大致的结果让我明白了parent只能网上推一级，也就是找到父元素，而不会再继续往上找祖先元素，而parents更像是将祖先元素收集起来，然后获取我们加了给定条件的那一个祖先元素。

以下是做了几次小尝试的对比图，放上来看一下
HTML:
```
<div class="div1" data-name="myName" data-height="1.83">
    <ul class="ul1" data-color="red" data-age="20">
        <li class="li1"></li>
    </ul>
</div>
```

JS:
```
$(document).ready(function () {
        var name    = $('li').parents('.div1').data('name'),
            color   = $('li').parents('.ul1').data('color'),
            age     = $('li').parent('.ul1').data('age'),
            height  = $('li').parent('.div1').data('height');
        document.writeln(name + "\n" + color + "\n" + age+ "\n" + height);
    });
```
结果：

![image.png](http://upload-images.jianshu.io/upload_images/7930564-27a51bf40bd54abd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

That's all for today...
