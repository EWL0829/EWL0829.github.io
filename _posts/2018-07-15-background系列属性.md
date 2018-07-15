---
layout:     post
title:      background系列属性
subtitle:   css
date:       2018-07-10
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - CSS    
---

#### background系列属性

background是一个简写属性，其属性值可以包括
backgrund-color
background-image
background-position/background-size
background-size
background-origin
background-attachment

#### background属性学习

* background-color
 
用于表示元素的背景颜色，取值可以是十六进制色号也可以是rgba，或者是hsl。

* background-image

用于表示元素的背景图片，与上述的background-color互不冲突，取值为图片所在路径。

* background-position

用于表示背景图片的位置，它的取值有两项：x与y，x表示距离左侧边界的长度，y表示距离顶部边界的长度。

*取值举例*

|值|描述|
|:--------------|:----------|
|x(px）y(px)|第一个值代表水平，第二个值代表垂直。单位可以是例子中的像素px，也可以是别的一些常见的css单位，比如em。而当只取一个值的时候，那么第一个值就是所取值，第二个值默认为50%|
|x% y%|两个值的含义同上，当图片位置在左上角的时候就是0% 0%，当为右下角时就是100% 100%，同样，如果只设置了一个值，那么就默认第二个值为50%|
|left top, left bottom, left center, right top, right bottom, right center, center top, center bottom, center bottom |仅指定一个关键字，其他值将会是"center"|

* background-size

用于表示背景图片的大小，取值可以为百分比/数字/contain/cover。

其他两项取值都很好理解，对于contain和cover做了演示和学习，如下。

**当取值为contain的时候**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-b3899600a7e92d6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

很明显，contain指的是，在适应元素大小的情况下，将整个图片全部显示出来

**当取值为cover的时候**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-80d429e644a08798.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

cover则是在保持原来图片纵横比的情况下，将整个元素覆盖，不留空隙。（所以明显图片没有被拉扯失真）

* background-origin

用于表示图片在元素中所处的位置，取值可以为padding-box/border-box/content/box

**取值为content-box的时候**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-3660942c1e6084ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**取值为padding-box的时候**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-5cd6ddc9610144d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**取值为border-box的时候**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-0ac6876b12ded70f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上是在border为透明的时候作出的尝试，如果border有自己的颜色比如下图，就会出现背景图片边缘被遮盖的情况，虽然这个时候看起来不像是图片已经覆盖到border的样子，但它实际上已经在border上了，只是因为border原本层级高于背景，所以会被遮挡。

![image.png](https://upload-images.jianshu.io/upload_images/7930564-de4a9c5c49376e81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* background-attachment

用于表示背景图片是否随着元素的上下滚动而滚动，取值有fixed/scroll/inherit。

**scroll**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-a5afa3e9dd4d20dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**fixed**
![image.png](https://upload-images.jianshu.io/upload_images/7930564-1f078a0c24fef51c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

且上述操作都是在图片可重复的情况下进行的，如果图片不可重复就会出现在取值scroll的时候图片会因为滚动而消失在视野里，而fixed则不会，尤其在有文本对照的时候最为明显。



 ```
html,body {
    width: 100%;
    height: 100%;
    margin: 0;
    background: #000;
}
.color-con {
    width: 50%;
    height: 50%;
    margin: 20px;
    padding: 20px;
    /*background: url("./imgs/img01.jpg");*/
    background-color: #666;
    background-image: url('./imgs/img01.jpg');
    background-size: 50% auto;
    background-position: 50% 50%;
    background-origin: content-box;
    background-clip: content-box;
    background-repeat: no-repeat;
}
```
**效果图**

![image.png](https://upload-images.jianshu.io/upload_images/7930564-19a3b601fd81d375.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 参考资料

[张鑫旭对于html和body的背景色研究](https://www.zhangxinxu.com/wordpress/2009/09/%E5%AF%B9html%E4%B8%8Ebody%E7%9A%84%E4%B8%80%E4%BA%9B%E7%A0%94%E7%A9%B6%E4%B8%8E%E7%90%86%E8%A7%A3/)
[菜鸟教程背景属性](http://www.runoob.com/css/css-background.html)

