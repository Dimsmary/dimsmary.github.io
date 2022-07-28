---
layout: page
title: "STM"
description: "扫描隧道显微镜开发企划"
header-img: "img/stm.jpg"
---

# 嗨，欢迎来到STM专栏

这里将记录扫描隧道显微镜开发过程的一些资料  ，欢迎持续关注本页面以获取最新STM资讯。  

GitHub页面：[OpenSTM - GitHub](https://github.com/Dimsmary/OpenSTM)

OSHWHub页面：[扫描隧道显微镜OpenSTM - 嘉立创EDA开源硬件平台](https://oshwhub.com/Dimsmary/4ieRpV8S00kGn1MTpsc4MyZat8MwQPzn)

# 版本发布命名规则

截至2022/07，目前已公布了两个不同机械结构的STM方案  

虽然目前STM的搭建方案仍在修改完善，但目前发布的资料集合在一定程度上是能够运行的，故本项目将参考软件发行的方式，在STM方案更新后，采用Release的方式对方案进行发布，每次发布的STM方案版本号命名规则如下：

版本号以**A.B.C**式命名，当方案机械结构存在重构时，A将发生变化。在方案的电路、软件、机械结构存在较大的修改时，B将发生变化。当方案存在细微修改时，C将发生变化。

以1.0.0为例，该版本号即代表第一代机械结构的STM设计方案。

# 已发布的STM方案版本

## <mark>[Release OpenSTM v1.0.0](https://github.com/Dimsmary/OpenSTM/releases/tag/v1.0.0)</mark>

这是初代STM方案，机械结构采用两块铝板搭建（[https://www.bilibili.com/video/BV1Jr4y1v7gq）](https://www.bilibili.com/video/BV1Jr4y1v7gq%EF%BC%89)  
方案较为简单，没有取得能够用于分析的实验结果，但后续方案的搭建基于本初代方案进行搭建，本版本的方案仅供参考，暂不提供详细的文档资料。  
发布的方案文件包括了：

- 3D模型文件（SolidWorks）
- Arduino程序：用于控制STM的ESP32单片机控制程序（采用LVGL进行交互）、基于MPU9250的震动探测程序，均采用Arduino+Platform IO进行开发
- PCB及原理图
- 用于测量干涉条纹的Python脚本
- LTSpice对电源芯片的仿真文件

## <mark>[Release OpenSTM v2.0.0](https://github.com/Dimsmary/OpenSTM/releases/tag/v2.0.0)</mark>

该版本方案为第二代显微镜结构：

https://www.bilibili.com/video/BV1eB4y1S7u8  
该方案的结构能够测量：

- 隧穿距离-电流曲线
- 扫描隧道谱（STS）

v2.0.0.zip内含的文件包括：

- 3DModel：SolidWorks绘制的3D模型文件、CNC加工所需的STEP文件
- PCB：立创EDA专业版绘制的原理图、PCB文件
- SoftWare：在Arduino文件夹下，包含ESP32单片机的控制程序、ATMEGA 328P单片机的控制程序。在Python文件夹下，包含了上位机控制软件、图像转换程序

# 开发技术文档

## STM基础知识

待补充
