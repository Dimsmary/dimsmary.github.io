---
layout: post
title: 荔枝派-ZERO踩坑记录
categories: [blog ]
tags: [Share, Tutorial]
description: "关于荔枝派Zero的开发心路历程以及技术分享"
---
## 一个前言
最近在搞一个项目，需要用到：

- 摄像头

- 屏幕

- OpenCV

用OpenCV的话是想实现图像的处理和人脸识别功能。

本来打算用树莓派Compute Module或者Zero，但是感觉有点杀鸡用牛刀。

这时候我想起来之前看到的荔枝派Zero：价格在50元左右，搭载的是全志的V3S Armv7处理器，主频最高据说可以达到1.2Ghz；内置了64M的内存。

然后看了看荔枝派官网的文档，感觉资料还算全？然后就决定项目基于荔枝派Zero来开发。


## 正式入坑
![淘宝图](/img/lichee/taobao.png)

说买就买，这里我直接买的裸板，送了一个micro USB的OTG。说实话，没买这个WIFI模块有点后悔，这事儿到后面说。

### 启动方式

根据全志V3S的芯片手册，V3S有两种启动方式：通过SPI接口启动或者通过SDC接口启动。

默认的荔枝派Zero板是没有焊接SPI存储芯片的，不过我也不打算用。我直接通过SDC接口（即SD卡）进行启动。

官方有给出对应的系统dd镜像：

[荔枝派zero镜像]: http://cn.dl.sipeed.com/LICHEE/Zero

在 Image >> dd_img 下有介绍各个镜像的内容：

>   pack_zero_img.sh -->pack script
>   minX_dd.tar.bz2  -->min system include Xorg
>   mindb_dd.tar.gz  -->min Debian，include gcc,python, etc.
>   brpy_dd.tar.bz2  -->buildroot image，include python etc.(no gcc)
>   brmin_dd.tar.bz2  -->min buildroot
>   minmin_dd.tar.bz2  -->min Debian (almost nothing but apt)
>   lichee_zero_test_Debian_LXDE.tar.bz2  -->Debian with LXDE

待续...