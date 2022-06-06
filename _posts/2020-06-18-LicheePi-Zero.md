---
layout: post
title: 荔枝派-ZERO踩坑记录
categories: [blog ]
tags: [Tutorial ]
description: "关于荔枝派Zero的开发心路历程以及技术分享"
---
## 一个前言
***作者水平有限，内容可能存在不正确的地方，欢迎批评指正。**

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

这里有buildroot和debian系统的镜像，可以自行选择下载。

### 烧录镜像

烧录这里用到两个软件：**Win32DiskImager** 和 **SD Card Formatter**

前者用于格式化SD卡（因为windows无法读取到ext格式的分区，所以要用特殊软件格式化）；后者用于镜像的烧录。

首先用SD Card Formatter进行格式化：

![sdcardformatter](/img/lichee/sdformat.png)

然后用Win32DiskImager进行烧录。

![win32disk](/img/lichee/win32.png)

### 擦腚 开基

默认终端输出在UART0，我们需要准备好**USB转串口设备**和**Putty**，串口的连接和putty的使用这里不过多介绍。

把烧录好的SD卡插入荔枝派，然后通过自带的Micro USB口上电。

接下来我们就可以看到终端的输出了！

图片

默认登录账号：`Account:root Password:toortoor`

至此，已经完成了伟大的第一步，接下来开始各种坑。

## 联网

这就是我后悔为什么没一起买SDC接口网卡，如果买了SDC接口网卡的话应该会省去很多麻烦。

于是乎我买了一个USB网卡。买网卡要注意网卡有没有Linux下的驱动，这里我买的是EDUP的一款网卡，主控用的是RTL8188CUS芯片。

图片

官方的镜像并没有添加相关驱动，于是折腾之旅就开始了。

### 整个系统的启动过程

参考：https://d1.amobbs.com/bbs_upload782111/files_52/ourdev_722094VGLOVG.pdf（友善之臂uboot教程）

![kernel](/img/lichee/systemboot.png)

简单来说，linux系统启动分为三个过程，分别是

- 加载BootLoader

- 加载linux内核

- 加载根文件系统

在V3S内部首先会读取SD卡，看看有没有Bootloader，如果没有，则将读取SPI。

BootLoader有很多种，在这里使用的是u-boot作为bootloader。

在u-boot启动后，将会进行一些初始化操作。随后，将会读取linux内核，把系统权限交给内核。

我也是刚刚接触这方面，涉及不深，也没有办法详细介绍了。

在这里我只讲linux内核和根文件系统的基本编译。

### 编译Linux内核

参考：http://zero.lichee.pro/ （荔枝派zero官方wiki）

​			https://www.kancloud.cn/lichee/lpi0/ （荔枝派看云文档）

​			https://whycan.cn/t_561.html （whycan论坛帖子）

默认的官方镜像在linux内核中是没有勾选RTL8188CUS的驱动的，所以我们需要自己编译内核。

