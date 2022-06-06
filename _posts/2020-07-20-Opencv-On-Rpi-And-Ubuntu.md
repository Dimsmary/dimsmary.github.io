---
layout: post
title: 编译、安装Open-CV（4.1.2/3.4.9）
categories: [blog ]
tags: [Share, Tutorial]
description: "在Ubuntu和树莓派上安装OpenCV"
---
## 前言
***作者水平有限，内容可能存在不正确的地方，欢迎批评指正。**

​	一直用的都是python的OpenCV库，安装非常简单，直接pip就能完成安装。但是最近需要用到C++的OpenCV库，这就有点难受了，需要自己编译安装。下面写出安装的全过程。



## 准备

​	在ubuntu上和在树莓派上编译安装OpenCV的流程是一样的。

​	首先需要安装依赖（确保apt更新到最新）：

```
sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
gfortran openexr libatlas-base-dev python3-dev python3-numpy \
libtbb2 libtbb-dev libdc1394-22-dev
```

​	编译安装OpenCV需要准备好OpenCV的源代码，在~目录下创建名为OpenCV_build的文件夹，并通过git克隆OpenCV的源代码：

```
mkdir ~/opencv_build && cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
```

​	创建并进入build目录：

```
cd ~/opencv_build/opencv
mkdir build && cd build
```

## 编译

接下来我们需要使用cmake生成makefile编译文件，这里注意opencv_contrib/modules路径的设置：

```
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D INSTALL_C_EXAMPLES=ON \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
-D BUILD_EXAMPLES=ON ..
```

接下来进行编译，这里的x指的是使用几个CPU核心进行编译。

但在树莓派3b+上编译时使用make -j4时会卡死，可以先使用make -j4编译到卡死的地方，然后重启后再使用make不加参数继续编译。

当然不嫌慢也可以直接使用make进行编译。

```
make -jx
```

## 出错解决

#### 缺失xfeatures2d相关文件（4.1.2/3.4.9均出现该问题）

编译的过程可能不会一帆风顺，其中一个问题就是提示xfeatures2d的相关文件缺失。

这是因为在编译过程中需要下载一些相关的文件，而因为一些奇妙的网络问题会导致下载失败，从而导致编译失败。

解决方法：打开build目录下的CMkaeDownloadLog.txt文件，搜索缺失的文件名将可以找到下载地址，手动下载后复制到~/opencv_contrib/opencv/modules/xfeatures2d/src

#### 缺失xfeatures hpp文件（4.1.2）

```
cp -r ~/opencv_build/opencv/modules/features2d ~/opencv_opencv/opencv/build
```

#### 缺失cuda.hpp(3.4.9)

在~/opencv_contrib/opencv/CMakeLists.txt中添加：

```
INCLUDE_DIRECTORIES("~/opencv_build/opencv_contrib/modules/xfeatures2d/include")
```

## Raspberry Pi Zero?

众所周知，RPI-0的性能极其不行，编译OpenCV几乎要耗费十几个小时。有没什么解决方案呢？

最简单的解决方法就是在性能强的树莓派上（4/3B+）编译好opencv，然后把/usr/local的内容全部复制到树莓派0的文件系统中。

这样可能会导致一些小问题，但正常使用OpenCV是没有问题的。