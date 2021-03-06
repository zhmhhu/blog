---
title: 锂离子电池中的电量是如何表示的
entitle: 'the-amount-of-electricity-in-lithium-ion-battery'
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws3.sinaimg.cn/large/006tNc79gy1ftaldjcwhlj30b8091t9y.jpg
date: 2016-11-26 22:57:47
keywords:
description: 拿到一块锂离子电池或移动电源之后，我们首先能看到的是信息是容量是多少 mAh，输入输出电压是多少。比如下面这块移动电源的参数，电池容量10000mAh，输入5V,1A，输出5V,1A等等。
---

拿到一块锂离子电池或移动电源之后，我们首先能看到的是信息是容量是多少 mAh，输入输出电压是多少。比如下面这块移动电源的参数，电池容量 10000 mAh，输入5V-1A，输出5V-1A等等。

![](https://ws3.sinaimg.cn/large/006tNc79gy1ftaldjcwhlj30b8091t9y.jpg)

mAh 是代表什么含义呢？我怎么知道这块电池有多少能量，以及把它充满需要多长时间呢？其实，这些问题，只要明白了其中的原理，都能够从这些标明的数字中计算出来。

## mAh表示电池所带电荷量

mAh 称作毫安时，在物理学中，它是衡量物体携带电荷量多少的物理量。电荷量以 Q 表示。它的标准单位是库伦，以 C 表示。

毫安时如何能评价物理带电量呢？我们知道电流表示单位时间内通过导体横截面的电量大小，即

    I=Q/t。 可见Q=I*t，

也就是说电量Q等于电流强度I与用电时间t的乘积，这时I的单位是“安培A”，时间的单位是“秒s",电量的单位是“库伦C”。

    1A*1s=1C，1mAh=3.6C，1Ah=1000mAh=3600C。

如果把电比作水，那么mAh只能表示这个池子里面有多少水，并不能表示这一池子的水有多少能量。假设1mA对应一个流量，那么这池水在1万个流量的状态下可以持续1个小时。

毫安时可以表示电池带电量的多少，但是并不能表示能量的多少。要表示能量，还缺一个物理量，那就是电压。

我们知道，电功率等于导体两端电压与通过导体电流的乘积，即P=U·I。同时，电器的耗电量可以通过功率乘以时间来计算，即W=P·t。那么，电器的耗电量用电流电压来表示就是

    W=P·t=U·I·t

对于蓄电池来说，它所蕴含的能量 W=U·I·t=U·Q，也就是说，把电池的放电电压与电量相乘，就是这块电池的能量。以一块普通的移动电源为例，容量10000mAh，放电电压5V，全部释放出来的能量是

    W=U·Q=5V×10000mAh=5V×10A×3600s=180000J

也就是说，这块电池完全放电能放出180000 焦耳的电能。

同样以水打比方，一池水所蕴含的能力，除了与水的多少有关，还与它说在的高度有关，高度越高，能量越大。这里的高度，相当于电压。

## 能量的另一个单位Wh

在乘坐飞机的时候，我们会被告知
>携带的移动电源额定能量不超过100Wh，如果额定能量超过100Wh但不超过160Wh的，便要经航空公司批准后才能携带。额定能量超过160Wh的移动电源，被明确严禁携带.

这里的Wh是什么单位呢，它又如何能表示能量呢？

Wh读作瓦时，它并不是一个标准单位，而是两个物理量的乘积，瓦是功率，时是时间单位，表示一小时。

    1Wh=1W×3600s=3600J。

然而，知道电池有多少焦耳的能量并没有什么卵用。我们想知道的是，如何把标定移动电池电量的mAh转换成机场规定的Wh，以便于判断我的移动电源是否超标了。

还以刚刚那块移动电源为例，由于1W=1V*1A，如果时间都取一小时，那么1Wh=1V*1A*1h。如果额定电压是5V，那么10000mAh容量的电池所具有的能量就是50Wh。100Wh的移动电源如果放电压是5V，它的容量就可以用100Wh除以5V等一20Ah，也就是20000mAh。

## 电池的充电时间该如何计算

电池的充电时间与充电器的功率有关。拿到一个充电器之后，我们可以它的充电指标比如

    输出电压5V，电流1A

计算一下功率是5W。理论上，50Wh（即容量10000mAh）的电池需要充电10小时。实际工作是可能会有损耗，比如电池的发热以及充电器本身的发热，时间要稍长一些。

手机电池3000mAh（如果放电电压3.7V，能量是11.1Wh），使用上述充电器充电需要大约11.1Wh/5W，约为2.22小时。

## 总结
mAh是电量的单位，其标准单位是库伦（C）,1mAh=3.6C。

Wh是能量单位，其标准单位是焦耳（J）,1Wh=3600J。

mAh和Wh之间的换算需要指定电压（U），1Wh=1000mAh*1V。

如果要计算移动电源有多少瓦时，就用它的电池容量（mAh）除以一千再乘以电压(V)。

计算充电需多长时间，就用它的额定能量（Wh）除以充电器的功率（输出电压V乘以电流A），得出充电需要多少小时，当然，计算出来的值是理论值。