**注意，接下来的操作需要在Linux环境下进行**，推荐使用Vmware虚拟机 + Ubuntu系统。

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
// 更新环境变量
source /etc/bash.bashrc
// 验证编译器是否能成功使用
arm-linux-gnueabihf-gcc -v
// 安装设备树编译器
sudo apt install device-tree-compiler
```

接下来需要获取linux内核源代码，这里我尝试过两个不同版本的内核，分别是4.10和4.13，两个版本的内核都可以使用，大家二选一。

```
git clone https://github.com/Lichee-Pi/linux.git //4.10内核版本
git clone https://github.com/Lichee-Pi/linux.git -b zero-4.13.y //4.13内核版本
```

如果github拉取过慢，可以尝试我创建的码云的源：

```
git clone https://gitee.com/dimsmary/linux.git //4.10内核版本
git clone https://gitee.com/dimsmary/linux.git -b zero-4.13.y //4.13内核版本
```

拉取好之后，在当前工作目录应该有名为linux 的文件夹了，我们进入文件夹

```
cd linux
```

首先读取预设好的编译配置

```
make ARCH=arm licheepi_zero_defconfig
```

然后使用menuconfig进入配置页面

```
make ARCH=arm menuconfig
```

通过搜索我们可以找到RTL8188驱动的位置：
![kernel](/img/lichee/kernel_search.png)

在 Device Drivers >> Network device support >> Wireless Lan 下

![kernel](/img/lichee/kernel_8188.png)

按Y勾选，然后保存、退出。

完成后进行内核编译（这里的j4是指允许使用4个线程进行编译，可以加快编译速度）：

```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j4
```

编译完成后，将在arch/arm/boot下生成zImage文件，把这个文件覆盖掉原来SD卡上的zImage文件后即完成了内核的替换。

### 获得网络！

参考：https://wenqixiang.com/linux-wireless-configuration-in-terminal-command-line/

​			https://www.cnblogs.com/ZQQH/p/8366992.html

在替换linux内核后，重新插卡上电。这时候再插入USB网卡，可以看到rtl8192cufw_TMSC.bin镜像被加载。这里可能会提示这个镜像没找到，而加载了代替镜像，这没有关系，也是能正常使用的。

此时我们ifconfig还不能看见wlan0，我们需要：

```
ifconfig wlan0 up
ifup wlan0
```

我们再次ifconfig就能看见wlan0了。

此时我们可以扫描当前的AP：

```
iwlist scan
```

接下来我们需要配置/etc/network/interfaces文件：

```
nano /etc/network/interfaces
```

添加：

```
auto wlan0
iface wlan0 inet dhcp
pre-up ip link set wlan0 up
pre-up iwconfig wlan0 essid ssid
wpa-ssid ssid
wpa-psk password
```

重启后就会自动连接上网络。

## 添加Open-cv 和 Python

可能是因为官方镜像的debian系统问题？执行apt update提示404，没办法升级，安装软件包也各种失败。

于是乎我打算自己编译一个根文件系统。

参考：https://tutorial.linux.doc.embedfire.com/zh_CN/latest/submission/debian9-rootfs.html

### 获取根文件系统

首先创建一个文件夹，名字任意，然后进入这个文件夹：

```
mkdir debian8_rootfs
cd debian8_rootfs
```

安装qemu和 debootstrap

```
sudo apt install qemu-user-static -y
sudo apt install debootstrap -y
```

获取debian8：

```
sudo debootstrap --foreign --verbose --arch=armhf  jessie rootfs http://ftp2.cn.debian.org/debian
```

这里也可以获取9或者10， 如果需要debian9则把jessie改成stretch，10改成buster。

这里选择8的话是因为各种软件包支持比较完整。

出现如下log表示获取成功：

```
...
I: Extracting libmount1...
I: Extracting libsmartcols1...
I: Extracting libuuid1...
I: Extracting mount...
I: Extracting util-linux...
I: Extracting liblzma5...
I: Extracting zlib1g...
```

### 写几个脚本

我们需要写几个脚本和文件来简化操作：

chroot_mount.sh：

```
#! /bin/sh

sudo mount --bind /dev rootfs/dev/
sudo mount --bind /sys rootfs/sys/
sudo mount --bind /proc rootfs/proc/
sudo mount --bind /dev/pts rootfs/dev/pts/
```

chroot_unmount.sh:

```
#! /bin/sh

sudo umount rootfs/sys/
sudo umount rootfs/proc/
sudo umount rootfs/dev/pts/
sudo umount rootfs/dev/
```

chroot_run.sh:

```
#! /bin/sh

sudo LC_ALL=C LANGUAGE=C LANG=C chroot rootfs
```

sources.list：

```
deb http://ftp2.cn.debian.org/debian stretch main
```

把相关脚本和sources.list复制到debian8-rootfs目录内。

准备工作结束后，debian8-rootfs目录内是这样的：

```
tree -L 1
.
├── chroot_mount.sh
├── chroot_run.sh
├── chroot_unmount.sh
├── rootfs
└── sources.list

1 directory, 4 files
```

### 配置根文件系统

首先为脚本们添加可执行权限，然后执行chroot_mount脚本：

```
chmod +x chroot_mount.sh chroot_unmount.sh chroot_run.sh

