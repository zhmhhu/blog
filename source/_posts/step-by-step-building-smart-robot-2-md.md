---
title: 手把手教你打造智能语音机器人（2）-配置属于自己的语音交互机器人
entitle: step-by-step-building-smart-robot-2.md
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws3.sinaimg.cn/large/006tNc79ly1g44j8lvo92j30u00k0wfz.jpg
date: 2019-06-05 18:43:30
keywords:
description:
---

## DuerOS 的 基本配置

在上一篇文章中，我们已经讲到下载 DuerOS 的 Python SDK，并已经在树莓派上安装调试成功了。

至此为止，你的设备已经具备了百度智能音箱同样的功能。此外，百度还提供智能设备的个性化开发，其中包括对音色、语速、音量等语音风格的设置，还包括对设备本身拟人化的设置。比如，你可以问：你叫什么名字，你家在哪里，你爸爸是谁等等这些问题。而问题的答案，你可以事先设置好。这样一来，这个智能机器人就更加具有生命力了。

设置方法是，打开[DuerOS 开放平台](https://dueros.baidu.com/open)，登录,选择控制台 ->设备控制台。在这里能够看到我们在上一节讲到的申请的智能设备。点击“设置” ，在“端能力配置”中，试着配置音色、音速等等，点击保存。之后，再试着唤醒你的智能设备，看看音色和音速有没有改变。

DuerOS 默认提供的语音能力有很多，包括播放新闻、查询天气、报时、闹钟、计时、播放音乐、播放相声小品等等。在“云服务配置”页面中，都可以自由设置。

除此之外，对打造智能机器人来说，非常重要的一项配置就是自定义指令。用户可以上传意图文件（intent.dic）、词典文件（dict.dic）和命令实例（command.dic）来配置自定义指令。举个例子，假如你要控制智能机器人往前行驶 5 秒钟，就需要事先制定好指令，在词典文件中添加控制方向的词“往前”，控制动作的词“行驶”，以及表达时间的词“秒”。这些设置都在“云服务配置”里面。具体操作，在后期开发中再展开来讲。

## DuerOS 的 Python SDK
之前说过，DuerOS 是一套对话式人工智能系统，它提供了强大的语音交互能力，借助百度本身具有的海量的内容，为用户提供丰富的内容服务，并且支持部署在树莓派等硬件上面，提供个性化的开发服务。DuerOS 的人工智能能力在服务端上，调用这些能力，需要遵循  DuerOS 的 服务端与设备端之间的通讯协议——DCS协议。DCS 协议 在 HTTP/2 协议之上，封装了众多多请求/响应消息格式，我们不必去关心 DCS 协议的细节，因为已经有人提供了 SDK 和相应的示例，我们可以直接调用。

我们在上一节使用的 [DuerOS-Python-Client](https://github.com/MyDuerOS/DuerOS-Python-Client) 就是
Python 语言版本的 SDK。当成功唤醒设备的那一刻，也就代表了 SDK 提供的示例已经成功运行了。

DuerOS-Python-Client 代码结构如下图所示，

![代码结构](https://raw.githubusercontent.com/zhmhhu/DuerOS-Python-Client/raspberry-dev/readme_resources/%E4%BB%A3%E7%A0%81%E7%BB%93%E6%9E%84.png)

其中，

DuerOS-Python-Client:项目根目录

DuerOS-Python-Client/auth.sh:认证授权脚本
DuerOS-Python-Client/enter_trigger_start.sh:[Enter]按键触发唤醒脚本
DuerOS-Python-Client/wakeup_tirgger_start.sh:[小度小度]触发唤醒脚本
DuerOS-Python-Client/app:应用目录

DuerOS-Python-Client/app/auth.py:认证授权实现模块
DuerOS-Python-Client/app/enter_trigger_main.py:[Enter]按键触发唤醒实现模块
DuerOS-Python-Client/app/wakeup_tirgger_main.py:[小度小度]触发唤醒实现模块
DuerOS-Python-Client/app/framework:平台相关目录
DuerOS-Python-Client/app/framework/mic.py:录音模块(基于pyaudio)
DuerOS-Python-Client/app/framework/player.py:播放模块(基于GStreamer)
DuerOS-Python-Client/app/snowboy:snowboy唤醒引擎
DuerOS-Python-Client/sdk:dueros sdk目录

DuerOS-Python-Client/sdk/auth.py:授权相关实现
DuerOS-Python-Client/sdk/dueros_core.py:dueros交互实现
DuerOS-Python-Client/sdk/interface:端能力接口实现

下一步，我们开始做一些定制开发。

系列目录：

《手把手教你打造智能语音机器人（0）-写在前面的话》

《手把手教你打造智能语音机器人（1）-强大的 DuerOS 系统》

《手把手教你打造智能语音机器人（2）-配置属于自己的语音交互机器人》

《手把手教你打造智能语音机器人（3）-用语音控制机器人》

《手把手教你打造智能语音机器人（4）-用 YOLO 算法搭建目标检测系统》

《手把手教你打造智能语音机器人（5）-集成并运行目标检测系统》