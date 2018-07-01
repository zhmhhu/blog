---
title: Web程序员必备的CSS工具
entitle: 'CSS-tools-you-need-to-know'
categories:
  - resource
tags:
  - CSS
  - NiceTools
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 在科技和人文的世界里翱翔
date: 2018-01-30 17:50:45
keywords:
description: WEB程序员会经常发现自己的或别人的CSS文件里有大量的冗余代码或错误或能够大量优化的地方。如果你经常使用静态编程语言(比如，Java，C语言)等，你会发现实用的IDE工具会给编程带来巨大的效率
photos: https://ws2.sinaimg.cn/large/006tNc79gy1fsuef6rzydj30fa0ct4d3.jpg
---

对于 web 开发来说，CSS 是最有效的美化页面、设置页面布局的技术。但问题是，CSS 是一种标记性语言，语法结构非常的松散、不严谨。WEB程序员会经常发现自己的或别人的 CSS 文件里有大量的冗余代码或错误或能够大量优化的地方。如果你经常使用静态编程语言(比如，Java，C 语言)等，你会发现实用的 IDE 工具会给编程带来巨大的效率，像 Eclipse 这样的能够实时自动分析代码问题的集成开发环境就是一个典型的例子。那么，CSS 程序员有没有这样的帮助工具呢？

下面将要介绍的 10 款工具都是一些在线的应用，你不需要将它们安装到自己的电脑上，只要能联网，你就可以使用它们。大部分的这些工具使用起来都非常的简单，但也有需要技巧的地方。(部分需科学上网才可使用)

# CSS问题检查工具：[CSS Lint](http://csslint.net/) 
CSS Lint 是一个开源的校验CSS文件质量的工具，最初是由 Nicholas C. Zakas和 Nicole Sullivan编写的，最初版本在Velocity会议上于2011年6月发布。CSS Lint的检测规则包括错误的和警告，当选择器或属性书写不正确、漏掉了大括号、多写了分号等时，会提示解析错误，解析错误优先提示。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fsuef6rzydj30fa0ct4d3.jpg)

# CSS代码分析统计工具：[CSS Stats](http://cssstats.com/)
Css Stats 是一款在线 CSS 代码分析工具，输入网站 CSS 网址即可进行 CSS 代码分析。Css Stats 是前端网页设计师分析网站 CSS 代码的利器，可以统计CSS代码里的规则、字体大小、宽度高度、颜色数等等。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fsuef99xm7j30fa0ctwhi.jpg)

对于网页设计师而言分享网页CSS代码是必须要做的事情，统计网站设计里使用了多少种字体、多少种字体大小、多少种颜色、背景颜色有多少种，只有对CSS代码有一个详细的统计数字才能分析出来整个网站设计出来以后的效果。Css Stats还提供热门网站的CSS分析数据，例如谷歌、雅虎、Twitter、FaceBook、Tumblr等网站。

# CSS代码优化压缩工具：[CSS Shrinks](http://cssshrink.com/)
CSS Shrinks 能够非常明显的压缩你的CSS体积大小。很多Web程序员编写的CSS代码里有大量的冗余语法，空白空间等，这款工具能在不影响你的CSS的正确性的情况下，优化CSS的语法，去除无用的空格和空行，显著的压缩CSS的提交，大量的减少带宽的浪费。

![](https://ws1.sinaimg.cn/large/006tNc79gy1fsuef9vr48j30fa0ctk19.jpg)


# CSS代码整理工具：[ProCSSor](http://tools.maxcdn.com/procssor/)
ProCSSor 除了提供基本优化CSS代码功能，还提供了大量的自定义选项。比如，让你设定CSS规则，CSS属性，CSS语法的优化选项。它还提供了对新型CSS3属性、规则中各种浏览器里的不兼容替代方案。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fsuefapz22j30fa0ctgth.jpg)

