---
title: 手把手教你打造智能语音机器人（4）-用 YOLO 算法搭建目标检测系统
entitle: step-by-step-building-smart-robot-4.md
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws1.sinaimg.cn/large/006tNc79ly1g4bdu3bu5lj30xc0dwjsy.jpg
date: 2019-06-05 18:43:40
keywords:
description:
---
## 初始 YOLO 算法

目前，基于深度学习算法的一系列目标检测算法大致可以分为两大流派： 
1.两步走（two-stage）算法：先产生候选区域然后再进行CNN分类(RCNN系列)， 
2.一步走（one-stage）算法：直接对输入图像应用算法并输出类别和相应的定位(YOLO系列)

R-CNN系列虽然准确率比较高，但是即使是发展到Faster R-CNN，检测一张图片如下图所示也要7fps，为了使得检测的工作能够用到实时的场景中，提出了YOLO。 
![](https://ws1.sinaimg.cn/large/006tNc79ly1g4bdcz6qv8j31q40u0aaw.jpg)

YOLO 的速度足够快，准确率也还不错，因此可以使用在对视频的监测中。



系列目录：

《手把手教你打造智能语音机器人（0）-写在前面的话》

《手把手教你打造智能语音机器人（1）-强大的 DuerOS 系统》

《手把手教你打造智能语音机器人（2）-配置属于自己的语音交互机器人》

《手把手教你打造智能语音机器人（3）-用语音控制机器人》

《手把手教你打造智能语音机器人（4）-用 YOLO 算法搭建目标检测系统》

《手把手教你打造智能语音机器人（5）-集成并运行目标检测系统》
