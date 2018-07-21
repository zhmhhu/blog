---
title: CSS布局（1）——display属性
entitle: css-layout-1-display-property
categories:
  - coding
tags:
  - CSS
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws1.sinaimg.cn/large/006tNc79gy1fthhici6q4j30k00fs3z6.jpg
date: 2018-07-21 14:11:10
keywords:
description: 布局的传统解决方案(CSS3以前)，基于盒状模型，依赖 display 属性 + position属性 + float属性。在这一系列里分别介绍。
---
布局的传统解决方案(CSS3以前)，基于盒状模型，依赖 display 属性 + position属性 + float属性。在这一系列里分别介绍。

display 属性规定了元素以何种方式显示，它的值有很多。随着从CSS1到CSS2.1在到CSS3的不断推进，w3c 给 display 属性赋予了越来越多的值，元素显示的表现形式也越来越多样化，下面主要简介常用的CSS2.1中的属性，文章最后提一下CSS3中的规范，以后会有专门的文章讲解CSS3中的布局。

CSS2.1中，display有以下取值
![](https://ws1.sinaimg.cn/large/006tNc79gy1fthhici6q4j30k00fs3z6.jpg)

但是并不是每一种都会用到，在CSS布局设置中，常用的属性有：none、block、inline、inline-block、inherit等等，其中inherit是继承父元素的样式，不用多说，其他的几个会在下文详解。

## display: none

将元素与其子元素从普通文档流中移除。这时文档的渲染就像元素从来没有存在过一样，它所占据的空间也会被折叠。在可以阅读屏幕内容的设备上，其内容也会被忽略。

## inline

该元素生成一个或多个行内框，这样的元素我们称为**行内元素**。所占具的空间就是他的标签所定义的大小。

常用的 inline 就是文字和图片，它没有大小和形状，其宽度取决于父容器的宽度。
因此，针对 inline 的标签，设置宽度和高度是无效的，通过查看 user agent stylesheet 可以知道，该元素实际的宽度和高度都是 auto ，并不是我们设定的值。

行内元素特点：
1. 它不会单独占据一整行，而是只占领自身的宽度和高度所在的空间。若干同级行内元素会从左到右（即某个行内元素可以和其他行内元素共处一行），从上到下依次排列。
2. 行内元素不可以设置高度、宽度，其高度一般由其字体的大小来决定，其宽度由内容的长度控制。
3. 行内元素只能设置左右的margin值和左右的padding值，而不能设置上下的margin值和上下的padding值，就算设置了也是无效的。
4. 常见的行内元素有`<a><em><img>`等等，这些元素在 user agent stylesheet 已经设定其 display 属性值为 inline。

## block
设置了此属性的元素将显示为**块级元素**。上一篇中讲解了盒子模型，对于block，其实就是“盒子模型”。一个元素设置了block，它就必须遵循盒子模型的规则。因此，这里也不再去详细写它了。

块级元素特点：
1. 总是以一个块的形式表现出来，占领一整行。若干同级块元素会从上之下依次排列（使用float属性除外）。
2. 可以设置高度、宽度、各个方向外边距（margin）以及各个方向的内边距（padding）。
3. 当宽度（width）缺省时，它的宽度时其容器的100%，除非给它设定了固定的宽度。
4. 块级元素中可以容纳其他块级元素或行内元素。
5. 常见的块级元素有`<p><div><h1><li>`等等,这些元素在 user agent stylesheet 已经设定其 display 属性值为 block。

![](https://ws1.sinaimg.cn/large/006tNc79gy1fthgjg6r7wj30d1050dfu.jpg)

## inline-block
这是CSS2.1中新增的属性（与CSS1相比）。设置了此属性的元素被称为**行内块元素**。读者可以自己查看一下 user agent stylesheet，哪些元素被默认设置为行内块元素。

inline-block 结合了 inline 和 block 两者的特性。综合起来，特点如下
1. 不会单独占据一行，从左到右，从上到下依次排列
2. 可以设置高度和宽度，设置其margin值和padding值也有效。
3. 同一行中有多个inline-block元素或inline元素和inline-block元素并存时，会形成一个line boxes，这个line box的高度由里面最高的元素决定。
4. 常见的行内块元素有 `<button><input><select>` 等等

拿button举例子。我们在页面中输入若干个`<button>`，发现它们是按照文档流
排列的。但是针对一个button，我们还可以自定义修改它的形状，这样就有“块”的特征。

## display: list-item;
该元素生成一个块状盒，随后再生成一个列表型的行内盒。其效果就和`<li>`和`<ul>`中出现项目列表符号一样。

## 表格布局
CSS2.1 中专门定义了一系列的display属性用于表格布局

| 值 | 描述 |
| ------ | ------ | 
| table | 这个元素的作用就像  `<table>` 元素. 它定义了一个块级盒子 |
| table-caption	 | 这个元素的作用就像  `<caption>` 一样 |
| table-cell | 这个元素的作用就像  `<td>` 一样 |
| table-column | 这个元素的作用就像  `<col>` 一样 |
| table-column-group | 这个元素的作用就像  `<colgrop>` 一样 |
| table-footer-group | 这个元素的作用就像  `<tfoot>` 一样 |
| table-header-group | 这个元素的作用就像  `<thead>` 一样 |
| table-row | 这个元素的作用就像  `<tr>` 一样 |
| table-row-group	 | 这个元素的作用就像  `<tbody>` 一样 |
| inline-table | inline-table值在HTML中没有对应元素。它的行为就像一个HTML中的table元素，但是它是一个内联框，而不是块级框。| 

现在大多数的人都不再使用基于表格的布局，但是 display: table 在一定的情况下还是十分有用的。例如，想要在大的布局上使用表格，但是在在较小宽度上保持典型的块布局。这可以同时结合媒体查询与display(为了采取更好的措施，使用伪元素)进行实现，仅需要调整窗口的大小观看实现的效果。

## 其他display属性值

CSS3 中引入了两个重要的 display 属性来进行布局，一个是讲元素设置为Flex盒模，另一个是讲元素设置为栅格(Grid)盒模型。

Flex盒模型值包括两个
```display: flex;
  display: inline-flex;
```
flex:该元素的行为类似于块级元素，并根据Flex盒模型来描述其内容。
inline-flex:该元素的行为类似于行内块元素，并根据Flex盒模型来描述其内容。


栅格盒模型值也包括两个
```display: grid;
  display: inline-grid;
```
其中，设置为 grid 的元素其行为类似于块级元素，设置为 inline-grid 的元素其行为类似于行内块元素。

以后会专门讲解 Flex 和 Grid 这两种布局。
　　