---
title: CSS中的定位与浮动
entitle: ''
categories:
  - coding
tags:
  - CSS
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
date: 2018-01-24 21:50:27
keywords:
description: 本文主要讲述CSS中的三种定位样式static、relative和absolute的区别以及浮动元素的特征。
photos: https://ws1.sinaimg.cn/large/006tNc79gy1fsuef417c7j308n03bmwz.jpg
---

本文主要讲述CSS中的三种定位样式static、relative和absolute的区别以及浮动元素的特征。

## 定位样式
CSS中定位样式position的取值有三个，默认值：static，相对定位：relative，绝对定位：absolute。

1. static： 采用默认定位时，一般可以不用设置position样式。 各个元素是按照文档流的形式从上往下排，同时保证块状元素独占一行的原则。

1. relative : 生成相对定位的元素，相对于其正常位置进行定位，同时配合top, bottom, left和right样式完成定位。它不会影响同一容器中其他元素的定位。
相对定位元素并没有脱离文档流，其参造物是其本身，不管你怎么移动，它原有的位置还是会留着，父容器对其布局影响照旧。

1. absolute：对于绝对定位的元素，则需要分其的父元素及以上元素的定位情况来说：

    1. 父元素中有设置了Position属性，并且属性值为 absolute 或 relative的时候，那么子元素相对于该父元素来进行定位。
    1. 父元素没有设置非static的Position属性，则以其第一个设置了非static的Position属性的祖先元素来进行定位。如果不存在一个有着position属性的祖先元素，那么那就会以body为定位对象，按照浏览器的窗口进行定位。 
绝对定位是将元素彻底从文档流删除，并相对于其父元素定位（父元素可能是文档中的另一个元素或者是初始父元素，比如body），元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样，该元素再也不会影响父元素中其他元素的布局了。

## 浮动元素

1. 假如在一行之上只有极少的空间可供浮动元素，那么这个元素会跳至下一行，这个过程会持续到某一行拥有足够的空间为止。

2. 浮动元素会生成一个块级框，而不论它本身是何种元素。如果浮动非替换元素，则要指定一个明确的宽度；否则，它们会尽可能地窄。

3. 如果浮动元素跳过了这一行，那么浏览器就认为这一行没有足够的空间，其他div在浮动过程中会自动跳过这一行，即使该行还有空间容纳其他div。

4. 浮动元素是脱离文档流的，脱离文档流的元素没有实际高度，不会把父元素撑开。对于浮动属性来说，位置属性（left/top/right/bottom）是没有用的。

5. 浮动元素之后的文字内容，如果是汉字则会自动换行； 如果是连续的数字或英文字母，则不会换行，设置word-wrap : break-word;属性会使内容强制换行。

看下面的例子。

    <style> 
    #container {  
    position:relative;  
    width:300px;  
    height:100px;  
    background:#039;  
    }  
    #A {  
    float:left;  
    top:0;  
    left:0;  
    width:50px;  
    height:40px;  
    background:#C00;  
    }  
    </style>
    </head>
    <body>  
    <div id="container">  
    <div id="A">你好</div>  
    <div id="B">  
    1234567890abcdabcdabcdabcdabcdabcdabcdabcd</div>  
    </body> 


 ![图片](https://ws1.sinaimg.cn/large/006tNc79gy1fsuef417c7j308n03bmwz.jpg)

 原本应该环绕在红色块周围的文字因不能换行而显示在其下方，并且部分文字被容器遮住了。

### 浮动元素和绝对定位的区别

浮动元素设计之初是为了使文本环绕图像排版，而后来的CSS允许浮动任何元素。理解了这一设计原则，就能理解它与其他定位方式的区别了。

1. 绝对定位是将元素彻底从文档流删除，并相对于其包含块定位（包含块可能是文档中的另一个元素或者是初始包含块），元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样，该元素再也不会影响其他元素的布局了。
2. 浮动，会以某种方式将浮动元素从文档的正常流中删除，并把浮动元素向左边和右边浮动，不过它们还是对文档的其余部分有影响。当一个元素浮动时，其他内容会“环绕”该元素。

看下面的例子

    <style>
    body {  
        color:#FFF;  
    }  
    #container {  
        position:relative;  
        width:300px;  
        height:100px;  
        background:#039;  
    }  
    #A {  
        position:absolute;  
        top:0;  
        left:0;  
        width:50px;  
        height:40px;  
        background:#C00;  
    }  
    </style>
    </head>
    <body>  
    <div id="container">  
        <div id="A">你好</div>  
        <div id="B">  
        浮动元素和绝对定位的区别浮动元素和绝对定位的区别浮动元素和绝对定位的区别浮动元素和绝对定位的区别</div>  
    </body> 

![图片1](https://ws4.sinaimg.cn/large/006tNc79gy1fsuef4is7nj308y03da9w.jpg)

把A元素的定位改成浮动后，其后的内容会环绕该元素。

     #A {  
        float:left;  
        top:0;  
        left:0;  
        width:50px;  
        height:40px;  
        background:#C00;  
    } 

![图片2](https://ws4.sinaimg.cn/large/006tNc79gy1fsuef5iqgrj308r035dfo.jpg)

下面顺便引出**文本流和文档流**的概念。

文档流是相对于盒子模型讲的。

文本流是相对于文子段落讲的。

元素浮动之后，会让它跳出文档流，也就是说当它后面还有元素时，其他元素会无视它所占据了的区域，直接在它身下布局。但是文字却会认同浮动元素所占据的区域，围绕它布局，也就是没有拖出文本流。

但是绝对定位后，不仅元素盒子会拖出文档流，文字也会出文本流。那么后面元素的文本就不会在认同它的区域位置，会直接在它后面布局，不会在环绕。