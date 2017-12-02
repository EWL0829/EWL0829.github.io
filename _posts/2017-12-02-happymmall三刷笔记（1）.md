---
layout:     post
title:      happymmall三刷笔记（1）
subtitle:   happymmall
date:       2017-12-02
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - happymmall
    - jquery 
    - javascript
---

#### 开发的准备

* nvm-windows的下载和使用

* 编辑webstorm中的live template来进行sublime中ctrl+h的头部信息代替（这回使用了ws来代替sublime编程）

#### 具体实施

nvm-windows可以直接在github中下载，[放个链接](https://github.com/coreybutler/nvm-windows)

**下载**
![image.png](http://upload-images.jianshu.io/upload_images/7930564-38187596436d3ca6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我下了个最新版的，不知道会有什么问题，但现在用着还是很美滋滋的。
具体的安装以及配置，[还是放个链接](http://www.jianshu.com/p/28bca6529150)，我就是照着这篇帖子搞定的安装，还是很赞的。
不过需要注意几个点，一旦搞错环境变量，根本出不来效果，就很痛苦了。

安装结束后，就直接在控制台输入sysdm.cpl,打开高级设置，将下载好的nvm里的nodejs路径，npm路径，以及他自身的路径都写好，不然查看版本的时候就不会出效果。比如我的环境变量：
![image.png](http://upload-images.jianshu.io/upload_images/7930564-e779ed4ba5c3192e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其次是WS的liv template， 之前一直是用sublime写项目来着，但是既然把WS都下载下来了，不用的话，未免很浪费啊，那就用嘛，但是ctrl+h出不来头部的作者时间信息，我就很烦了。于是上网搜，看文档终于找到了。

* ctrl + alt + s打开设置
![image.png](http://upload-images.jianshu.io/upload_images/7930564-f6414439bc822b91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 搜索live template
![image.png](http://upload-images.jianshu.io/upload_images/7930564-54a791015a419898.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置自己的模板组
比如名字就叫myHeadInfo,我的头部信息
![image.png](http://upload-images.jianshu.io/upload_images/7930564-c97d8505f2748fc2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/7930564-4a5f3b557934564f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/7930564-87790b3565e7a47f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
再点击绿色的加号
![image.png](http://upload-images.jianshu.io/upload_images/7930564-2efb4a98b940bf1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这回点击live template
![image.png](http://upload-images.jianshu.io/upload_images/7930564-7e700e408f56ec34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开始编写模板，比如这样的
```
/*
* author: $user$
* date  : $date$ $time$
* last modified by : $user
*/
```
![image.png](http://upload-images.jianshu.io/upload_images/7930564-237ab76efda1f58d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**abbreviation**表示之后使用的时候输入的文字，尽量简短，比如我的h，后面的描述**description**随便写,然后把上面那段模板写进**Template text**中， 然后发现**$xxx$**这种样式的文字都是紫色的，这些指的是变量，可以自己定义的，只要这段模板中有这种样式的字眼，**edit variable**就会亮起来，点击edit variable可以编辑变量的含义。
**但是在edit variable之前一定要先点击define，否则根本不能成功编辑变量，这个坑我踩过**
define直接写everywhere就行，除非你是想让它单独出现在某一个地方
![image.png](http://upload-images.jianshu.io/upload_images/7930564-6c5edce4a360f46d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点击edit variable，可以在这里面选择表达式，**user()**表示用户，**date()**表示日期，**time()**表示具体的时间，这三个是我最常用的表达式，剩下的，可以自行挖掘好玩儿的。
![image.png](http://upload-images.jianshu.io/upload_images/7930564-98166fa3ce3e16e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

搞定之后，点击**apply**,然后**ok**,退出来。随便找一个文件，比如Js文件，在顶部写个**h**,然后回车，就会看见这样的内容了

![image.png](http://upload-images.jianshu.io/upload_images/7930564-8eb69d43ba602b66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/7930564-a9e2cd86b49cfde6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**但是切记要在第一张图片出来之后再回车，不然就是个换行指令，没得卵用，如果第一张图出来了，自己又慢半拍，还是个换行，这是个考验时机的操作**

第一张红框的地方就是之前写在description里面的内容，可以更加语义化一点，写代码也更舒服。

* 搞定收工

emmmmm,今天没啥东西可说，我个人觉得这些都是干货了
