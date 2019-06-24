---
title: 手把手教你打造智能语音机器人（5）-集成并运行目标检测系统
entitle: step-by-step-building-smart-robot-5.md
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws2.sinaimg.cn/large/006tNc79ly1g4bdy6b8u8j30hf0ehjro.jpg
date: 2019-06-05 18:43:44
keywords:
description:
---

## 目标检测测试

我们不打算将一个深度学习模块整合到相机中，原因在于树莓派本身的硬件配置不高，恐怕难以支持深度学习的计算量。
我们准备将树莓派“挂钩”到摄像头上，然后通过WiFi来发送照片。简单说来，就是将树莓派捕捉到的视频画面发送给一台 web服务器，然后，服务器进行计算，识别图像，再将结果返回。

 ![](
 https://github.com/burningion/poor-mans-deep-learning-camera/raw/master/images/deeplens.png)

我们这里所使用的计算机需有较高的处理能力，并且部署 YOLO 神经网络架构服务来检测输入的图像画面。

在正式使用树莓派之前，我们做一下单机测试。并判断小鸟是否出现在画面内。 代码参考[此处](https://github.com/zhmhhu/poor-mans-deep-learning-camera) 

如果像框内检测到了小鸟，那我们就保存图片并进行下一步分析。

## 配置并使用树莓派摄像机

模块`/Inference-Computer/predict_flask.py` 将目标检测服务通过 Flask 发布出来。

当我们启动了树莓派之后，首先需要根据IP地址来判断服务器是否正常工作，然后尝试通过Web浏览器来访问服务器。 URL地址格式类似如下： http://192.168.1.4:5000/image.jpg 。在树莓派中加载 Web 页面及图像来确定服务器是否正常工作。

系列目录：

《手把手教你打造智能语音机器人（0）-写在前面的话》

《手把手教你打造智能语音机器人（1）-强大的 DuerOS 系统》

《手把手教你打造智能语音机器人（2）-配置属于自己的语音交互机器人》

《手把手教你打造智能语音机器人（3）-用语音控制机器人》

《手把手教你打造智能语音机器人（4）-用 YOLO 算法搭建目标检测系统》

《手把手教你打造智能语音机器人（5）-集成并运行目标检测系统》
