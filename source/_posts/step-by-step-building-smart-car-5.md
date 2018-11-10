---
title: 手把手教你打造智能小车（5）-使用舵机打造摄像机云台
entitle: step-by-step-building-smart-car-5
categories:
  - technology
tags:
  - 黑科技
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws1.sinaimg.cn/large/006tNbRwgy1fw00t0jio0j30a307s3yg.jpg
date: 2018-09-24 15:13:26
keywords:
description: 完成智能小车的制作之后，还差一项非常重要的工作，那就是摄像头。
---

## 1 摄像头介绍
完成智能小车的制作之后，还差一项非常重要的工作，那就是摄像头。没有摄像头的小车，跟普通的遥控玩具车没什么区别。我们要给小车装上眼睛，让它可以到我们想要去的地方，传回我们想要看到的画面。给小车安装摄像头的方式非常简单，在树莓派主板上装上摄像头就可以了。推荐树莓派的官方摄像头，它是这个样子的。

![](https://ws1.sinaimg.cn/large/006tNbRwgy1fw00t0jio0j30a307s3yg.jpg)

在关机状态下，将软排线插入到树莓派的 CAMERA 接口上，开机。运行树莓派配置工具来激活摄像头模块：
```
$ sudo raspi-config 
```
移动光标至菜单中的 "Enable Camera（启用摄像头）"，将其设为Enable（启用状态）。完成之后重启树莓派。

在重启完树莓派后，我们就可以使用Pi Cam了。要用它来拍摄照片的话，可以从命令行运行raspistill：
```
$ raspistill -o pic.jpg -t 2000 
```
这句命令将在 2000ms 后拍摄一张照片，然后保存为 pic.jpg。找到这个文件，看看拍摄效果如何。

使用普通的 USB 摄像头也可以，但前提是你能够搞的定它的驱动。

此时，我们只是能够使用摄像头拍照。如何能使摄像头传输实时视频信号，并能够通过客户端访问呢？

## 2 发布实时视频服务
使用 Flask 框架发布Python Web服务，用户可以获得实时视频流数据。

首先要做的是在你的树莓派上安装Flask。之前已经讨论过如何安装 Flask了，在此不再赘述。

由于此项目涉及到比较多的文件，我们要建立一个工作目录。

切换到我们之前创建的 myPiCar 文件夹，使他成为当前工作目录。

现在，在这个文件夹上，我们将创建两个子文件夹：静态的CSS、最终的JavaScript文件以及HTML文件的模板。 转到你的新创建的文件夹。

创建2个新的子文件夹：
```
mkdir static
```
和
```
mkdir templates
```
最终的目录“树”，如下所示：
```
├── myPiCar
        ├── templates
        └── static
```
下面，我们在创建好的的环境下，用Python Web 服务器应用程序来流式传输视频。

下载 Miguel Grinberg 的树莓派相机软件包[camera_pi.py](https://github.com/Mjrovai/Video-Streaming-with-Flask/blob/master/camWebServer/camera_pi.py) 并将其保存在创建的目录myPiCar上。 这是我们项目的核心，Miguel 的安装包相当的不错。

现在，使用Flask，让我们调整原始的 Miguel 的 web 服务器应用程序（app.py），创建一个特定的python脚本来渲染我们的视频。 我们可以命名为appCam.py。
```
from flask import Flask, render_template, Response
 
# Raspberry Pi camera module (requires picamera package, developed by Miguel Grinberg)
from camera_pi import Camera
 
app = Flask(__name__)
 
@app.route('/')
def index():
    """Video streaming home page."""
    return render_template('index.html')
 
def gen(camera):
    """Video streaming generator function."""
    while True:
        frame = camera.get_frame()
        yield (b'--frame\r\n'
               b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')
 
@app.route('/video_feed')
def video_feed():
    """Video streaming route. Put this in the src attribute of an img tag."""
    return Response(gen(Camera()),
                    mimetype='multipart/x-mixed-replace; boundary=frame')
 
if __name__ == '__main__':
    app.run(host='0.0.0.0', port =8000, debug=True, threaded=True)
```
以上脚本将你的摄像机视频流式传输到 index.html 页面上，在 templates 目录下新建 index.html 文件，写入以下内容：
```
<html>
  <head>
    <title>Live Streaming</title>
    <link rel="stylesheet" href='../static/style.css'/>
  </head>
  <body>
    <h1>Live Streaming</h1>
    <h3><img src="{{ url_for('video_feed') }}" width="90%"></h3>
    <hr>
  </body>
</html>
```
index.html 最重要的一行是：
```
<img src="{{ url_for('video_feed') }}" width="50%">
```
视频将会在这里“反馈”到我们的网页上。

在静态目录中需包含style.css文件，这是网页正常显示所必须的样式文件。到目前为止，我们的文件树结构如下。
```
├── myPiCar
      ├── camera_pi.py
      ├── appCam.py
      ├── templates
      |     ├── index.html
      └── static
            ├── style.css
```
所有文件都可以从我的GitHub仓库下载获得：[myPiCar](https://github.com/zhmhhu/myPiCar)。

现在，在终端上运行python脚本：
```
<p>sudo python appCam.py</p>
```
使用你的网络中的任何浏览器，输入 http://树莓派的IP地址:端口号/ ，例如 http://192.168.1.78:8000 ，就可以看到视频信号了。如果你使用树莓派本身的浏览器，也可以输入 http://0.0.0.0:8000 。

不出意外的话，摄像头实时画面已经出现在你的浏览器中了。

可是，此时它是固定不动的。显然，这样的功能是完全不够的。

我们需要摄像头能够上下左右转动，就像球形摄像机一样。这时，我们要打造摄像机云台。

## 2 舵机介绍

我们使用性价比超高的 SG90 舵机来打造摄像机云台。

舵机就是可以自由指定转角的伺服马达。在舵机内部，有一个周期 20ms，脉宽 1.5ms 的基准脉冲，它对应于一个基准电压；为了控制舵机，我们输入一个周期也为 20ms，一定占空比的控制脉冲，这个脉冲经过调制芯片的处理成为一个偏置电压，舵机内部首先通过比较偏执电压和基准电压差值的正负来进行正相或反相转动，同时舵机内部带有平衡电位器，内部齿轮转动的同时会带动电位器变化，电位器会逐渐减小电压差，当电机转到指定角度时电压差值刚好为 0，舵机停止转动。归结起来，要控制舵机转到指定角度，就给它输入指定占空比的脉冲波。


![](https://ws3.sinaimg.cn/large/006tNbRwgy1fw00mxflmlj30jl0a3gmd.jpg)

舵机的转角范围是0-180度，脉宽范围是 0.5ms~2.5ms。对应的控制关系是这样的：

|脉宽(ms)|占空比(%)|转角(°)|
|---|----|----|
|0.5|2.5|0|
|1.0|5|45|
|1.5|7.5|90|
|2.0|10|135|
|2.5|12.5|180|

这一数据可能跟舵机的实际数据有误差，在使用之前，我们需要先校准一下。

## 3 舵机控制

### 3.1 舵机接线
舵机有三根引出的线，红线是电源线，可以接5V输入；棕线是地线，接树莓派 GND 引脚；橙线是信号线，也是我们唯一需要输入控制信号的线，接任何一个 GPIO 引脚。

### 3.2 舵机校准测试

理论上说，在规定范围内，输入信号给多少占空比，必须有个一确定的角度与他对应。在使用之前，我们需要测试一下占空比和角度的对应关系。
打开树莓派终端，输入一下命令进入 python 环境：
```
sudo python
```
之后，一行一行输入一下代码：
```
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD) # 使用BOARD引脚编号方案
tiltPin = 11
GPIO.setup(tiltPin, GPIO.OUT)
tilt = GPIO.PWM(tiltPin, 50) #舵机的信号频率是 50Hz
tilt.start(0) # 初始占空比设为0
```
现在，你可以输入不同的占空比值，观察舵机的运动。让我们从2％开始，看看会发生什么（我们观察舵机从“零位”开始）：
```
tilt.ChangeDutyCycle(2)
```
我项目的情况是，当我将占空比改为3％时，舵机进入零位。我观察到舵机停留在同一位置，开始以大于3％的占空比移动。所以，3％是我的初始位置（o°）。同样的情况发生在10％，我的舵机超过这个数值，最终达到13％。所以对于这个舵机，最终结果是：
0°==>占空比3％
90°==>占空比8％
180°==>占空比13％
完成测试后，必须停止PWM并清理GPIO：
```
tilt.stop()
GPIO.cleanup()
```
实际测试过程中，你的数据可能有所不同。

## 双舵机打造摄像机云台
摄像机有水平和垂直两个方向需要转动，因此需要两个舵机才能满足要求。

“平板”舵机将“水平”移动摄像机（“方位角”），而“仰角”舵机将“垂直”移动摄像机（仰角）。

双舵机需要组合才能使用，你可以自己组装，但是可能需要一些辅助配件。更方便的方式是直接在某宝上选购已经组装好之后的双舵机云台，如下图。
![](https://ws1.sinaimg.cn/large/006tNbRwly1fwo4edrb2nj312w0tc0y8.jpg)

下面，创建一个Python脚本来同时控制两个舵机。
```
from time import sleep
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)
 
pan = 27
tilt = 17
 
GPIO.setup(tilt, GPIO.OUT) # white => TILT
GPIO.setup(pan, GPIO.OUT) # gray ==> PAN
 
def setServoAngle(servo, angle):
    assert angle >=30 and angle <= 150
    pwm = GPIO.PWM(servo, 50)
    pwm.start(8)
    dutyCycle = angle / 18. + 3.
    pwm.ChangeDutyCycle(dutyCycle)
    sleep(0.3)
    pwm.stop()
 
if __name__ == '__main__':
    import sys
    if len(sys.argv) == 1:
        setServoAngle(pan, 90)
        setServoAngle(tilt, 90)
    else:
        setServoAngle(pan, int(sys.argv[1])) # 30 ==> 90 (middle point) ==> 150
        setServoAngle(tilt, int(sys.argv[2])) # 30 ==> 90 (middle point) ==> 150
    GPIO.cleanup()
```
脚本执行时，必须输入参数，平移角度和倾斜角度。 例如：
```
sudo python3 servoCtrl.py 45 120
```
上述命令将使“水平/倾斜”平台在“方位角”（水平角）上定位120度，在“仰角”（倾斜角）方向上定位为45°。 请注意，如果未输入任何参数，则默认平移和倾斜角度均为90°。

现在，自由转动你的摄像机吧。
