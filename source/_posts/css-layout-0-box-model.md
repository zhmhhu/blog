---
title: CSS布局（0）——盒子模型
entitle: css-layout-0-box-model
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
date: 2018-07-19 20:43:16
keywords:
description: JavaScript大行其道的今天，CSS往往被人们忽视。虽说大多数 UI 框架帮助开发者节省了调整样式的时间，但是如果想要进行灵活的布局或者作出漂亮的有个性的界面，精通CSS必不可少。
---

JavaScript大行其道的今天，CSS往往被人们忽视。虽说大多数 UI 框架帮助开发者节省了调整样式的时间，但是如果想要进行灵活的布局或者作出漂亮的有个性的界面，精通CSS必不可少。

下面主要梳理一下CSS的布局，在此之前，先从盒子模型开始说起。

##盒子模型

在DOM文档中，每元素都被表示为一个矩形的盒子。设置这些盒子的尺寸、背景、边框、和位置等等，就可以控制元素在浏览器中的如何显示。

在CSS中，使用标准盒模型来描述这些矩形盒子。盒模型描述了元素所占空间的内容。每个盒子有四块区域：外边距（margin）, 边框（border）, 内填充（padding） 与 内容（content）。

元素框的最内部分是实际的内容，直接包围内容的是内边距。内边距呈现了元素的背景。内边距的边缘是边框。边框以外是外边距，外边距默认是透明的，因此不会遮挡其后的任何元素。

内边距、边框和外边距都是可选的，默认值是零。但是，许多元素将由用户代理样式表（user agent stylesheet ）设置外边距和内边距。

 user agent stylesheet 是浏览器默认样式表，在写网页时，如果元素没有指定样式，则按浏览器内置的样式表来渲染。由于不同浏览器内置样式不同，会导致实际的显示效果差别很大。

可以通过将元素的 margin 和 padding 设置为零来覆盖这些浏览器样式。这可以针对具体元素分别进行，也可以使用通用选择器对所有元素进行设置
```
* {
  margin: 0;
  padding: 0;
}
```

## W3C 标准盒模型

标准的盒模型中，元素的宽度等于内容的宽度，元素占据的总宽度实际上是内容、内边距、外边距和边框的和。

![](https://ws3.sinaimg.cn/large/006tNc79gy1ftfgo14ipxj30b90793z6.jpg)

box width = content width

box height = content height，

## 怪异模式中的盒模型

在CSS发展的过程中，对于盒模型，针对高度和宽度的定义，不同浏览器的解释不同。早期的 IE 浏览器（IE 6 以前）将盒子的 padding 和 border 算到了盒子的尺寸中，这一模型被称为 IE 盒模型。

![](https://ws4.sinaimg.cn/large/006tNc79gy1ftfi9km17zj30b9079wf5.jpg)

在 IE 盒模型中

box width = content width + padding left + padding right + border left + border right

box height = content height + padding top + padding bottom + border top + border bottom

这种情况会在文档没有指定 doctype 或形式不正确时发生。因为浏览器无法确认文档是不是标准的 DOM 文档，也就无法按照 w3c 的标准对其渲染，因此它将使用浏览器自己的方式解析文档。
这一区别将导致页面绘制时所有的块级元素都出现很大的差别。因此，我们把使用浏览器自己的方式解析执行代码，从而出现不同执行结果的情况称之为怪异模式（Quirks Mode）。

## box-sizing 属性
为了更好的避免这种现象的发生，在CSS3中，使用box-sizing 属性来规定用于计算元素宽度和高度的默认的 CSS 盒子模型。可以使用此属性来模拟不正确支持CSS盒子模型规范的浏览器的行为。
```
/* 关键字 值 */
box-sizing: content-box;
box-sizing: border-box;
```
box-sizing 属性用来设置使用哪种方式来计算盒子的高度和宽度。
* content-box，默认值，即w3c标准盒模型。元素的宽度和高度等于内容的高度和宽度，如果设置一个元素的宽为100px，那么这个元素的内容区会有100px宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素总宽度中。
* border-box，这种情况下，边框和内边距的值是包含在width内的，这与上面提到的IE盒模型相同。如果将一个元素的width设为100px,那么这100px会包含它的border和padding。大多数情况下这使得我们更容易的去设定一个元素的宽高。

默认情况下，设置一个元素的 width 与 height 只会应用到这个元素的内容区。如果这个元素有任何的 border 或 padding ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点鉴于上面的特点，一些专家甚至建议所有的 Web 开发者们应当将所有的元素的 box-sizing 都设为 border-box。