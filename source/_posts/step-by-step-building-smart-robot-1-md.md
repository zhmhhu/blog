---
title: 手把手教你打造智能语音机器人（1）-强大的 DuerOS 系统
entitle: step-by-step-building-smart-robot-1.md
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws1.sinaimg.cn/large/006tNc79ly1fz58q10cf5j30a7097q3i.jpg
date: 2018-12-26 22:05:18
keywords:
description:
---

## DuerOS 是什么

DuerOS是百度研发的一款对话式人工智能操作系统，是百度全球领先人工智能技术的重要应用之一，借助百度的信息与服务构成的巨大生态，DuerOS拥有海量数据，能通过自然语言完成对硬件的操作与对话交流，为用户提供完整的服务链条。作为一款开放式的操作系统， DuerOS通过云端大脑时刻进行自动学习让机器具备人类的语言能力。

DuerOS广泛支持手机、电视、音箱、汽车、机器人等多种硬件设备，搭载DuerOS系统能力的硬件设备，可以轻松打造对话式人工智能系统，我们打造智能语音机器人的核心硬件设备————树莓派————也是 DuerOS 支持的硬件设备之一。

和 DuerOS 类似的还有 Amazon Alexa 和 Google Assistant，这两者由于是美国人研发的，对中文语音的识别并不是很好，特别是 Google Assistant 用到的服务在中国境内被屏蔽了，想要使用还得翻墙，非常麻烦。所以这里我们首选百度的 DuerOS 。


## 硬件准备

一个完整的智能语音系统主要由如下几个核心部分组成：

麦克风阵列

唤醒识别

语音识别（ASR）

自然语言处理（NLP/NLU）

内容召回

下面一个个分别介绍。

### 麦克风阵列

麦克风阵列是智能语音系统里面唯一的语音输入设备。我们在日常生活中使用的单麦克风，也可以使用，但是单麦克风对于远场拾音效果很不好。比如，手机上一般使用单麦克风，打电话时手机需要放在半米的范围之内，对方才能听清说话的声音。如果想要在 3~5 米的范围之内，也能够捕获声音，就需要麦克风阵列。

麦克风阵列在硬件上可以简单的理解成，一个麦克风阵列由多个麦克风组成。麦克风阵列的远场拾音效果很好，另外，麦克风阵列能够获取声源的角度信息，也就是说能够辨别声音的来源方位。有了声音的来源方位信息，我们就可以在开发语音交互系统时，扩展新的功能。

这里推荐两类比较常用的麦克风：

1. PlayStation Eye

Playstation Eye 是 PS3 主机上搭配 PS Move 玩体感游戏的，自带麦克风和摄像头，只占用一个 USB 接口。花一份钱，买两样东西，而且价格竟然只有惊人的 26 块钱，还包邮。至于使用效果呢，总体来说还是可以的。麦克风在没那么嘈杂的环境下，三米以外也能获得不错的拾音效果。摄像头分辨率确实不高，只有640×480，但是帧数比较高，一般使用也足够了。目前，我们还用不到摄像头的功能，所以这个设备足以满足要求了。 

