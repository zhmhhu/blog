---
title: 如何在 linux系统下使多版本 Python 共存
entitle: install-different-python-on-linux
categories:
  - coding
tags:
  - python
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws4.sinaimg.cn/large/006tNc79ly1g4cmxewbb5j30go08wdfz.jpg
date: 2019-06-23 22:24:27
keywords:
description:
---

## 系统自带 python
linux系操作系统，包括运行在 PC机上的ubuntu,运行在树莓派上的 Raspbian 等等，通常自带 python,而且不允许卸载。原因在于，Linux 的内部命令是依赖于 python 环境的。如果不小心把系统中的 python 路径 usr/bin/python 移除了，导致的后果就是，系统内部命令（例如 apt-get）无法使用。而打开终端后，终端里面直接显示缺少文件，所有的 .py 命令无法执行。

然而 Linux 下自带的 python，因为版本略低，大部分人都不会直接利用。这就需要我们重新安装高版本 python，并且不影响原 系统的使用。

以下所有操作，使用树莓派的 Raspbian 系统为例。

## 查看 python版本及安装路径

安装 python 之前，我们先看看系统自带了那些版本。
打开命令行，分别输入命令 which python 和 which python3（通常 python 命令指向 python 2.X，python 指向 python 3.X）。
Which命令主要是用来查找系统**PATH**目录下的可执行文件。  此处可查看执行 python 命令的可执行文件存放的路径。
```
pi@raspberrypi:~ $ which python
/usr/bin/python
pi@raspberrypi:~ $ which python3
/usr/bin/python3
```
下面来看一下 python 具体的安装路径
```
pi@raspberrypi:~ $ python
Python 2.7.13 (default, Sep 26 2018, 18:42:22) 
[GCC 6.3.0 20170516] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print(sys.path)
['', '/usr/lib/python2.7', '/usr/lib/python2.7/plat-arm-linux-gnueabihf', '/usr/lib/python2.7/lib-tk', '/usr/lib/python2.7/lib-old', '/usr/lib/python2.7/lib-dynload', '/usr/local/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages']
```

```
pi@raspberrypi:~ $ python3
Python 3.5.3 (default, Sep 27 2018, 17:25:39) 
[GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print(sys.path)
['', '/usr/lib/python35.zip', '/usr/lib/python3.5', '/usr/lib/python3.5/plat-arm-linux-gnueabihf', '/usr/lib/python3.5/lib-dynload', '/usr/local/lib/python3.5/dist-packages', '/usr/lib/python3/dist-packages']
```

## 重新安装 python

重新安装原版本 python，并将其覆盖。在线安装时，系统会提示已经安装了 python,无法继续。因此只能离线安装。

下载离线安装包，解压缩。为避免覆盖原路径‘/usr/lib/ ’下的python，我们把安装包解压到路径‘/usr/local/’
```
$ wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz 
sudo tar -zxvf Python-2.7.14.tgz  -C /usr/local/
```
之后是编译和安装。由于编译的过程比较长，我们可以将编译安装的几条命令合起来，等一段时间会提示安装成功.
```
$  cd /usr/local/Python-2.7.14
$ sudo ./configure && sudo make && sudo make install
```
安装3.X版本的 python,操作类似
```
$ wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz
$ sudo tar zxvf Python-3.6.1.tgz  -C /usr/local/
$ cd /usr/local/Python-3.6.1
$ sudo ./configure && sudo make && sudo make install
```
## 建立软连接
安装之后，应当将新的 python 与原 python 的可执行文件建立连接，这样我们才可以在命令行中使用新的 python
```
sudo rm -rf /usr/bin/python
sudo ln -s /usr/local/Python-2.7.14/python /usr/bin/python
```
``` 
sudo rm -rf /usr/bin/python3
sudo ln -s /usr/local/Python-3.6.1/python /usr/bin/python3
```
安装成功后我们可以看一下python的版本
```
pi@raspberrypi:~ $ python --version
Python 2.7.14
```
如果想把将Python 3.6.1软链接到python也是可以的。
sudo rm -rf /usr/bin/python3
sudo ln -s /usr/local/Python-3.6.1/python /usr/bin/python

这样，在命令行中输入`python`时，就直接指向Python3.6.1了

未完待续

 
