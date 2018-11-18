---
title: 手把手教你打造智能小车（1）-树莓派及其使用配置
entitle: step-by-step-building-smart-car-1
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws4.sinaimg.cn/large/006tNbRwly1fwn4qmcbbwj30hs0bzt9j.jpg
date: 2018-09-16 16:39:04
keywords:
description: 打造智能小车的核心部件就是树莓派。树莓派不是蛋黄派，没有馅儿也不能吃，而是一张只有信用卡大小的电路板。它的功能足够强大，具有普通计算机的所有硬件配置。
---

## 1 简介

打造智能小车的核心部件就是树莓派。树莓派不是蛋黄派，没有馅儿也不能吃，而是一张只有信用卡大小的电路板，其英文名是 Raspberry PI。树莓派的功能足够强大，配置也都很齐全，具有普通计算机的所有硬件配置，比如CPU、内存、显卡、声卡、wifi、蓝牙、USB接口和网线接口。可以说，树莓派就是一台微型计算机。不仅如此，它还带有 40 个引脚，通过引脚可以接入设备，比如 LED 灯、传感器、驱动板等等，从而探测信号以及发出控制指令。
![](https://ws4.sinaimg.cn/large/006tNbRwly1fwn4qmcbbwj30hs0bzt9j.jpg)

我这里使用的树莓派型号是 Raspberry PI 3 Model B（以下简称 3B），于2016年2月发布，目前售价人民币 200 多元。现在已经有了功能更加强大的 3B+，与 3B 相比，升级了内存和网卡等等，售价也更高。点击[这里](http://shumeipai.nxez.com/raspberry-pi-version-compare)可以查看树莓派各版本对照表。与普通计算机相比，树莓派的价格简直是便宜得不要不要的。

## 2 制作树莓派的 SD 卡

树莓派本身不带有硬盘，SD卡就是它的硬盘，因此所有的操作系统文件都要写到 SD 卡里面。也就是说，如果树莓派里的软件坏了启动不了，换一张装有全新操作系统的 SD 卡就可以了。所以树莓派本身一般不会有软件故障，故障都出在 SD 卡上。

在使用树莓派之前，要先把操作系统刷进去。刷机的操作也很简单，到官方网站上下载安装镜像，用刷机软件烧录进 SD 卡就可以了。不管你用的是 windows、MAC 或 Linux 系统的个人电脑，都可以借助刷机软件轻松制作 树莓派 SD 卡。点击[这里](http://shumeipai.nxez.com/download#tools)查看详细教程。

MAC 系统甚至可以不使用刷机工具，直接使用命令行就可以了。下面以 MAC Book Pro 为例讲解烧录树莓派 SD 卡的操作过程。

1. 插入 SD 卡

首先要将 SD 卡插入到电脑上（你可能需要一张具有 USB 接口的读卡器）。打开命令行，通过命令 `df -lh` 
查看当前已挂载的卷，判断 SD 卡是否被读取。
```
xxdeMacBook-Pro:Downloads administrator$ df -lh
Filesystem     Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk1    233Gi   43Gi  189Gi    19%  813124 4294154155    0%   /
/dev/disk2s1   15Gi  2.4Mi   15Gi     1%       0          0  100%   /Volumes/SD
```
我们可以通过属性，如 Size Used Avail 等，可以判断出 
当前 disk2s1 就是 SD 卡在系统里对应的分区。如果你的sd卡有多个分区，那么可能还会有disk2s2,disk2s3…

2. 卸载 SD 卡

通过命令 `diskutil unmount /dev/disk2s1` 卸载 SD 卡
```
xxdeMacBook-Pro:Downloads administrator$ diskutil unmount /dev/disk2s1
Volume SD on disk2s1 unmounted
```
3. 确认设备号

通过命令 `diskutil list` 来确定设备
```
iluhaodeMacBook-Pro:Downloads administrator$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage Macintosh HD            250.1 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           +249.8 GB   disk1
                                 Logical Volume on disk0s2
                                 E8CADD9F-4CA2-4156-9CEE-D3FCE187322D
                                 Unencrypted

/dev/disk2 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *15.9 GB    disk2
   1:             Windows_FAT_32 SD                      15.9 GB    disk2s1
```
根据显示信息 SIZE 可以判断 /dev/disk2 是 SD 卡，这个要根据自己的 SD 卡的实际情况判断。

4. 烧写系统

通过 cd 命令进入镜像文件所在目录，然后通过命令
```
sudo dd bs=4m if=xxx of=yyy
```
进行系统的烧写。if=“xxxx” of=“yyyy”中 “xxxx”代表镜像的名称，“yyyy”代表我们要烧写的 SD 卡的设备号。例如：
```
xxdeMacBook-Pro:Downloads administrator$ sudo dd bs=4m if=rpi_35_v6_1_2_3_jessie_kernel_4_4_50.img of=/dev/disk2
Password:
1062+1 records in
1062+1 records out
4454400000 bytes transferred in 339.766726 secs (13110171 bytes/sec)
```
提示 Password 时，需要输入你的计算机密码，输入过程中，可能不会显示任何内容，输完之后，按下 Enter 键。
过几分钟（这个过程可能会比较长，耐心等待），出现“records in, records out”之类的信息，表明系统刷成功了。

5. 卸载 SD 卡

通过命令 `diskutil unmountDisk` 卸载 SD 卡。
```
xxdeMacBook-Pro:Downloads administrator$ diskutil unmountDisk /dev/disk2
Unmount of all volumes on disk2 was successful
```
这样，树莓派的 SD 卡就制作好了，此时取下 SD 卡。

## 3 启动配置

把装有操作系统的 SD 卡 插入树莓派卡槽，通电，系统会自动开机。

在使用之前，要对其进行初始配置。为了看到开机画面，可以使用 HDMI 线连接树莓派和具有 HDMI 接口的显示器，比如电脑显示器，或者电视机。

只初始配置需要连接显示器，之后的使用过程，我们可以用其他电脑远程登录树莓派，不再需要接显示器了。

树莓派开机后会启动配置向导，安装提示一步步傻瓜式的操作下去就可以了。设置国家和地区 ->设置用户名和密码（以后远程登录也用这个用户名密码）->选择 wifi 网络 -> 检查更新 -> 重启

下图是树莓派启动后的默认桌面
![](https://ws2.sinaimg.cn/large/006tNc79gy1fvbic26yckj31kw0vwhdv.jpg)

输入用户名和密码的时候，你需要鼠标和键盘，把 USB 接口直接插上去就可以使用了，就跟你用其他的电脑一样。

启动配置的图文并茂教程请查看[这里](http://shumeipai.nxez.com/2018/07/09/raspbian-2018-06-17-new-features-and-configuration.html)，做到重启一步就可以了，后面的操作不一定跟它一样。

树莓派的默认用户名是pi，默认密码是 raspberry，可以使用命令行修改密码。
```
sudo passwd pi 
```
系统会提示用户输入两遍新密码，之后就修改成功了。树莓派有一个 root 账号，但默认锁定，需由用户手动启用。启用 root 账号之前，需先为其设定密码，方法与为 pi 账号修改密码相同。
```
sudo passwd root
```
用户输入两遍的root密码，在输入以下命令对 root 账号解锁，并跳转到 root 账号。
```
sudo passwd --unlock root   #解锁root账号
su root                     #跳转到root账号
```
root 账号的权限较高，请不要在此账号下随意修改系统文件。

## 4 远程连接

每次使用树莓派都要接显示器和键盘鼠标是不是很麻烦？其实有更好的方式就是远程连接。通过远程连接，可以实现与直接操作一样的效果，何乐而不为？

实现远程连接需要安装 VNCserver，幸运的是，最新的操作系统默认自带 VNCserver。如果你不幸刷的系统是没有 VNCserver 的也没有关系，装一个就是了。

打开树莓派的命令行界面，输入以下命令
```
sudo apt-get update
sudo apt-get install realvnc-vnc-server realvnc-vnc-viewer
```

稍等片刻，就装好 VNCserver，可能要重启才会生效。

之后，只要开启远程连接功能就可以了。下面是开启远程连接功能的方法。

1. 打开命令行,输入 `sudo  raspi-config` ，将显示系统配置界面，如下图。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fvbixeu9bij30gr077dg3.jpg)

2. 选择 `5 Interfacing Options`，进入如下画面

![](https://ws2.sinaimg.cn/large/006tNc79gy1fvbj1fq3ozj30zm0ee0wu.jpg)

3. 选择 `P3 VNC`，之后选择 `True`。树莓派远程配置就完成了。

下面要在你的电脑上安装 VNCserver，这样才能成功连接树莓派。

安装方式也很简单，点击 [链接](
https://www.realvnc.com/en/connect/download/viewer/)，根据自己的操作系统选择对应的版本下载安装就好啦。比如，我的是MAC book，就选择“MACos”，下载的安装包名为“VNC-Viewer-6.18.907-MacOSX-x86_64.dmg”

安装完之后，打开，并输入树莓派的内网 IP 地址，之后输入用户名和密码就可以成功连接了。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fvbjam6bksj30dr09j746.jpg)

除了 VNCserver 之外，还有 tightvncserver 也可以进行远程连接，但个人认为不好用，因为我搞了半天，都没有配置好剪贴板共享。最后还是发现系统自带 VNCserver 很好用，推荐用这个。

使用 VNC 连接到树莓派之后，可能会出现屏幕分辨率过大或过小的情况，这个时候，需要调整下分辨率。

命令行输入 `sudo  raspi-config`。

弹出树莓派原装系统的配置界面：

![](https://ws4.sinaimg.cn/large/006tNc79gy1fvbixeu9bij30gr077dg3.jpg)

![](https://ws1.sinaimg.cn/large/006tNbRwly1fwy5woekmyj30fv03y3yd.jpg)

![](https://ws2.sinaimg.cn/large/006tNbRwly1fwy5wrndplj30ch05mt8j.jpg)

Resolution就是“分辨率”的意思。

如果你连接的是屏幕超大的电视机，可以选择最高的1920x1080分辨率。

 VNC 中，shell 使用的是 xshell， 传送文件到树莓派用的是 winscp。所有的操作都是在本地局域网进行，基本上没有延时。

 系列目录：

[《手把手教你打造智能小车（0）-写在前面的话》](https://zhmhhu.github.io/technology/2018-09-16-step-by-step-building-smart-car-0.html)

[《手把手教你打造智能小车（1）-树莓派及其使用配置》](https://zhmhhu.github.io/technology/2018-09-16-step-by-step-building-smart-car-1.html)

[《手把手教你打造智能小车（2）-点亮 LED 灯》](https://zhmhhu.github.io/technology/2018-09-23-step-by-step-building-smart-car-2.html)

[《手把手教你打造智能小车（3）-小车跑起来》](https://zhmhhu.github.io/technology/2018-09-23-step-by-step-building-smart-car-3.html)

[《手把手教你打造智能小车（4）-使用传感器自动避障》](https://zhmhhu.github.io/technology/2018-09-23-step-by-step-building-smart-car-4.html)

[《手把手教你打造智能小车（5）-使用舵机打造摄像机云台》](https://zhmhhu.github.io/technology/2018-09-24-step-by-step-building-smart-car-5.html)

