---
title: 手把手教你打造智能小车（3）-小车跑起来
entitle: step-by-step-building-smart-car-3
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws1.sinaimg.cn/large/006tNbRwgy1fvkhlr9btxj30rs0rstbb.jpg
date: 2018-09-23 23:12:54
keywords:
description: 本篇主要讲述小车组装的过程，以及使用 GPIO 通过 L298n 电机驱动板控制小车的行驶方向和车速。
---

## 1. 小车组装
有关树莓派的准备工作已经做好了，下面正式开始小车的制作。
首先把小车车架拼好，买来的车架包括 4 个直流电机、4个轮胎、一个电池盒、各种螺丝以及接线等等。如下图。

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fvkhbljah6j31kw23vh61.jpg)

车架一般是配有有说明书的，组装车架本身也并不复杂，用固定片把各个电机固定好就基本 OK 了，需要注意的是马达朝向、螺丝朝向等小细节，有时候朝向不对会卡住轮子，或者不方便后续的绕线等操作。下图是电机安装图。 
![](https://ws1.sinaimg.cn/large/006tNbRwgy1fvkhcd281jj31kw23vgz3.jpg)

图中的接线，红色代表正极，黑色代表负极。之后连接 L298n 电机驱动板的时候是要这样对接的。所以同一侧的电机，正负极应该是相对的。不过装错了也问题不大，后期跑起来的时候，感觉方向不对可以拆了重装。所以这一次，我们没有必要把所有螺丝都拧上，免得拆起来麻烦。

## 2. L298n 电机驱动

树莓派一般使用 L298n 模块来驱动电机。L298N的实物图如下。

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fvkhlr9btxj30rs0rstbb.jpg)

这个模块要与树莓派和直流电机相连，并且由一个独立的 12V 左右的电源供电。

引脚的接法如下。

1. 电源部分

12v power ： 接 7~12 v 直流电源。接 5v （如树莓派 GPIO 口输出的5v）是不能带动的，而且多树莓派来说也并不安全。所以推荐独立的电源。4 节干电池或者 3 节 18650 锂电池都可以。

Power GND : 接地口。接树莓派的任何一个 GND 口都可以。

5v power:  这个 5v 是输出的！给你的单片机或树莓派供电用的。不推荐使用，因为树莓派和 L298n 最好分开供电。分开供电的话这个脚悬空就行了！

2. 输入部分

A Enable :  接 GPIO 口。电机 A 使能和 PWM 调速。

Logic Input :  接 4 个 GPIO 口。上面两个脚 Input1 、Input2 (靠近 A Enable )控制电机 A；下面两个脚 Input3、Input4 （靠近 B Enable）控制电机 B。

B Enable : 接 GPIO口。电机 B 使能和 PWM 调速。

3. 输出部分：

Output A : 接电机 A 。

Output B :  接电机 B 。


总结一下就是 A Enable 、Input1、Input2 控制电机 A 的运行，B Enable、Input3、Input4 控制电机 B 的运行。

A 电机是指左边的电机，B 电机是指右边的电机。这里我们一边同时接两个电机。

如何控制的呢？ 下面是对电机 A 进行控制的真值表。

<table><tr><td>直流电机</td><td>旋转方式</td><td>IN1</td><td>IN2</td><td>IN3</td><td>IN4</td><td>使能端 A</td><td>使能端 B</td></tr>
<tr><td rowspan="3">M1</td><td>正转</td><td>高</td><td>低</td><td>--</td><td>--</td><td>高</td><td>--</td></tr>
<tr><td>反转</td><td>低</td><td>高</td><td>--</td><td>--</td><td>高</td><td>--</td></tr>
<tr><td>停止</td><td>低</td><td>低</td><td>--</td><td>--</td><td>高</td><td>--</td></tr>
<tr><td rowspan="3">M2</td><td>正转</td><td>--</td><td>--</td><td>高</td><td>低</td><td>--</td><td>高</td></tr>
<tr><td>反转</td><td>--</td><td>--</td><td>低</td><td>高</td><td>--</td><td>高</td></tr>
<tr><td>停止</td><td>--</td><td>--</td><td>低</td><td>低</td><td>--</td><td>高</td></tr></table>

按照真值表的指示，给各个接口施加相应的高低电平，电机就可以工作了。

## 3. 电机转动

树莓派和 L298n 以及 马达接线连接起来之后，我们就应该来试试如何用树莓派通过 Python 来控制这个马达的转动。

树莓派的 33、11、12 脚分别连到 A Enable、IN1 、IN2（把短接帽拿掉）。

由控制表可知给 33 脚高电平，11 脚高电平，12 脚低电平，电机就会正转。

新建 carCtrl.py 文件，写入代码：
```
import RPi.GPIO as GPIO
import time
GPIO.setmode(GPIO.BOARD)

def go():
    GPIO.setup(33,GPIO.OUT)
    GPIO.setup(11,GPIO.OUT)
    GPIO.setup(12,GPIO.OUT)
    GPIO.output(33,True)
    GPIO.output(11,True)
    GPIO.output(12,False)

go()
#延时2秒之后执行cleanup释放GPIO接口
time.sleep(2)
GPIO.cleanup()
```
代码写完之后我们保存退出，接着执行一下观看马达有没有转动。

## 4. 车速控制

刚刚说过，A Enable 是用来控制 A 电机的行驶速度的，那么该如何实现了。

实现方式就是之前说过的 PWM。使用 PWM 控制 A Enable 端的平均电压的高低，较高的电压使车速增加，较低的电压使车速减小。而控制平均输出的电压的高低就是占空比。
在 carCtrl.py 文件中加入如下代码
```
ENA = 33
def changeSpeed():
	leftpwm = GPIO.PWM(ENA, 50)
	leftpwm.stop()
	leftpwm.start(100)
	leftpwm.ChangeDutyCycle(50)
	print('changeSpeed'+50)

changeSpeed()

```
我们发现，电机的行驶速度减慢了。

## 5. 小车跑起来

我们已经让马达转起来了。那么，接下来只要写 t_up(),t_down(),t_left(),t_right()四个函数，就可以控制小车的行驶方向了。
但是，我们在玩的时候，总不能每次都使用命令行来控制小车吧，其实，我们可以通过编写 web 程序使用网络来发送控制指令。

我们使用 Python 中的 Flask 框架来搭建 web 程序。首先需要安装 Flask 框架，运行如下代码
```
sudo apt-get install python3-flask
```
请注意，我这里使用的 Python3，在此之前，你需要运行先安装 Python3，树莓派默认的是 Python2。 

然后就是搭建一个前端界面，并且把控制小车方向的代码写入其中，具体就不说了，直接看我在 [Github](https://github.com/zhmhhu/myPiCar) 上的代码就好了。

运行时，打开命令行，切换到 myPiCar 目录，执行
```
python3 appMyPiCar.py
```
可以看到自动跳出来一个网页，里面就是我们操作小车的界面。




