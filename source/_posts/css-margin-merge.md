---
title: CSS外边距合并的几种情况
entitle: 'css margin merge'
categories:
  - coding
tags:
  - CSS
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
date: 2018-01-13 21:11:27
keywords:
description: 外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。
photos: https://ws3.sinaimg.cn/large/006tNc79gy1fsuef2ol9pj30ah03u744.jpg
---

外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

外边距在[CSS1](https://www.w3.org/TR/CSS1/#vertical-formatting)中就有
>The width of the margin on non-floating block-level elements specifies the minimum distance to the edges of surrounding boxes. Two or more adjoining vertical margins (i.e., with no border, padding or content between them) are collapsed to use the maximum of the margin values. In most cases, after collapsing the vertical margins the result is visually more pleasing and closer to what the designer expects.

意思是说，非浮动块级元素的外边距宽度指定了其到环绕元素的边缘的最小距离。两个或更多的垂直外边距（比如，没有边框，内边距或内容在它们之间）就会发生合并，其值为它们中的最大者。在大多数情况下，在垂直方向发生外边距合并的结果，是设计师所喜闻乐见的。

按照这一设计思想，CSS外边距合并会在以下几种情况发生。

## 1 上下元素挨在一起

当一个元素位于另一个元素上面时，第一个元素的下外边距与第二个元素的上外边距会发生合并。合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者。

    <!-- sytle.css -->
    .div1{
    margin:10px; 
    border:1px solid gray;
    }
    .div2{
    margin:20px; 
    border:1px solid gray;
    }

    <div class='div1'>内容1</div>
    <div class='div2'>内容2</div>

结果是div1和div2的间距是20px。

## 2 子元素包含在父元素之中

当一个元素包含在另一个元素中时（假设没有内边距或边框把外边距分隔开），它们的上和/或下外边距也会发生合并。如果父元素有内边距或边框，则子元素的外边距以父元素的内边距或边框为准，不会发生外边距合并。

    <!-- sytle.css -->
    .div1{
    margin:10px; 

    }
    .div2{
    margin:30px; 
        border:1px solid gray;
    }

    <div class='div1'>
        <div class='div2'>内容2</div>
        内容1
    </div>

显示结果为：

![](https://ws3.sinaimg.cn/large/006tNc79gy1fsuef2ol9pj30ah03u744.jpg)

父元素div1和子元素div2的上外边距之间没有边框或内边距阻隔，导致两者的上外边距合并，从而导致父元素div1至其上部元素的距离变为30px（两者合并取大值）。如果父元素有上边框或上外边距，则不会发生外边距合并。

## 3 空元素的外边距合并

假设有一个空元素，它有外边距，但是没有边框或填充。在这种情况下，上外边距与下外边距就碰到了一起，它们会发生合并。

    <!-- sytle.css -->
    .div1{
    margin:10px; 

    }
    .div2{
    margin:10px; 
    border:1px solid gray;
    }

    <div class='div1'>内容1</div>
    <div class='div2'></div>
    <div class='div1'>内容1</div>

显示为：
![](https://ws3.sinaimg.cn/large/006tNc79gy1fsuef3oxawj30e702et8i.jpg)

元素div2的上下边距合并，边框变成了一条直线

## 注意

只有普通文档流中块元素的垂直外边距才会发生外边距合并。行内元素、浮动元素或绝对定位之间的外边距不会合并。因为CSS 的 外边距合并 设计之初就是为实现文本排版的段落间距而提供的特性。理解了这一设计意图，也就能理解哪些情况会发生外边距合并以及如何利用这一特性。但是明白它的用途就可以知道它是非常有用的、不可能被舍弃的特性。实际上这个特性还会进化，CSS3支持竖排文本，对于竖排文本来说，collapse应该发生在水平方向而不是垂直方向。

