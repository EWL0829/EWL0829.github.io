---
layout:     post
title:      事件绑定回顾以及sertTimtout相关
subtitle:   事件
date:       2018-08-05
author:     EWL
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - 前端    
    - 事件绑定
    - setTimeout
    - setInterval
---

#### 不提倡的HTML事件绑定

```
<div class="event" onclick="alert('I was clicked!!!');">
        this is a event wrapper and its HTML event
    </div>
```

#### DOM0-2级

* DOM 0 级

```
<div class="event" id="dom2">
    this is a DOM0 event
</div>

document.getElementById('dom2').onclick = function () {
  alert('dom 0');
};

```

* DOM 2级

```
document.getElementById('dom2').addEventListener('click', function () {
    alert('dom2');
}, false);//false表示冒泡阶段触发，true表示捕获阶段触发
```

具体来说，事件的冒泡阶段或者是捕获阶段触发，可以体现在事件触发的顺序上，如果 是父子元素嵌套，给父子元素绑定上不同的事件，然后改变DOM2级第三个参数，就可以看出触发的顺序发生了变化。

```
<div id="father">
    this is father
    <div id="son">this is son</div>
</div>

document.getElementById('father').addEventListener('click', function () {
    alert('father');
}, true);//  捕获

document.getElementById('son').addEventListener('click', function () {
    alert('son');
}, true);// 捕获
```
上述例子中，当第三个参数为true的时候，点击子元素，先触发的是父元素的事件，然后才会触发子元素的事件。

#### setTimeout与setInterval

* setTimeout模拟setInterval
在使用setInterval的时候，如果函数的执行时间超出了间隔时间，那么久无法确切的保证每两次调用之间的事件间隔为设置的时长，所以大部分时候会使用setTimeout来模拟setInterval的使用。代码如下

```
let i = 1;
let timer = setTimeout(function countAdd() {
    console.log(i);
    document.getElementById('sec').innerHTML = i++;
    timer = setTimeout(countAdd, 1000);
}, 1000);

```

setTimeout会保证在当前线程的操作都结束之后等待设置时长结束立即执行下一次的操作。上面递归调用countAdd就可以保证两次操作的间隔为一秒钟，而无论这两次操作各自花费的时长是多久。

* setTiemout与线程

对于我来说，第一次了解到JavaScript关于线程相关的知识点是在启蒙老师问了一个关于JavaScript单线程和浏览器渲染阻塞的问题。

大致相关内容如下：


```
<!DOCTYPE html>
<html>
<head>
    <title>01</title>
</head>
<body>
 
    <div id="hel">hello</div>
 
    <script type="text/javascript">
        function Elem(id){
            this.elem = document.getElementById(id);
        }
        Elem.prototype.html = function(val){
            var elem = this.elem;
            if(val){
                elem.innerHTML = val;
                return this;
            }else{
                return elem.innerHTML;
            }
        };
 
        Elem.prototype.on = function(type, fn){
            var ele = this.elem;
            ele.addEventListener(type, fn);
            return this;
        };
 
        var div1 = new Elem('hel');
        div1.html('click me').on('click', function(){
            alert('u clicked me ');
        }).html('<p>hi clicked</p>');
 
         
    </script>
</body>
</html>

```
![image.png](https://upload-images.jianshu.io/upload_images/7930564-c9c5f8f303a9fe1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/7930564-e3dbfd16ced9d7cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/7930564-66a4f048ff3ee630.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

关于单线程阻塞的问题，现在我当然已经明白，但是借助setTimeout来调换线程上操作的执行顺序，比如将冒泡的事件改为捕获事件，不通过修改addEventListener来完成，到底该怎么做？这成为了我疑惑的点。

很幸运，在一篇帖子里找到了类似的回答。

```
//HTML
<div id="father">father

    <button id="son">son</button>
</div>
//JavaScript
document.getElementById('father').addEventListener('click', function () {

    console.log('this is father');

}, false);//冒泡


document.getElementById('son').addEventListener('click', function () {
    // setTimeout(function () {
    //     console.log('this is son');
    // }, 0);
    console.log('this is son');
}, false);//冒泡
```

在添加了setTimeout之后，将间隔设置为0，可以保证function内部的操作在当前线程上排列在所有操作结束之后再进行。这就可以保证，在触发子元素的事件时，将子元素的事件挪到所有操作的最后，也就是父元素的事件操作结束之后再去执行，所以即使addEventListener的第三个参数为false(冒泡)，执行的顺序仍然为捕获型。
