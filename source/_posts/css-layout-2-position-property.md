---
title: CSS 布局（2）——position属性
entitle: css-layout-2-position-property
categories:
  - coding
tags:
  - CSS
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws3.sinaimg.cn/large/006tNc79gy1ftfgo14ipxj30b90793z6.jpg
date: 2018-07-21 20:01:27
keywords:
description: CSS 中的 position 属性规定了元素的位置及位置参照方式。
---
CSS 中的 position 属性规定了元素的位置及位置参照方式。

position 有六个属性值：static、relative、absolute、fixed、sticky和inherit。其中inherit是继承父元素的样式，不用多说；sticky是CSS3新增的属性值，其使用范围不多，多数浏览器的兼容性不是很好，本文不做详述；其他的几个在下文详解。

## static
CSS 中的默认定位样式，一般可以不用设置。 各个元素是按照文档流的形式从左往右，从上往下排，同时根据其display属性，保证块级元素独占一行的原则。

## relative
生成相对定位的元素，相对于其正常位置进行定位，同时配合top, bottom, left 和 right 样式完成定位。它不会影响同一容器中其他元素的定位。

相对定位元素并没有脱离文档流，其参造物是其本身，不管你怎么移动，它原有的位置还是会留着，父容器对其布局影响照旧。

## absolute
对元素进行绝对定位，具体如何定位需要根据其父元素及祖先元素的定位情况来说。

1. 父元素中有设置了Position属性，并且属性值为 absolute 或 relative的时候，那么子元素相对于该父元素来进行定位。
2. 父元素没有设置非static的Position属性，则以其第一个设置了非static的Position属性的祖先元素来进行定位。如果不存在一个有着position属性的祖先元素，那么那就会以body为定位对象，按照浏览器的窗口进行定位。

绝对定位是将元素彻底从文档流删除，并相对于其第一个设置了非 static 的定位属性的祖先元素进行定位，元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样，该元素再也不会影响父元素中其他元素的布局了。

## fixed
设置了 fixed 定位属性的，其位置相对于视窗进行定位。
元素框不占有文档流的位置，并且不影响其他元素的定位。

## sticky
最后提一下sticky定位，它是CSS3新增的属性值，粘性定位，它就相当于relative和fixed混合。最初会被当作是relative，相对于原来的位置进行偏移；一旦超过一定阈值之后，会被当成fixed定位，相对于视口进行定位。一般用于长页面向下滑动时，固定标题栏或菜单栏，查看[demo](https://jsbin.com/moxetad/edit?html,css,output)

再看下面这个[网易新闻移动端首页](https://3g.163.com/touch/news?version=v_standard)的例子，导航栏用了粘性定位，使其内容上滑到一定程度之后，固定在视窗上。
![](https://ws1.sinaimg.cn/large/006tNc79gy1fthrdue138g30ao0ig4qr.gif)

这一定位属性的兼容问题比较麻烦，目前网易移动端已经去掉了这个样式。

## 总结
设置了 absolute 和 fixed 定位属性的元素，其内容将充文档流中删除，不会对其他元素的位置造成影响，设置了 relative 定位属性的元素只是相对于它本来的位置进行定位，它原来在文档流中占据的位置仍然存在。