---
title: 使用 jQuery 同步执行多个 ajax 请求
entitle: synchronously-execute-multiple-ajax-requests-by-jQuery
categories:
  - coding
tags:
  - javascript
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: /images/default-photos.jpg
date: 2018-09-18 10:20:42
keywords:
description: 如果有多个 ajax 请求需要串行执行（按先后顺序执行），应该怎么办？使用 jQuery 有多种方式可以实现。
---

如果有多个 ajax 请求需要串行执行（按先后顺序执行），应该怎么办？
使用 jQuery 有多种方式可以实现。

## 简单实现
（1）嵌套在回调里面

```
$.ajax({
　　　　url: "test1.html",
　　　　success: function(){ 
　　　　　　alert("哈哈，成功了！"); 
                $.ajax({
　　　        　url: "test2.html",
　　　　success: function(){ 
　　　　　　alert("哈哈，成功了！"); 
　　　　},
　　　　　});
　　});
```

这种方法的好处是简单直观，但如果请求很多，就会形成“回调地狱”。

（2）使用同步模式

jQuery 里面的同步模式可以保证 ajax 同步进行。代码的顺序，就是 ajax 操作的执行顺序。
```
  $.ajax({
    url: "ajax请求1",
    async: false,
    success: function (data) {
      console.log("ajax请求1 完成");
    }
  });

  $.ajax({
    url: "ajax请求2",
    async: false,
    success: function (data) {
      console.log("ajax请求2 完成");
    }
  });

  $.ajax({
    url: "ajax请求3",
    async: false,
    success: function (data) {
      console.log("ajax请求3 完成");
    }
  });
```

这样避免了回调嵌套。

（3）使用 when 函数

when 函数提供一种方法来执行多个异步操作对象的回调函数，也就是说，可以在多个异步操作放在 when 函数里面，等它们执行完成之后，统一执行回调函数。但是，没法确定这其中多个异步操作的执行顺序。
下面这种写法不能保证先后顺序，当所有 ajax 操作执行完毕之后，就会执行done() 函数里面的内容。

```
$.when(
      $.ajax("test1.html"),
      $.ajax("test2.html")
    ).done(function (test1data, test2data) {
      alert("哈哈，成功了！");
    }
    ).fail(function () { alert("出错啦！"); });
```

需要设置 async 参数，才能保证 ajax 操作的先后顺序。

```
$.when(
    $.ajax({async:false,url:"test1.html”}), 
    $.ajax({async:false,url:"test2.html”})
    ).done(function(test1data,test2date){ 
        alert("哈哈，成功了！"); }
    ).fail(function(){ alert("出错啦！"); });
```

## 函数递归

当 ajax 操作里面的参数，需要批量生成，而不能事先列举的情况下，我们可以先创建 deffer 对象，放入数组中，然后再遍历这个数组，执行 ajax 操作。

下面的例子演示的是查询设备的数据，设备编号是存储在 diviceIds 数组中的。

```
//此函数用来生成 deffer 对象
var GetSomeDeferredStuff = function () {
    var deferreds = [];
    $.each(diviceIds, function (i, v) {
        var deviceid = v;
        deferreds.push(
            $.ajax({
                type:'GET',
                url:"getedata!parm",
                data:{tm1: startm, tm2: endtm, id: deviceid},
                dataType:"json"
            })
        )
    });
    return deferreds;
};
//执行这个函数，把所有包含 ajax操作的 deffer 对象都放在一个数组里面
var d = GetSomeDeferredStuff();  

//使用 when 函数使所有 ajax 操作并行执行，但不能决定其先后顺序。
 $.when.apply(null, GetSomeDeferredStuff()).done(function () {
    //所有异步数据加载后，开始绘图
    DrawChart(seriesData)
 });
```

如果想要所有 ajax 操作 按顺序执行，可以使用函数回调，把下一个 ajax 操作放在上一个 ajax 操作的 done 函数里面执行。

```
//串行执行，按先后顺序来
var action = function(d){
    if (d.length == 1){
        d[0].done(function(data){
            seriesData.push(data)
        })
    }else{
        arr = d.pop();  //从最后一个开始，依次往前
        //  arr =  d.shift();  // 从第一个开始，依次往后
        arr.done( function(data){
            seriesData.push(data)
           action(d);
        })
    }
};
action(d);

//所有异步数据加载后，开始绘图
 DrawChart(seriesData)
```

这种写法代码简单，结构清晰。