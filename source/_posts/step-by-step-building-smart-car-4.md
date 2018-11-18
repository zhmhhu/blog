---
title: 手把手教你打造智能小车（4）-使用传感器自动避障
entitle: step-by-step-building-smart-car-4
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws4.sinaimg.cn/large/006tNbRwgy1fvznwtun4uj30m80d53zp.jpg
date: 2018-09-23 23:14:46
keywords:
description: 本文介绍使用超声波传感器探测前面目标距离，以及如何使用红外传感器躲避障碍。
---

## 1. 超声波传感器

### 1.1 简介
超声波传感器是一种广泛使用的距离传感器。本文使用树莓派连接 HC-SR04 超声波测距传感器，用python GPIO 控制传感器完成距离测定，并控制小车在前方障碍小于某一特定值时，做出“停止”、“后退”等一系列动作。

HC-SR04 超声波距离传感器模块可提供 2cm - 400cm 的非接触式距离感测功能，测距精度可达高到3mm。模块包括超声波发射器、接收器与控制电路。如下图。

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fvznwtun4uj30m80d53zp.jpg)

### 1.2 工作原理

传感器的基本工作原理是

(1)给 IO 口 TRIG 至少 10 μs 的高电平信号触发测距；

(2)模块自动发送 8 个 40 khz 的方波，自动检测是否有信号返回；

(3)有信号返回，通过 IO 口 ECHO 输出一个高电平，高电平持续的时间就是超声波从发射到返回的时间。因此，可以得到
```
测试距离=(高电平时间*声速)/2  #声速一般取340 m/s
```
### 1.3 接线方式

HC-SR04 超声波距离传感器模块共有四个引脚，其中有两个电源引脚和两个控制引脚。

1. Vcc 和 Gnd 是电源引脚，Vcc 接树莓派GPIO 口输出的 5v 电源接口，Gnd 接树莓派任意一个 Gnd 接口。理论上说，Vcc 和 Gnd 接任意的 5v DC 电源都行，但最好使用树莓派的 GPIO 口供电，不然会影响这个模块的运行。

2. Trig 引脚用来接收树莓派的控制信号。接任意 GPIO 口。

3. Echo 引脚用来向树莓派返回测距信息。接任意 GPIO 口。

(注意 Echo 返回的是 5v信号，而树莓派的 GPIO 接收超过 3.3v 的信号可能会被烧毁，为保证树莓派 GPIO 口 安全，最好加一个分压电路，我这里没有加，直接用杜邦线连的)。

### 1.4 使用方法
使用 Python 的 GPIO 库操作超声波传感器方法如下。

创建一个 python 文件，命名为 checkDist.py。打开文件，输创建测距函数 checkdist，如下。
```
import RPi.GPIO as GPIO
import time

########超声波传感器接口定义#################
Trig = 38
Echo = 40
# 超声波距离探测
def checkdist(self):
	GPIO.setup(Trig, GPIO.OUT, initial=GPIO.LOW)
	GPIO.setup(Echo, GPIO.IN)
	GPIO.output(Trig, GPIO.HIGH)
	time.sleep(0.00015)
	GPIO.output(Trig, GPIO.LOW)
	while not GPIO.input(Echo):
		pass
	t1 = time.time()
	while GPIO.input(Echo):
		pass
	t2 = time.time()
	return (t2-t1)*340*100/2
```
之后，循环执行这一函数，并把测得的目标物距离显示出来

```
def distStart():
	try:
    while True:
        print '目标距离:%0.2f cm' % checkdist()
        time.sleep(0.5)
	except KeyboardInterrupt:
    	GPIO.cleanup()

distStart()
```

保存代码并执行，可以看到，每隔 0.5秒，系统将显示前方目标物的距离。

## 2. 红外传感器

### 2.1 简介

红外传感器是专为轮式机器人设计的一款距离可调的避障传感器。其具有一对红外线发射与接收管，发射管发射出一定频率的红外线，当检测方向遇到障碍物（反射面）时，红外线反射回来被接收管接收，此时指示灯亮起，经过电路处理后，信号输出接口输出数字信号，通过探测此信号来判断前方有障碍物。
工作电压为3.3V-5V，由于工作电压范围宽泛，在电源电压波动比较大的情况下仍能稳定工作，具有干扰小、便于装配、使用方便等特点，可以广泛应用于机器人避障、 避障小车、流水线计数及黑白线循迹等众多场合。

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fvzrpac320j30dw0dw3ym.jpg)

### 2.2 使用说明

红外传感器的工作电压为DC 3.3V-5V，可直接与树莓派的 5V 电压连接；工作温度 －10℃—＋50℃，可适应大部分工况；在指定距离内探测到障碍物时，电路板上指示灯点亮，同时 OUT 端口持续输出低电平信号，使用非常方便；另外，通过传感器上的电位器旋钮可以调节检测距离，有效距离 2 ～ 40 cm。

接线方式如下：

1. VCC 外接 3.3V-5V 电压(直接与树莓派 5v 电源接口相连)
2. GND 外接 GND(与树莓派任意 GND 接口相连)
3. OUT 输出接口（与树莓派 GPIO 口相连）

### 2.3 使用方法

我们使用一个 LED 灯来来作为有障碍物时发出的警告。Python代码如下：
```
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BOARD)
GPIO.setwarnings(False)
########红外传感器接口定义#################
GPIO_OUT = 16
led = 37 
#设置引脚为输入和输出
#设置16针脚为输入，接到红外避障传感器模块的out引脚
GPIO.setup(GPIO_OUT,GPIO.IN) 
GPIO.setup(led,GPIO.OUT)     
 
def warn():   
	GPIO.output(led,GPIO.HIGH) #亮灯来作为有障碍物时发出的警告
	time.sleep(0.5)
	GPIO.output(led,GPIO.LOW)
	time.sleep(0.5)
		
while True:
	if GPIO.input(GPIO_OUT)==0: #当有障碍物时，传感器输出低电平，所以检测低电平
		warn()
GPIO.cleanup()

```
保存代码并执行，然后将手挡在传感器前，就会看到 LED 灯一闪一闪的，拿开手时，LED 灯将熄灭。转动传感器上的电位器旋钮，可以发现红外传感器发出警告时，目标的距离会发生变化。选择合适的告警距离，大约 15 cm 就可以了。

将超声波测距功能和红外避障功能加入到小车转向控制代码中，一个完整的自动避障小车就完成了。详细代码，见 [Github](https://github.com/zhmhhu/myPiCar)。

系列目录：

[《手把手教你打造智能小车（0）-写在前面的话》](https://zhmhhu.github.io/technology/2018-09-16-step-by-step-building-smart-car-0.html)

[《手把手教你打造智能小车（1）-树莓派及其使用配置》](https://zhmhhu.github.io/technology/2018-09-16-step-by-step-building-smart-car-1.html)

[《手把手教你打造智能小车（2）-点亮 LED 灯》](https://zhmhhu.github.io/technology/2018-09-23-step-by-step-building-smart-car-2.html)

[《手把手教你打造智能小车（3）-小车跑起来》](https://zhmhhu.github.io/technology/2018-09-23-step-by-step-building-smart-car-3.html)

[《手把手教你打造智能小车（4）-使用传感器自动避障》](https://zhmhhu.github.io/technology/2018-09-23-step-by-step-building-smart-car-4.html)

[《手把手教你打造智能小车（5）-使用舵机打造摄像机云台》](https://zhmhhu.github.io/technology/2018-09-24-step-by-step-building-smart-car-5.html)