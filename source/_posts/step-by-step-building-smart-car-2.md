---
title: 手把手教你打造智能小车（2）-点亮 LED 灯
entitle: step-by-step-building-smart-car-2
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://wx3.sinaimg.cn/mw690/0060lm7Tly1fvkg47vmqij30go0iojw5.jpg
date: 2018-09-23 16:11:35
keywords:
description: 这篇文章将讨论如何使用树莓派点亮 LED 灯，以及通过 PWM（脉宽调制）的方法，控制 LED 灯的亮度及闪烁频率。
---

## 1 树莓派引脚及 GPIO

### GPIO 介绍
上文提到了树莓派（如无特别说明，树莓派型号均以 3b 为准）有 40 个引脚，控制这些引脚，需要使用 GPIO。

GPIO（General Purpose I/O Ports）意思为通用输入/输出端口，通俗地说，就是可以通过它们给引脚输出高低电平或者通过它们读入引脚的高低电平状态。GPIO 是一个重要的概念，用户可以通过 GPIO 接口和硬件进行数据交互（如UART），控制硬件工作（如LED、蜂鸣器等），读取硬件的工作状态信号（如中断信号）等。掌握了GPIO，差不多相当于掌握了操作硬件的能力。

现在，我们先来看看树莓派上的 GPIO 是怎么样的。下图是引脚编号对照表。可以看出，引脚有多种不同的编号方式。

