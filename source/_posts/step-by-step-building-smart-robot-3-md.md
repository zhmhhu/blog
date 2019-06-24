---
title: 手把手教你打造智能语音机器人（3）-用语音控制机器人
entitle: step-by-step-building-smart-robot-3.md
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws3.sinaimg.cn/large/006tNc79ly1g4bdplbp6cj30oh0drdgl.jpg
date: 2019-06-05 18:43:36
keywords:
description:
---
## 增加语音控制模块

我们先在 DuerOS-Python-Client/sdk/interface 目录下新建模块 wifirobots.py

此模块提供智能机器人的基本操作，包括前进后退、手臂动作等等。

实现定义好 GPIO 引脚,并接好线路。在接口不够的情况下，可使用面包板。

```
class robots(object):

    def __init__(self,dueros, player):
        '''
        类初始化
        :param dueros:DuerOS核心模块实例
        :param player: 播放器实例（平台相关）
        '''
        self.namespace = 'ai.dueros.device_interface.thirdparty.robot.xiaobai'
        self.dueros = dueros
        self.token = ''
        self.state = 'IDLE'

        self.player = player
        self.player.add_callback('eos', self.__playback_finished)
        self.player.add_callback('error', self.__playback_failed)

        print '....WIFIROBOTS START!!!...'

        #######################################
        #############信号引脚定义##############
        #######################################
        GPIO.setmode(GPIO.BOARD)

        ########电机驱动接口定义#################
        ENA = 29  # //L298使能A
        ENB = 31  # //L298使能B
        IN1 = 7  # //电机接口1
        IN2 = 16  # //电机接口2
        IN3 = 13  # //电机接口3
        IN4 = 15  # //电机接口4

        ########舵机接口定义#################
        S_Hand_L = 18
        S_Hand_R = 22
        S_Ear_L = 37
        S_Ear_R = 36

        ########超声波接口定义#################
        ECHO = 32  # 超声波接收脚位
        TRIG = 31  # 超声波发射脚位

        #######################################
        #########管脚类型设置及初始化##########
        #######################################
        GPIO.setwarnings(False)

        #########电机初始化为LOW##########
        GPIO.setup(ENA, GPIO.OUT, initial=GPIO.LOW)
        ENA_pwm = GPIO.PWM(ENA, 1000)
        ENA_pwm.start(0)
        ENA_pwm.ChangeDutyCycle(100)
        GPIO.setup(IN1, GPIO.OUT, initial=GPIO.LOW)
        GPIO.setup(IN2, GPIO.OUT, initial=GPIO.LOW)
        GPIO.setup(ENB, GPIO.OUT, initial=GPIO.LOW)
        ENB_pwm = GPIO.PWM(ENB, 1000)
        ENB_pwm.start(0)
        ENB_pwm.ChangeDutyCycle(100)
        GPIO.setup(IN3, GPIO.OUT, initial=GPIO.LOW)
        GPIO.setup(IN4, GPIO.OUT, initial=GPIO.LOW)

        #########舵机初始化##########
        GPIO.setup(S_Hand_L, GPIO.OUT)
        GPIO.setup(S_Hand_R, GPIO.OUT)
        GPIO.setup(S_Ear_L, GPIO.OUT)
        GPIO.setup(S_Ear_R, GPIO.OUT)

        ##########超声波模块管脚类型设置#########
        GPIO.setup(TRIG, GPIO.OUT, initial=GPIO.LOW)  # 超声波模块发射端管脚设置trig
        GPIO.setup(ECHO, GPIO.IN, pull_up_down=GPIO.PUD_UP)  # 超声波模块接收端管脚设置echo


    ##########机器人方向控制###########################
    def Motor_Forward(self):
        print 'motor forward'
        GPIO.output(self.ENA, True)
        GPIO.output(self.ENB, True)
        GPIO.output(self.IN1, True)
        GPIO.output(self.IN2, False)
        GPIO.output(self.IN3, True)
        GPIO.output(self.IN4, False)

        shutil.copy("/home/pi/DuerOS-Python-Client/app/resources/forward.mp3",
                    "/home/pi/DuerOS-Python-Client/temp.mp3")
        print "语音反馈"
        self.player.play('file://{}'.format('/home/pi/DuerOS-Python-Client/temp.mp3'))
        os.remove("temp.mp3")

        # time.sleep(2)
        # GPIO.output(LED1,False)#LED1亮
        # GPIO.output(LED2,False)#LED1亮

    def Motor_Backward(self):
        print 'motor_backward'
        GPIO.output(self.ENA, True)
        GPIO.output(self.ENB, True)
        GPIO.output(self.IN1, False)
        GPIO.output(self.IN2, True)
        GPIO.output(self.IN3, False)
        GPIO.output(self.IN4, True)
        shutil.copy("/home/pi/DuerOS-Python-Client/app/resources/backward.mp3",
                    "/home/pi/DuerOS-Python-Client/temp.mp3")
        print "语音反馈"
        self.player.play('file://{}'.format('/home/pi/DuerOS-Python-Client/temp.mp3'))
        os.remove("temp.mp3")


        # time.sleep(2)
        # GPIO.output(LED1,True)#LED1灭
        # GPIO.output(LED2,False)#LED2亮
```

左转、右转等操作的代码与前进、后台操作类似。代码可参考 Github


## 开始执行

唤醒之后，可执行“方向前进”指令，看看效果。



系列目录：

《手把手教你打造智能语音机器人（0）-写在前面的话》

《手把手教你打造智能语音机器人（1）-强大的 DuerOS 系统》

《手把手教你打造智能语音机器人（2）-配置属于自己的语音交互机器人》

《手把手教你打造智能语音机器人（3）-用语音控制机器人》

《手把手教你打造智能语音机器人（4）-用 YOLO 算法搭建目标检测系统》

《手把手教你打造智能语音机器人（5）-集成并运行目标检测系统》