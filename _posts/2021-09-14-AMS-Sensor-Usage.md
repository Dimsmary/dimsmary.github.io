---
layout: post
title: AMS系列光谱传感器使用指南，以AS7263为例
categories: [blog ]
tags: [Share, Tutorial]
description: "AMS系列光谱传感器基于UART接口的参数调试、使用方法"
---
# 前言
***作者水平有限，内容可能存在不正确的地方，欢迎批评指正。**

因项目原因需要使用到光谱传感器，在网上冲浪的时候偶然发现了AS7263这款六通道的近红外光谱传感器，价格实惠。其内由六个带有不同通过波长的光电二极管构成，可以探测六个特征波长的光强。

但在国内网络搜索了一圈发现没有中文的使用教程，甚至英文教程也寥寥，只有官方手册硬啃。于是乎在啃完官方手册后决定发布一个中文教程。

AMS系列除了近红外传感器之外，还有可见光等一系列其他的传感器，其他传感器使用方法与本文介绍的AS7263大同小异，可以参考本文基础上结合数据手册使用。

文档及开发工具下载：[下载](/download/as7263_DevelopKit2021.zip) 





# AS7263 传感器简介

> The AS7263 is a digital 6-channel spectrometer for spectral  identification in the near IR (NIR) light wavelengths.
>
> AS7263  consists of 6 independent optical filters whose spectral response is defined in the NIR wavelengths from approximately  600nm to 870nm with full-width half-max (FWHM) of 20nm. 
>
> An  integrated LED driver with programmable current is provided  for electronic shutter applications. 
>
> The AS7263 integrates Gaussian filters into standard CMOS silicon via Nano-optic deposited interference filter technology  and is packaged an LGA package that provides a built in aperture to control the light entering the sensor array. 
>
> Control and Spectral data access is implemented through either  the I²C register set, or with a high level AT Spectral Command  set via a serial UART

上文是AS7263数据手册（*AS7263_Datasheet.pdf*）的简介，把AS7263的主要特性基本介绍了一遍。即：

- 支持六通道近红外（600~870nm）传感
- 每个通道响应的半峰全宽为20nm
- 内置LED驱动器
- LGA封装
- 支持IIC和UART两种方式进行数据输出

在本文的使用中，使用UART对传感器数据进行读取。

芯片推荐在3.3V逻辑电平下工作，下图为该传感器的光谱响应曲线。

![](/img/as7263/spectrum.jpg)

# 基本电路

![](/img/as7263/schematic.jpg)

上图是来自官方AS726X系列传感器的参考设计文件（*AS726x_ReferenceDesign.pdf*）。从图中可以注意到，除了传感器本体以及各种调试接口之外，这里还需要添置FLASH芯片。这可能是因为AS7263采用8051内核，而内部不具有存储单元，需要通过SPI使用外置存储。

![](/img/as7263/applying.jpg)

上图是简化后使用UART接口进行通讯的原理图。其中LED1是功率LED，AS7263能对其提供12.5~100mA的电流。LED2是AS7263工作状态指示LED，AS7263能为其提供1~8mA的电流。

在选择UART接口时，传感器的I2C_ENB引脚应接地。

# FLASH固件刷入

 AS7263的正常运行依赖外部FLASH，若外部FLASH没有刷入固件，则无法正常使用。（在我印象中用于指示工作状态的LED在AS7263外部FLASH没有刷入固件时应该是保持亮起的状态）

在官网的提供EvalSW工具中，可以寻找到名为AS7263_complete.bin的固件，将此固件刷入至FLASH即可。

本文使用的FLASH芯片为官方手册中的AT25SF041，烧录器使用的是某宝50元购置的XTW100。遗憾的是，官网提供的上位机程序并不支持AT25SF041。然而，在一番苦寻后，发现选择另外一款FLASH型号同样能成功刷入（暂时忘了名字叫啥，后续补上）。

 在刷入FLASH后，AS7263即可以正常工作。

# AT命令的使用

AS7263的UART接口默认波特率为115200，8位数据位，1位起始位和1位停止位。

在官方数据手册Figure33给出了完整的AT命令列表，在此便不过多赘述。

# 待补充

暂时更新至此，若有不详细的地方欢迎在本网站的About页面联系本人。