这里附上[链接](https://s.taobao.com/search?q=Playstation+Eye&imgfile=&commend=all&ssid=s5-e&search_type=item&sourceId=tb.index&spm=a21bo.2017.201856-taobao-item.1&ie=utf8&initiative_id=tbindexz_20170306)，找销量最高的那个就别买贵的。下面的配置也会按照这个硬件做示例。

2. ReSpeaker 2 Mics Pi HAT

ReSpeaker 2 Mics Pi HAT 是专门为树莓派打造的 2 阵列开发板，带 2 Mic 阵列，有声卡，支持外接 3.5mm 音频输出。与 PlayStation Eye 不同的是，它要占用树莓派的GPIO，并且需要自己安装驱动（过程略显复杂，后面会讲）。
这里给出[链接](https://s.taobao.com/search?q=respeaker+2-mics+pi+hat&imgfile=&js=1&stats_click=search_radio_all%3A1&initiative_id=staobaoz_20170829&ie=utf8)。找个便宜的买，其实质量都差不多。

两种麦克风建议都配备。在我的开发过程中，先是使用了 PlayStation Eye，因为配置简单，即插即用；后来使用了 ReSpeaker 2 Mics Pi HAT 开发板，因为要做真正的机器人，不可能在用麦克风时还拖着一根线吧，这个开发板是直接插在树莓派 GPIO 口上的，小巧方便。ReSpeaker 还有 4个麦的 ReSpeaker 4-Mic 和 6 个麦的 ReSpeaker 6-Mic，价格依次递增。这两者我没有试过，如果你不缺钱，可以买来试试，效果应该比 2 个麦的要好。

### 唤醒识别

语音唤醒的常见场景就是用户使用唤醒词（如百度的“小度小度”，亚马逊的“Alex”）将设备激活。不将设备激活，它是没有办法接收用户的指令的。

实际上设备在通过唤醒词激活之前也是一直在工作的，设备一直在录音，并检查录音的数据中是否包含预设的唤醒词（如“小度小度”、“Alexa”），当检测到有唤醒词，设备便立刻进入唤醒状态。

当前对于个人开发者相对友好的免费的唤醒引擎主要有：

SnowBoy (https://snowboy.kitt.ai/)

Pocketsphinx (https://cmusphinx.github.io/wiki/tutorialpocketsphinx/)

Sensory (http://www.sensory.com)

目前，百度已全资收购了KITTAI（SnowBoy是KITTAI旗下产品），建议开发者直接使用SnowBoy作为唤醒引擎，同时，SnowBoy的唤醒词训练，及唤醒引擎的集成使用也很简洁方便。SnowBoy 支持 更换唤醒词，就比如我这台智能机器人的唤醒词“小白小白”，就是我自己配置的。根据教程，你可以更换自己喜欢的唤醒词。

### 语音识别

语音识别（ASR）简单的说就是讲语音转化为文本，目前几乎所有的语音系统都是先将语音转化为文本，然后再基于文本进行后续的语义理解和处理的。

在 DuerOS 中，系统将采集到的语音信息不断地发送到百度的云服务后台，由后台对语音进行解析和识别，再进行后续的处理。

### 自然语言处理

有了语言识别（ASR）获取的文本信息，后面就进入了自然语言处理单元了，可以说这个步骤是最接近我们概念上理解的人工智能了。这个部分会从输入文本中获取用户的意图和对应的关键信息。举个例子，对应用户输入请求：“我想听周杰伦的歌”，NLU会将请求拆解成如下的结构化结果：

意图：听歌

词槽：周杰伦

有了 NLU 的处理结果，就可以获取用户请求的结果了。

用户可设置自定义控制指令，系统就会根据用户的设定，识别控制指令并下发相对应的指令编码，便于用户后续处理。比如，用户输入请求“让机器人向左转弯”。系统能够识别“左”和“转”两个关键信息，下发指令编码，接着用户在编写代码识别指令编码，使用 GPIO 控制电机运转。

### 内容召回

这部分是对话式语音系统的重要内容，背后是支撑对话的强大数据库和信息源。

假设你有两个资源库，其中，一个是电影库，一个是歌曲库。当接收 NLU 的处理结果后，从意图（听歌）上，你可以判别用户希望从歌曲库中获取资源，从词槽（周杰伦）可以判断用户想听歌曲的类别。有了意图和词槽就能从资源库中检索到用户期望的结果，并将结果按请求的路径返回。


## 在树莓派上使用 DuerOS

DuerOS 虽好，但我们应该如何使用呢。幸运的是，百度已经给出了在树莓派上使用 DuerOS 强大能力的示例。点击[此处](https://developer.baidu.com/forum/topic/show/244796?pageNo=1)查看。通过这个示例，我们就能够在树莓派上部署一套 SDK，结合 麦克风阵列和树莓派开发板等硬件，打造一个如上一节所说的完整的智能语音系统。下面介绍下在树莓派上如何使用 DuerOS。

### 刷机
如教程所说，由于树莓派官方镜像的 OpenSSL 并不支持 Http2 ALPN，因此我们需要刷 DuerOS 的树莓派镜像安装包，并在之后再次安装 OpenSSL，才能够正常使用 DuerOS。点击[此处](https://dueros.bj.bcebos.com/DevkitPersonalImg%2FDuerOS_For_Raspberry_v0.8.1_20171221.img.gz?authorization=bce-auth-v1%2Ff04711cb669b4f6f9c57cbe33dbcd609%2F2017-12-21T08%3A58%3A38Z%2F-1%2Fhost%2Fd263e7ba66ef6213d75cb02f5e400aa6ac7e029d5409bd16f31d353b37df2d82)下载 DuderOS 专用的树莓派镜像。这一镜像也可以在[“DuerOS智能硬件开发套件产品介绍”](http://open.duer.baidu.com/doc/device-devkit/intro_markdown)中找到。之后，按照我们在《手把手教你打造智能小车（1）-树莓派及其使用配置》学过的方法刷机、配置和启动树莓派。

### 开始

首先连接好麦克风和音箱（麦克风使用PlayStation Eye，连接 USB 接口，音箱接树莓派3.5mm耳机孔，没有音箱就用耳机代替），进行以下操作。

停止现有小度功能，因为会占用MIC资源
```
 sudo systemctl disable duer
 sudo systemctl stop duer
```
安装依赖包
```
 sudo apt-get update
 sudo apt-get install python-dateutil
 sudo apt-get install gir1.2-gstreamer-1.0
 sudo apt-get install python-pyaudio
 sudo apt-get install libatlas-base-dev
 sudo apt-get install python-dev     
 sudo pip install tornado
 sudo pip install hyper
```
hyper库用来支持http2.0 client, pyaudio用来支持录音，tornado用来完成oauth认证。

下载编译好的openssl和Python安装包，并进行安装, 需要更新openssl才能支持python sdk的使用。

*从如下地址下载openssl安装包*(链接：https://eyun.baidu.com/s/3jJmdsbW 密码：EyA4)
*从如下地址下载python2.7.14安装包*(链接：https://eyun.baidu.com/s/3c3SSVao 密码：0sDM)

```
 sudo tar -zxvf openssl1.1.tar.gz -C /usr
 sudo tar -zxvf python2.7.14.tar.gz -C /usr/local/
 sudo rm -rf /usr/bin/python
 sudo ln -s /usr/local/python2.7.14/bin/python /usr/bin/python
 ```
下载 Python SDK 和参考示例代码
```
 git clone https://github.com/MyDuerOS/DuerOS-Python-Client.git
 cd DuerOS-Python-Client
 git checkout raspberry-dev
 ```
注意，上面代码有一条命令 `git checkout raspberry-dev` 非常重要，这表示切换到 raspberry-dev 分支，因为这个 Python SDK 的 master是针对 Linux 系统的，不切换到与树莓派相对应的分支可能会导致这个 SDK 无法使用。

### 运行和测试
授权
```
 ./auth.sh
```
直接运行将使用默认的 client_id 和 client_secret。开发者也可以自己在 [DuerOS 开放平台](https://dueros.baidu.com/open)提供的智能设备开放平台申请 client_id 和 client_secret，进而实现在控制台自定义配置属性。下面讲一下申请 id 的方法。

登录[DuerOS 开放平台](https://dueros.baidu.com/open)，选择“智能设备开放平台”，点击“配置新设备”，然后根据实际情况填写相关内容，记住你为设备取的名字，因为后面会用到。点击下一步，直到操作完成。最后，在基础信息里面，可以看到你刚刚配置的这个新设备的 client_id 和 client_secret。

之后，登录[百度开发者中心](http://developer.baidu.com/console#app/project)，在这里找到刚刚配置的新设备的名称，点击进去，发现在基础信息里面的 API Key 和
Secret Key 与我们之前看到的 client_id 和 client_secret 是一样的。此时，我们把“安全设置”的“授权回调页"，设置成
http://127.0.0.1:3000/authresponse

授权回调页设置
![](https://ws2.sinaimg.cn/large/006tNc79ly1fz56npbrk2j31ek0qymy9.jpg)

运行
```
 ./wakeup_trigger_start.sh
```
说出唤醒词(默认是“小度小度”)，识别之后，会听到音箱发出“叮”的一声，之后说出指令，比如“今天天气怎么样？”。系统会播报今天的天气。

下面这个命令是使用enter按键触发识别
 ```
 ./enter_trigger_start.sh
 ```
在控制台按下 enter 键即触发唤醒，无需使用唤醒词。

至此，你已经完成了 DuerOS 的安装调试，完成了一套完整的智能语音系统的部署。这套系统跟市面上出售的百度智能音箱没什么区别，后台用的都是 DuerOS,如果你愿意，可以把它当智能音箱来使用。但对于打造智能机器人来说，这只是完成了一小步，接下来，我们要做一些开发工作，用语音来控制机器人。


系列目录：

《手把手教你打造智能语音机器人（0）-写在前面的话》

《手把手教你打造智能语音机器人（1）-强大的 DuerOS 系统》

《手把手教你打造智能语音机器人（2）-配置属于自己的语音交互机器人》

《手把手教你打造智能语音机器人（3）-用语音控制机器人》

《手把手教你打造智能语音机器人（4）-用 YOLO 算法搭建目标检测系统》

《手把手教你打造智能语音机器人（5）-集成并运行目标检测系统》