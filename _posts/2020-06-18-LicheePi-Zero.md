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

http://cn.dl.sipeed.com/LICHEE/Zero

在 Image >> dd_img 下有介绍各个镜像的内容的readme：

>   pack_zero_img.sh -->pack script
>
>   minX_dd.tar.bz2  -->min system include Xorg
>
>   mindb_dd.tar.gz  -->min Debian，include gcc,python, etc.
>
>   brpy_dd.tar.bz2  -->buildroot image，include python etc.(no gcc)
>
>   brmin_dd.tar.bz2  -->min buildroot
>
>   minmin_dd.tar.bz2  -->min Debian (almost nothing but apt)
>
>   lichee_zero_test_Debian_LXDE.tar.bz2  -->Debian with LXDE

这里有buildroot和debian系统，可以自行选择下载。

### 烧录

烧录这里用到两个软件：**Win32DiskImager** 和 **SD Card Formatter**

前者用于格式化SD卡（因为windows无法读取到ext格式的分区，所以要用特殊软件格式化）；后者用于镜像的烧录。

首先用SD Card Formatter进行格式化：

![sdcardformatter](/img/lichee/sdformat.png)

然后用Win32DiskImager进行烧录。

图片

### 擦腚 开基

默认终端输出在UART0，我们需要准备好**USB转串口设备**和**Putty**，串口的连接和putty的使用这里不过多介绍。

把烧录好的SD卡插入荔枝派，然后上电。

接下来我们就可以看到终端的输出了！

图片

默认登录账号：`Account:root Password:toortoor`

至此，已经完成了伟大的第一步，接下来开始各种坑。

## 联网

这就是我后悔为什么没一起买SDC接口网卡，如果买了SDC接口网卡的话应该会省去很多麻烦。

于是乎我买了一个USB网卡。买网卡要注意网卡有没有Linux下的驱动，这里我买的是EDUP的一款网卡，主控用的是RTL8188CUS芯片。

图片

官方的镜像并没有添加相关驱动，于是折腾之旅就开始了。

### 编译Linux内核

参考：http://zero.lichee.pro/

首先，我们需要准备一下交叉编译器：

```
// 下载编译器
wget https://releases.linaro.org/components/toolchain/binaries/latest/arm-linux-gnueabihf/gcc-linaro-6.3.1-2017.05-x86_64_arm-linux-gnueabihf.tar.xz
// 解压
tar xvf gcc-linaro-6.3.1-2017.05-x86_64_arm-linux-gnueabihf.tar.xz
// 将编译器移动到opt目录下
mv gcc-linaro-6.3.1-2017.05-x86_64_arm-linux-gnueabihf /opt/
// 在bash.bashrc内添加编译器的环境变量
vim /etc/bash.bashrc
# add: PATH="$PATH:/opt/gcc-linaro-6.3.1-2017.05-x86_64_arm-linux-gnueabihf/bin"
// 验证编译器是否能成功使用
arm-linux-gnueabihf-gcc -v
```

待续......