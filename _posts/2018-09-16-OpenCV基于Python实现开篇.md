---
layout:     post
title:      "OpenCV基于Python实现开篇"
subtitle:   "安装和教程"
date:       2018-09-16
author:     "Soson"
header-img: "img/home-bg-geek.jpg"
tags:
- OpenCV
---



- OpenCV and Python


>OpenCV是开源、跨平台的计算机视觉库，由英特尔公司发起并参与开发，在商业和研究领域中可以免费使用，提供了C++、C、Python、Java接口，并支持Windows、Linux、Android、Mac OS平台。


**OpenCV可以做什么?**

>图像文件的读取和显示，图像和视频处理操作，图像处理的基本操作（边缘检测等），深度估计与分割，人脸检测与识别，图像的检索，目标的检测与识别，目标跟踪（安全和监控领域的工具），神经网络的手写体识别。


**OpenCV基于Python的实现需要用到的一些库：**

```
Numpy：矩阵计算函数
SciPy：与NumPy密切相关的科学计算库
OpenNI：支持一些深度摄像头，如Asus的XtionPRO
SensorKinect：是OpenNI插件，支持微软的Kinect深度摄像头
```

>Windows系统安装

```
1.使用二进制安装程序（不支持使用摄像头）
安装Anaconda Python
2.使用CMake和编译器，如果需要支持深度摄像头（如Kinect），需要安装OpenNI和SensorKinect
```

>OS X系统安装

```
使用MacPorts的现成包
使用Homebrew的现成包（不支持深度摄像头）
```

>Ubuntu系统安装

```
1.使用Ubuntu的资源库（不支持深度摄像头）
2.从源代码构建OpenCV
```

>OpenCV教程

```
C++：https://docs.opencv.org/2.4/doc/tutorials/tutorials.html
Python:https://github.com/abidrahmank/OpenCV2-Python/tree/master/Official_Tutorial_Python_Codes
```

>OpenCV官网：https://opencv.org
>
>OpenCV帮助文档：http://docs.opencv.org
>
>OpenCV GitHub地址：https://github.com/opencv/opencv
>
>（里面存在部分不同平台使用OpenCV的Sample）

```
OpenCV针对平台的SDK下载地址（iOS，Android，C++等）
https://sourceforge.net/projects/opencvlibrary/
```