![树莓派引脚对照图](https://wx3.sinaimg.cn/mw690/0060lm7Tly1fvkg47vmqij30go0iojw5.jpg)

通过上图可以看到，每一个针脚都有对应的物理引脚 BOARD 编码，BCM 编码和 wiringPi 编码。BOARD 编码按照引脚的物理位置依次进行编号；如果我们要基于 wiringPi 库用 C 语言对树莓派的GPIO进行操作，我们就要选用wiringPi 约定的编号方式；如果我们要基于 BCM 模式用 Python 语言对树莓派的 GPIO 进行操作，我们就要选用 BCM 约定的编号方式。由上图可以看出，wiringPi 中的 GPIO0 对应的是按物理位置编号的 Pin11，GPIO30 对应的是按物理位置编号的 Pin27。

### 1.2 使用 Python 控制 GPIO
和 C 语言相比，Python 语言有着更加强大的功能，而且学习起来更加简单。下面介绍如何使用 Python 语言的 RPi.GPIO 库来控制GPIO。

1. 导入RPi.GPIO模块

可以用下面的代码导入RPi.GPIO模块。
```
import RPi.GPIO as GPIO
```
引入之后，就可以使用GPIO模块的函数了。如果你想检查模块是否引入成功，也可以这样写：
```
try:
    import RPi.GPIO as GPIO
except RuntimeError:
    print("引入错误")
```
2. 确定针脚编号方式

在RPi.GPIO中，使用 BOARD 编号时，引脚编号与电路板上的物理引脚编号相对应。使用这种编号的好处是，你的硬件将是一直可以使用的，不用担心树莓派的版本问题。因此，在电路板升级后，你不需要重写连接器或代码。
如果选择 BCM 规则，是更底层的工作方式，它和 Broadcom 的片上系统中信道编号相对应。在使用一个引脚时，你需要查找信道号和物理引脚编号之间的对应规则。对于不同的树莓派版本，编写的脚本文件也可能是无法通用的。

可以使用下列代码指定一种编号规则，编号规则在使用引脚前必须强制指定：
```
GPIO.setmode(GPIO.BOARD)
  # or
GPIO.setmode(GPIO.BCM)
```
下面代码将返回被设置的编号规则
```
mode = GPIO.getmode()
```
警告:如果 RPi.GRIO 检测到一个引脚已经被设置成了非默认值，那么你将看到一个警告信息。你可以通过下列代码禁用警告：
```
GPIO.setwarnings(False)
```
2. 引脚设置
在使用一个引脚前，你需要设置这些引脚作为输入还是输出。配置一个引脚的代码如下：
```
# 将引脚设置为输入模式
GPIO.setup(channel, GPIO.IN)
 
# 将引脚设置为输出模式
GPIO.setup(channel, GPIO.OUT)
 
# 为输出的引脚设置默认值
GPIO.setup(channel, GPIO.OUT, initial=GPIO.HIGH)

# 释放引脚
GPIO.cleanup()
```
一般来说，程序到达最后都需要释放资源，这个好习惯可以避免偶然损坏树莓派。
注意，GPIO.cleanup() 只会释放掉脚本中使用的 GPIO 引脚，并会清除设置的引脚编号规则。

## 点亮 LED 灯

要想点亮一个LED灯，只需要给相应的引脚输出一个高电平。这个步骤很简单，设置引脚的输出状态就可以了，代码如下：
```
GPIO.output(channel, state)
```
state 可以设置为 0/GPIO.LOW/False 或者 1/ GPIO.HIGH / True。如果编码规则为 GPIO.BOARD，那么 channel 就是对应引脚的数字。

先按照下面这个图连线（LED 中有较大铁片的那一极为负），正极插在 GPIO 25 引脚（物理位置为 22），负极插在 GND 引脚。

![](https://wx3.sinaimg.cn/mw690/0060lm7Tly1fvkgf0zys3j30pa0e8kep.jpg
)

远程连接树莓派之后，先新建一个文件目录 pi_ws，在目录下新建文件 LED.py。

将下面的代码手动输入到 LED.py 里面：
```
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(25, GPIO.OUT)
```
保存文件之后，打开命令行工具，使用 cd 命令切换到 pi_ws 文件目录
```
pi@raspberrypi:~ $ cd /pi_ws
```
运行 LED.py
```
pi@raspberrypi:~/pi_ws $ python LED.py
```
可以看到 LED 灯已经亮了。

在 LED.py 的文件最后加上如下代码
```
while True:
    GPIO.output(25, GPIO.HIGH)
    time.sleep(0.5)
    GPIO.output(25, GPIO.LOW)
    time.sleep(0.5)
```
使用 python 命令运行，可以发现 LED 灯在闪烁。

## PWM 讲解

PWM是一种对模拟信号电平进行数字编码的方法。树莓派不能直接输出模拟电信号，但我们可以使用 PWM（脉宽调制）方法来模拟这一点。我们制作一个固定频率的数字信号，在那里我们将改变脉冲宽度，此时，“平均”输出电压的电平也会随之改变，如下图所示：
![](https://wx1.sinaimg.cn/mw690/0060lm7Tly1fvkgnfmw4qg30fl05u3z7.gif)

我们可以使用这个“平均”电压水平来控制LED亮度，如下图所示：

![](https://wx3.sinaimg.cn/mw690/0060lm7Tly1fvkgpcjzr0g30mu0d6jt0.gif)

请注意频率本身不是重点，而是“占空比”，即“高”脉冲的时间所占的波周期的比例，这个比例以百分制表示，其值为 0 到 100 之间。假设我们在树莓派的 GPIO 上产生一个50Hz的脉冲频率。周期（p）将是频率的倒数即 20ms（1/f）。如果我们想要 LED 达到“半”亮度，就把占空比设为50％，即“高”脉冲在一个周期内的时间是10ms。

使用方法如下。
创建一个 PWM 实例：
```
GPIO.PWM(channel, frequency)
```
启用 PWM：
```
p.start(dc)   # dc 代表占空比（范围：0.0 <= dc >= 100.0）
```
更改频率：
```
p.ChangeFrequency(freq)   # freq 为设置的新频率。单位为 Hz
```
更改占空比：
```
p.ChangeDutyCycle(dc)  # 范围：0.0 <= dc >= 100.0
```
停止 PWM：
```
p.stop()
```
注意，假设实例中的变量“p”超出范围，也会导致 PWM 停止。

下面为使 LED 每两秒钟闪烁一次的演示样例：
```
importRPi.GPIO as GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(25, GPIO.OUT)
  
p =GPIO.PWM(25, 0.5)
p.start(1)
input('点击回车停止：')   # 在 Python 2 中须要使用 raw_input
p.stop()
GPIO.cleanup()
```
这种闪烁的实现方式和上面的通过 `time.sleep()` 函数的实现原理是完全不同的。

下面为使 LED 在亮/暗之间切换的演示样例，注意把 LED 灯接到相应的引脚。
```
import time
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(23, GPIO.OUT)
GPIO.setup(24, GPIO.OUT)

p1 = GPIO.PWM(23, 50)
p2 = GPIO.PWM(24, 38)

p1.start(0)
p2.start(0)

try:
    while 1:
        for dc in range(0, 101, 5):
            p.ChangeDutyCycle(dc)
            p11.ChangeDutyCycle(dc)
            time.sleep(0.1)
        for dc in range(100, -1, -5):
            p.ChangeDutyCycle(dc)
            p11.ChangeDutyCycle(dc)
            time.sleep(0.1)
except KeyboardInterrupt:
    pass

p1.stop()
p2.stop()
GPIO.cleanup()
```
读者可以自己试一下，看看效果。

PWM 的概念非常重要，后期我们在制作小车，控制小车行驶速度的时候将用到。

