---
layout:     post
title:      happymmall二次开发过程小记
subtitle:   happymmall
date:       2017-11-06
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - 随手记
---
* 记一个点： 页面自身的css样式级别高于common部分的样式，如果同时为body设置样式，那么页面肯定会显示页面自身设置的css样式，而非common部分的样式
* 只有position不为static的元素才可以设置z-index
* 在开发list页面的时候，要先搜一个东西，无论是keyword 还是categoryId，不然根本没有东西来填充p-list-con部分，调试的时候也会成大问题， 比如这样
![image.png](http://upload-images.jianshu.io/upload_images/7930564-3ad6011832951208.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

要注意!的问题，
```
!(option.container) instanceof jQuery

```
如果不加option.container外面那一层括号的话，很容易出现优先级的问题
