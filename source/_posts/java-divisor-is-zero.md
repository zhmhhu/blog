---
title: java语言中除数为零问题
entitle: 'java-divisor-is-zero'
date: 2016-01-13 16:13:00
categories:
  - server-side
tags:
  - java
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 在科技和人文的世界里翱翔
keywords: 
description: java 语言中除数为零的问题，会因为数据类型的不同而产生几种结果。
photos: /images/default-photos.jpg
---

java 语言中的除数为零，会因为数据类型的不同而产生几种不同的结果

## 例子
在以下几个例子中，输出结果如何？

```
float aa=0;
System.out.println(aa/0);
System.out.println(1/aa);
System.out.println(aa/aa);
System.out.println(1/0);
```

答案是：

```
NaN
Infinity
NaN
抛出异常java.lang.ArithmeticException: / by zero
```

## 分析
从中我们可以看出有以下几种情况

- 一个为零的浮点类型的数据在做除数时，如果被除数是整型，则结果为Infinity,也就是一个无穷数；
- 如果被除数是为零的浮点类型，则无论除数是整型还是浮点类型的零，结果都是NaN。
- 当一个整数除以整数类型的零时，则会抛出异常

float 类型的 0 在除法中不会用准确的0而是非常接近0的小数来表示。

NaN，意思是not an number，即"不是数字"。Java虚拟机在处理浮点数运算时，不会抛出任何运行时异常，当一个操作产生溢出时，将会使用有符号的无穷大(Infinity)来表示，如果某个操作结果没有明确的数学定义的话，将会使用 NaN 值来表示，所有使用 NaN 值作为操作数的算术操作，结果都返回NaN。Java 中的 Double 和 Float 中都有isNaN函数，判断一个数是不是NaN，其实现都是通过 v != v 的方式，因为 NaN 是唯一与自己不相等的值，NaN 与任何值都不相等。

```
Double.NaN != Double.NaN   //true
```

可以用Double.isNaN(double x)的方法来判断结果是不是NaN。

可以用Double.isInfinite(double x)的方法来判断结果是不是Infinity。

因此根据上面的结果，有以下判断
```
		System.out.println(Double.isNaN(aa/0));
		System.out.println(Double.isInfinite(1/aa));
```
结果都显示true。   