#  CSS 语法参考 [Codrops](http://tympanus.net/codrops/css_reference/)
Codrops CSS 参考内容丰富而全面，并且界面清爽直接，你可以使用这个工具掌握CSS里最重要而全面的知识。它的CSS知识库分成了数个类别，包括伪类，属性，函数，数据类型，概念，规则等。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fsuefbr4m7j30fa0ddwim.jpg)

# CSS3浏览器兼容支持情况查询工具：[Can I Use](http://caniuse.com/#cats=CSS)
"Can I Use"在这里你能找到所有web新特性在各个品牌浏览器以及各品牌浏览器不同版本的兼容性，当你知道你针对的用户都在使用什么浏览器时，这写table将对你建设网站有很大帮助。打开 caniuse.com，该网站首页将所有HTML5、CSS3等web新特性罗列出来，如果你想查看某个特性在不同浏览器种的兼容情况，点击一下就可以。比如，看一下 @font-face Web fonts在各个浏览器中的兼容性，点击CSS区域中的第一项，会看到一个表格，列出所有浏览器的版本，用不同颜色代表每个浏览器对 @font face Web fonts的支持，被标识为红色的代表不支持，浅绿色代表部分支持。图中列出的浏览器还包括一些手机平板设备浏览器，例如Android系统浏览器。如此全面，设计网站时，可以根据网站针对的用户有选择的使用CSS和Javascript的高级特性，提高用户体验。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fsuefckpulj30fa0ajgq7.jpg)

# 检查你的代码是否符合CSS标准：[W3C CSS Validation Service](https://jigsaw.w3.org/css-validator/)
这个工具是用来检查你的CSS语法是否正确，是否符合W3C CSS标准。我们知道从最早的IE开始，各种浏览器都实现了一些自己的方言，这些方言中在各种浏览器里互不相通。但我们开发网页时，必须最大限度的考虑各种浏览器的兼容性，最好的方法是遵守W3C的CSS标准规范。W3C CSS Validation Service就是用来校验你的css中的问题，它会提醒你那些语法，哪些属性，那些规则是有问题的。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fsuefdkix3j30fa09a76p.jpg)

# 在线编码 [CodePad](http://codepad.co/)
Codepad.org是一个很有意思的网站，它的主页很简单，左边是可以编译并执行的程序语言，右边则是让你输入程序的输入框，输入框的下面是一个"Run Code"的复选钮和一个"Submit"的提交按钮。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fsuefdyw8rj30fa077q5y.jpg)

其操作起来也非常简单，先选中你要编译并运行的程序语言，然后在输入框中粘贴或输入程序的原代码，然后，点击提交，你就可以看么你程序编译出错的提示，或是执行的结果。

也许，你会觉得很无聊天，但我觉得这在某些时候会非常有用，尤其是你找不到编译器而又想验证一段代码的时候，这种时候还是比较多的。特别是我们很难有一台可以运行所有语言的电脑，如果有的话，那一定是你自己的个人电脑，当你不使用你自己的电脑时，你就会着急了。而且，我觉得这项服务非常地有意思，因为，这样一来，你甚至可以在你的手机上写任何语言的程序了。

目前这个网站支持下面这样语言——C，C++，D，Haskell，Lua，OCaml，PHP，Perl，Plain Text，Python，Ruby，Scheme，Tcl。

# CSS动画生成工具：[Gradient Animator](http://www.gradient-animator.com/)
这是一款使用CSS Gradient和CSS Animation技术实现的动态背景生成工具。工具非常易用，轻松地点击几下鼠标，一个现代感十足的渐变动态背景代码就生成了。它可以让CSS渐变背景平滑地移动，我们可以设置移动的角度，移动的速度，渐变的角度。支持所有现代浏览器以及 ie 10+。非常适合做网页开屏背景。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fsuefesahfj30fa09rgp4.jpg)

# Web颜色工具：[CSS Colours](http://colours.neilorangepeel.com/)
故名思议，这个工具是用来方便Web设计者查找颜色的，它提供了各种颜色的视觉效果，对于的颜色的英文名称，RGB值，16进制值，使用起来非常的方便。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fsueffswhlj30fa054aan.jpg)


来源：http://www.webhek.com/10-css-tools