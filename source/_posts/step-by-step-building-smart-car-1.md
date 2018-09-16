---
title: 手把手教你打造智能小车（1）-树莓派及其他配件简介
entitle: step-by-step-building-smart-car-1
categories:
  - coding
tags:
  - javascript
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws2.sinaimg.cn/large/006tNc79gy1fvbic26yckj31kw0vwhdv.jpg
date: 2018-09-16 16:39:04
keywords:
description: 打造智能小车的核心部件就是树莓派。树莓派是一个只有信用卡大小的微型电脑，它的功能足够强大，具有普通计算机的所有硬件配置。
---

## 树莓派及其使用配置

### 简介

打造智能小车的核心部件就是树莓派。树莓派是一个只有信用卡大小的微型电脑，它的功能足够强大，具有普通计算机的所有硬件配置，比如CPU、内存、显卡、声卡、wifi、蓝牙、USB接口和网线接口。可以说，树莓派就是一部浓缩版的计算机。不仅如此，它还带有 40 个引脚，通过引脚可以控制其他设备，进而增加功能。

我这里使用的树莓派型号是 Raspberry PI 3 Model B（以下简称 3B），现在已经有了功能更加强大的 3B+，与 3B 相比，升级了内存和网卡等等，售价也更高。点击[这里](http://shumeipai.nxez.com/raspberry-pi-version-compare)可以查看树莓派各版本对照表。3B 在某宝的售价是 200 左右。与普通计算机相比，简直是便宜得不要不要的。

### 制作树莓派的SD卡

树莓派本身不带有硬盘，SD卡就是它的硬盘。所有的操作系统文件都要写到 SD 卡里面。换句话说，如果树莓派里的软件坏了启动不了，换一张装有全新操作系统的 SD 卡就可以了。所以树莓派本身不会有软件故障，故障都出在 SD 卡上。
在使用树莓派之前，要先把操作系统刷进去。刷机的操作也很简单，到官方网站上下载安装镜像，用刷机软件烧录进 SD 卡就可以了。点击[这里](http://shumeipai.nxez.com/download#tools)查看详细教程。

### 启动配置

把装有操作系统的 SD 卡 插入树莓派卡槽，通电，系统会自动开机。在使用之前，要对其进行初始配置。为了看到开机画面，可以使用 HDMI 线连接树莓派和 具有 HDMI 接口的显示器，或者电视机也行。

只初始配置需要连接显示器，之后的使用过程，我们可以用其他电脑远程登录树莓派，不再需要接显示器了。

树莓派开机后会启动配置向导，安装提示一步步傻瓜式的操作下去就可以了。设置国家和地区 ->设置用户名和密码（以后远程登录也用这个用户名密码）->选择 wifi 网络 -> 检查更新 -> 重启

下图是树莓派启动后的默认桌面
![](https://ws2.sinaimg.cn/large/006tNc79gy1fvbic26yckj31kw0vwhdv.jpg)

对了，输入用户名和密码的时候，你需要鼠标和键盘，把 USB 接口直接插上去就可以使用了，就跟你用其他的电脑一样。

启动配置的图文并茂教程请查看[这里](http://shumeipai.nxez.com/2018/07/09/raspbian-2018-06-17-new-features-and-configuration.html)，做到重启一步就可以了，后面的操作不一定跟它一样。

### 远程连接

每次使用树莓派都要接显示器和键盘鼠标是不是很麻烦？其实有更好的方式就是远程连接。通过远程连接，可以实现与直接操作一样的效果，何乐而不为？

实现远程连接需要安装 VNCserver，幸运的是，最新的操作系统默认自带VNCserver。如果你不幸刷的安装包是没有 VNCserver 的也没有关系，装一个就是了。

打开树莓派的命令行界面，输入以下命令
```
sudo apt-get update
sudo apt-get install realvnc-vnc-server realvnc-vnc-viewer
```

稍等片刻，就装好VNCserver，可能要重启才会生效。

之后，只要开启远程连接功能就可以了。

打开命令行,输入 `sudo  raspi-config` ，将显示系统配置界面，如下图。
![](https://ws4.sinaimg.cn/large/006tNc79gy1fvbixeu9bij30gr077dg3.jpg)

选择 `5 Interfacing Options`，进入如下画面
![](https://ws2.sinaimg.cn/large/006tNc79gy1fvbj1fq3ozj30zm0ee0wu.jpg)

选择 `P3 VNC`，之后选择 `True`。树莓派远程配置就完成了。

下面要在你的电脑上安装 VNCserver，这样才能成功控制树莓派。

安装方式也很简单，点击 [链接](
https://www.realvnc.com/en/connect/download/viewer/)，根据自己的操作系统选择对应的版本下载安装就好啦。比如，我的是MAC book，就选择“MACos”，下载的安装包名为“VNC-Viewer-6.18.907-MacOSX-x86_64.dmg”

安装完之后，打开，并输入树莓派的内网 IP 地址，之后输入用户名和密码就可以成功连接了。
![](https://ws4.sinaimg.cn/large/006tNc79gy1fvbjam6bksj30dr09j746.jpg)

除了 VNCserver 之外，还有 tightvncserver 也可以进行远程连接，但个人认为不好用，因为我搞了半天，都没有配置好剪贴板共享。最后还是发现系统自带 VNCserver 很好用，推荐用这个。


## 其他配件

下面这些配件就比较简单了，买过来就行，不需要配置。某宝上都有，就不给链接了，免得有做广告的嫌疑。

LED灯，数量不限，最好5个以上吧，怕烧坏的话就多备几个，反正不贵。

L298n 电机驱动板，1个。

驱动电机，4个。

车架及轮子等，1套。这个五花八门的太多，如果要做成跟我一样的，就到[这里](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.19052e8d0gZcV5&id=40760596290&_u=glfvo14793c)买，如果你想买质量更好的，就到网上找。

杜邦线，公对母，母对母，公对公各 20 左右，多备点更好。长度以 20cm 为宜。

![](https://ws1.sinaimg.cn/large/006tNc79gy1fvbkp3jawbj30sg0lcn9k.jpg)

有这些就可以打造可以进行方向控制的小车了，至于做功能升级要用到的传感器，在后面的教程里会具体再说。

