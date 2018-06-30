---
title: jgGrid中的editrules使用函数来进行验证
entitle: 'jqgrid-editrules-validate-by-function'
date: 2018-06-30 18:11:08
categories:
  - big-front-end
tags:
  - javascript
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 在科技和人文的世界里翱翔
keywords:
description:
photos: http://img.blog.csdn.net/20151111193548966
---

jgGrid 中的 editrules 用于设置一些用于可编辑列的 colModel 的额外属性，大多数的时候是用来在提交到服务器之前验证用户的输入合法性的。editrules 中关于 custom_func 的用法，网上给出的例子都不完整，让人看了一头雾水。这里给出一个完整的例子。

 
## editrules的属性
可选的属性包括：
  - edithidden：只在Form Editing模式下有效，设置为true，就可以让隐藏字段也可以修改。
  - required：设置编辑的时候是否可以为空（是否是必须的）。
  - number：设置为 true，如果输入值不是数字或者为空，则会报错。
  - integer：
  - minValue：
  - maxValue：
  - email：
  - url：检查是不是合法的URL地址。
  - date：
  - time：
  - custom：设置为 true，则会通过一个自定义的 js 函数来验证。函数定义在 custom_func 中。
  - custom_func: 传递给函数的值一个是需要验证 value，另一个是定义在 colModel 中的 name 属性值。函数必须返回一个数组，一个是验证的结果，true 或者 false，另外一个是验证错误时候的提示字符串。形如 [false,”Please enter valid value”] 这样。

## custom_func 中的函数
关于custom_func的用法，网上给出的例子都不完整，让人看了一头雾水。这里给出一个完整的例子。

函数的作用是，判断是不是合法的手机号，如果是，则返回true，代码继续运行，如果否，则提示“不是完整的11位手机号或者正确的手机号格式”，jqgrid继续停留在编辑页面。

 ```
name : 'mobile',
index : 'mobile',
editable: true,
editrules : {required : true},
editrules:{
required : false,
custom:true, 
custom_func:function(value, colNames){    
   if(!(/^(1[3-9])\d{9}$/.test(value))){
       return [false, "不是完整的11位手机号或者正确的手机号格式"];  
   }else{
   return [true,""];
   }   
}
```

运行结果如下：

![](http://img.blog.csdn.net/20151111193548966)

当custom_func返回false时，对话框中显示错误提示字符串。


 