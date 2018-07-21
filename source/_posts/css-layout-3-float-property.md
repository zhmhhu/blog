---
title: CSS 布局（3）——float属性
entitle: css-layout-3-float-property
categories:
  - coding
tags:
  - CSS
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws2.sinaimg.cn/large/006tNc79gy1fthwl2vl8rj30uq0jwqa5.jpg
date: 2018-07-21 20:41:31
keywords:
description: 浮动是很有意思的一种布局方式，在布局中被广泛的应用，可是初学者并不容易掌握。
---

浮动是很有意思的一种布局方式，在布局中被广泛的应用，可是初学者并不容易掌握。

## 浮动元素的特性
float 属性使元素向左或向右方向浮动。这个属性的设计之初是应用于图像，使文本围绕在图像周围，不过在 CSS 中，任何元素都可以浮动。
我们根据下面这个例子来总结浮动元素的特性
```
<style>
	#panel{
		width: 200px;
		height: 200px;
		border: green solid 1px;
		}
	#left1{ 
		float:left;
		height: 100px;
		width: 100px;
		background: #000;
		color: #ffa;
	}
  #left2{
    float:left;
    height: 50px;
    width: 50px;
    background: #bba;
  }
  #left3{
    float:left;
    height: 30px;
    width: 30px;
    background: #a3a;
  }
  #left4{
    float:left;
    height: 60px;
    width: 60px;
    background: #a92;
  }
  #left5{
    float:left;
    height: 30px;
    width: 30px;
    background: #ff0;
  }
  #right1{
    float:right;
    height: 30px;
    width: 30px;
    background: #ff0;
  }
  #right2{
    float:right;
    height: 60px;
    width: 70px;
    background: #8f0;
  }
  </style>
  <div id="panel">
    <div id="left1">left1</div>
    <div id="left2">left2</div>
    <div id="left3">left3</div>
    <div id="left4">left4</div>
    <div id="left5">left5</div>
    <div id="right1">right1</div>
    <div id="right2">right2</div>
  </div>
```

显示效果为

![](https://ws2.sinaimg.cn/large/006tNc79gy1fthvne2gptj30bo0bkt90.jpg)

1. 浮动元素会生成一个块级框，即`display:inline-block`，而不论它本身是何种元素。浮动元素需要指定一个明确的宽度；否则，它们会尽可能地窄。

2. 假如在一行之上只有极少的空间可供浮动元素，那么这个元素会跳至下一行，这个过程会持续到某一行拥有足够的空间为止。比如例子中的 left4。

3. 如果浮动元素跳过了这一行，那么浏览器就认为这一行没有足够的空间，其他div在浮动过程中会自动跳过这一行，即使该行还有空间容纳其他div。比如例子中的 left5，原本在紧挨着 left3 的下方还有足够的空间，但是因为 left4 跳过了这一行，于是 left5 就认为这一行没有足够的空间。

4. 浮动元素是脱离文档流的，脱离文档流的元素没有实际高度，不会把父元素撑开。对于浮动属性来说，位置属性（left/top/right/bottom）是没有用的。

5. 浮动元素之后的文字内容，如果是汉字则会自动换行； 如果是连续的数字或英文字母，则不会换行，设置word-wrap : break-word;属性会使内容强制换行。

上面的例子改为
```
<div id="panel">
	<div id="left1">left1left1left1left1</div>
	<div id="left2">left2</div>
	<div id="left3">left3</div>
	<div id="left4">left4我在左边</div>
	<div id="left5">left5</div>
	<div id="right1">right1</div>
	<div id="right2">right2</div>
</div>
```
显示效果为

![](https://ws1.sinaimg.cn/large/006tNc79gy1fthw1ah5v9j30c20bigm3.jpg)

left1 中的内容没有换行而是直接被遮住了，left4 中的汉字则换行了。

## 高度塌陷
前面提到过，浮动元素是脱离文档流的，它没有实际的高度，这种结果常常会造成父元素的高度塌陷，即父元素框并没有产生包裹它子元素的应有高度。
看下面的例子
```
<div style="border:4px solid blue;">
  <img src="1.jpeg" />
</div>
  <div style="border:4px solid red;">
    <img style="border:4px solid green;float:left;" src="1.jpeg" />
  </div>
<div >啦啦啦</div> 
```

![](https://ws2.sinaimg.cn/large/006tNc79gy1fthwl2vl8rj30uq0jwqa5.jpg)

原本应该位于绿色边框图片下方的文字，却因为红色边框的高度为0，而位居其右侧了。这不是我们需要的结果。解决这个问题的办法就是清除浮动。

## 清除浮动
使用clear 属性可以清除元素由于浮动带来的问题。例如设置`clear:left` 可以清除元素左边的浮动。
稍微要注意的是，clear是仅作用于当前元素，例如元素A是浮动元素，靠左显示。元素B是block元素紧跟在A后面。此时要清除浮动，应该在B上设clear:left。在A上设clear:right是没有用的，因为A的右边没有浮动元素。

上面的例子中，修改如下代码可使文字在最下方，但是红色边框的元素高度仍然不会改变。
```
<div  style="clear:left;">啦啦啦</div> 
```
此时，虽然文本正常显示了，但是红色边框的元素仍然没好有获得正确的高度，它的高度仍然处于塌陷状态。此时应该使用闭合浮动。

闭合浮动的实现方法很多，常见的是最后增加一个清除浮动的子元素
方法一：

```
<div style="border:4px solid blue;">
  <img src="1.jpeg" />
</div>
<div style="border:4px solid red;">
  <img style="border:4px solid green;float:left;" src="1.jpeg" />
  <div style="clear:both;"></div>  //加上空白div节点来闭合浮动
  </div>
<div >啦啦啦</div> 
```

缺点是会增加一个DOM节点。

方法二：同样可以在最后增加一个清除浮动的br：将上面代码中`<div style=”clear:both;”></div>`替换成`<br clear=”all” />`即可。语义上比空的div标签稍微好一点，但同样会增加一个DOM节点。

## 总结

下面总结一下浮动元素和绝对定位的区别。

浮动元素设计之初是为了使文本环绕图像排版，而后来的CSS允许浮动任何元素。理解了这一设计原则，就能理解它与其他定位方式的区别了。

1. 绝对定位是将元素彻底从文档流删除，并相对于其包含块定位（包含块可能是文档中的另一个元素或者是初始包含块），元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样，该元素再也不会影响其他元素的布局了。
2. 浮动，会以某种方式将浮动元素从文档的正常流中删除，并把浮动元素向左边和右边浮动，不过它们还是对文档的其余部分有影响。使用清除浮动可以消除这种影响。