./chroot_mount.sh
```

复制qemu:

```
sudo cp /usr/bin/qemu-arm-static rootfs/usr/bin/
sudo chmod +x rootfs/usr/bin/qemu-arm-static
```

解压debian8：

```
sudo LC_ALL=C LANGUAGE=C LANG=C chroot rootfs /debootstrap/debootstrap --second-stage --verbose
```

出现如下log表示解压成功：

```
I: Configuring libgnutls30:armhf...
I: Configuring wget...
I: Configuring tasksel...
I: Configuring tasksel-data...
I: Configuring libc-bin...
I: Configuring systemd...
I: Base system installed successfully.
```

替换apt下载源，使软件包下载更顺畅：

```
sudo cp rootfs/etc/apt/sources.list rootfs/etc/apt/sources.list.bak
sudo cp sources.list rootfs/etc/apt/
```

### 安装各种包

接下来执行这个脚本就可以进入根文件系统的shell了：

```
./chroot_run.sh
```

更新一下apt，不过一般都已经是最新的了：

```
apt update
apt upgrade
```

我们可以通过apt来下载各种软件包：

```
apt install libxext-dev
apt install libwebp-dev
apt install libhdf5-dev
apt install libatlas-base-dev
apt install libjasper-dev
apt install libqt4-test
apt install libqtgui4
apt install libilmbase-dev
apt install libopenexr-dev
apt install libhdf5-serial-dev
apt install libopencv-dev
apt install python3
apt install ptyhon3-pip
```

安装完python和opencv的依赖后，我们需要通过pip来安装opencv-python这个库，不过如果这时候直接使用pip3-install opencv-python是会提示找不到包的，因为pip下是没有arm架构的包支持的。

所以我们需要到piwheels.org这个网站手动下载安装。

![kernel](/img/lichee/piwheel.png)

在导航点击packages，进入搜索：

首先搜索numpy，这是opencv-python的依，选择最新的版本下载即可。

（注意这里要选择ABI为cp34m， armv7l的包。因为我们现在安装的python版本为3.4，cpu架构为armv7）

同理下载opencv-python的包。

注意，opencv-python的包要选择较早的版本，否则可能会出现不兼容无法运行的情况。

亲测可以运行的包：

```
numpy-1.16.6-cp34-cp34m-linux_armv7l.whl
opencv_python-3.2.0.8-cp34-cp34m-linux_armv7l.whl
```

在下载完之后，把包复制到debian8-rootfs目录下，另开一个终端把包复制到根文件系统里：

```
sudo cp numpy-1.16.6-cp34-cp34m-linux_armv7l.whl /home
sudo cp opencv_python-3.2.0.8-cp34-cp34m-linux_armv7l.whl /home
```

在shell中，进入/home文件夹，通过pip3指令安装：

```
pip3 install numpy-1.16.6-cp34-cp34m-linux_armv7l.whl
pip3 install opencv_python-3.2.0.8-cp34-cp34m-linux_armv7l.whl
```

安装完成后，清除安装包

```
apt-get clean
```

退出shell

```
exit
```

取消挂载

```
./chroot_unmount.sh
```

删除qemu

```
sudo rm rootfs/usr/bin/qemu-arm-static
```

### 复制到SD卡

***我不确定更换不同文件系统是否需要修改linux内核或u-boot，不过在这里可行。不确保这个方法的正确性。**

清除原有文件系统

```
sudo rm -rf /media/用户名/SD卡的ext分区
```

复制新的文件系统

```
sudo cp -rf rootfs/* /media/用户名/SD卡的ext分区
```

拔卡上电，进入系统你会发现：

```
python3
>>> import cv2
```

成功了！

## 驱动SPI屏幕

参考：[http://zero.lichee.pro/%E8%B4%A1%E7%8C%AE/article%203.html](http://zero.lichee.pro/贡献/article 3.html)

https://github.com/notro/fbtft/wiki

这里使用的是ILI9341为主控的SPI屏幕。

首先和驱动USB网卡类似，需要在menuconfig中配置：

```
Device Drivers  --->
        [*] Staging drivers  --->
        <*>   Support for small TFT LCD display modules  --->
                        <*>   FB driver for the ILI9341 LCD Controller
                <*>   Generic FB driver for TFT LCD displays
```

保存退出后，修改设备树：

在/arch/arm/boot/dts/sun8i-v3s.dtsi中，删除自带的视频输出：

```
               simplefb_lcd: framebuffer@0 {
                       compatible = "allwinner,simple-framebuffer",
                                    "simple-framebuffer";
                       allwinner,pipeline = "de0-lcd0";
                       clocks = <&ccu CLK_BUS_TCON0>, <&ccu CLK_BUS_DE>,
                                <&ccu CLK_DE>, <&ccu CLK_TCON0>;
                       status = "disabled";
-              };
```

在/arch/arm/boot/dts/sun8i-v3s-licheepi-zero.dts中添加一个SPI视频设备：

```
       ili9341@0 {
               compatible = "ilitek,ili9341";
               reg = <0>;

               spi-max-frequency = <15000000>;
               rotate = <270>;
               bgr;
               fps = <10>;
               buswidth = <8>;
               reset-gpios = <&pio 1 7 GPIO_ACTIVE_LOW>;
               dc-gpios = <&pio 1 5 GPIO_ACTIVE_LOW>;
               width = <320>; 
               height=<240>;
               debug = <0>;
       };
};
```

这里可以设置各种参数。比如显示屏的分辨率：width = <320>; height=<240>;

修改完成后，删除/arch/arm/boot/sun8i-v3s-licheepi-zero.dtb这个文件，再进行编译。编译完成后复制sun8i-v3s-licheepi-zero.dtb和内核镜像至SD卡BOOT分区。

然后按照下表连线。

| SPI屏 | zero |
| ----- | ---- |
| 3v3   | 3v3  |
| GND   | GND  |
| DC    | PWM1 |
| RST   | 3v3  |
| CS    | CS   |
| CLK   | CLK  |
| MISO  | MISO |
| MOSI  | MOSI |

必须要连接的线：MOSI/CLK/CS/DC 这四根和电源线，不需要从屏幕读取数据MISO可以不连；LED不需要调光可以接3V3；RST不需要可以接3V3。

开机驱动成功加载后内核将会打印：

```
[    0.860671] fbtft_of_value: buswidth = 8
[    0.864653] fbtft_of_value: debug = 0
[    0.868325] fbtft_of_value: rotate = 270
[    0.872252] fbtft_of_value: fps = 10

[    1.244063] graphics fb0: fb_ili9341 frame buffer, 320x240, 150 KiB video memory, 16 KiB DMA buffer memory, fps=10, spi32766.0 at 15 MHz
```

同时SPI屏幕会进行显示，此时驱动成功。

若未在/arch/arm/boot/dts/sun8i-v3s.dtsi删除默认显示屏，在开机后将会有两个视频设备：

```
>ls /dev/fb*
/dev/fb0 /dev/fb1
```

此时可以输入：

```
cat /dev/urandom > /dev/fb1
```

若屏幕出现雪花，则表示驱动成功。

## 弃坑

到这一步还挺顺利的。接下来问题就出现了！

我在插入USB摄像头之后发现系统没有相关驱动，`ls /dev/video*`也没有任何设备。

说明没！有！驱！动！

于是乎重新编译linux内核，启用V4L相关驱动。拷贝进SD卡，上电之后发现

不！读！U！S！B！了！

在尝试了各种固件之后，发现是官方固件中u-boot有问题，在用别人编译的u-boot的时候，启用V4L后可以正常识别出USB摄像头。不过在更换根文件系统之后出现各种问题，所以可能需要自行编译u-boot。

不过折腾这么久了着实有些枯燥了，于是乎决定暂时放放。最终还是决定放弃荔枝派，拥抱树莓派。

荔枝派在价格上有绝对的优势，但是现在Sipeed似乎已经暂缓了对荔枝派的维护，而且帮助文档也是让新手感觉到一言难尽...

这次入坑荔枝派，我对嵌入式Linux有了一个整体的认识，更深入的研究还是需要比较完整的课程来支持。

后期会打算购买相关的开发板继续挖坑。