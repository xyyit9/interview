---
title: 面试准备1-css
tags: 面试
categories: 面试
---

https://www.cnblogs.com/bigboyLin/p/5272902.html

### CSS实现垂直水平居中

https://www.cnblogs.com/xuemingyao/p/5829263.html

https://www.cnblogs.com/yugege/p/5246652.html

**1.盒子没有固定的宽和高**

方法1：内容块定义transform: translate(-50%,-50%)  必须加上top: 50%; left: 50%;

```
.wrapper {
            padding: 20px;
            background: orange;
            color: #fff;
            position: absolute;
            top: 50%;
            left: 50%;
            border-radius: 5px;
            -webkit-transform: translate(-50%, -50%);
            -moz-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
        }
```

方法2：flex方式，在父级元素上面加上上面3句话，就可以实现子元素水平垂直居中。

```
.wrapper {
            display: flex;
            align-items: center; /*定义body的元素垂直居中*/
            justify-content: center; /*定义body的里的元素水平居中*/
        }
```

**2.盒子有固定的宽和高**

方法1：设置位置为绝对位置,距离页面窗口左边框和上边框的距离设置为50%，这个50%就是指页面窗口的宽度和高度的50%,最后将该div分别左移和上移，左移和上移的大小就是该DIV宽度和高度的一半。

```
.wrapper {
            width: 400px;
            height: 18px;
            padding: 20px;
            background: orange;
            color: #fff;
            position: absolute;
            top:50%;
            left:50%;
            margin-top: -9px;
            margin-left: -200px;
        }
```

方法2：margin:auto实现绝对定位元素的居中

```
.wrapper {
            width: 400px;
            height: 18px;
            padding:20px;
            background: orange;
            color: #fff;

            position: absolute;
            left:0;
            right:0;
            top: 0;
            bottom: 0;
            margin: auto;
        }
```



### position有哪几个属性

position：static | relative | absolute | fixed

static：对象遵循常规流。此时4个定位偏移属性不会被应用。

relative：对象遵循常规流，并且参照自身在常规流中的位置通过[top](http://www.css88.com/book/css/properties/positioning/top.htm)，[right](http://www.css88.com/book/css/properties/positioning/right.htm)，[bottom](http://www.css88.com/book/css/properties/positioning/bottom.htm)，[left](http://www.css88.com/book/css/properties/positioning/left.htm)这4个定位偏移属性进行偏移时不会影响常规流中的任何元素。被设置元素相对于其原始位置而进行定位，其原始占位信息仍存在，相对定位是“相对于”元素在文档中的初始位置

absolute：对象脱离常规流，独立于其他对象而存在，没有占位。绝对定位是“相对于”离自身最近的定位祖先元素，如果没有定位的祖先元素，则一直回溯到`body`元素。

fixed：与`absolute`一致，但偏移定位是以窗口为参考。当出现滚动条时，对象不会随着滚动。

### CSS画三角形

```
.content {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-left: 50px solid red;
    border-right: 0px solid transparent;
    border-bottom: 0px solid red;
}
```

