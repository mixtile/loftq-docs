.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=============
LOFT-Q 简介
=============
:组织: Mixtile Team 
:编写: Phil.Han <phil.han@focalcrest.com>
:版本: 0.1
:日期: 2015.01.23

|
|
|

.. image:: ../_static/pictures/loftq-board.png
  :width: 50%
  :align: center

|
|

LOFT-Q 是 Mixtile 项目的第二代原型板，基于全志 A31 芯片，主要面向嵌入式开发人员，工程师，创客，以及极客，可以作为家庭影音中心，个人云存储设备，NAS，等。

开放硬件
-----------

LOFT-Q 是一个开放硬件项目，所有有关的资源都会开放，包括源代码，硬件设计资料，文档工具等。目前开放的软件资源如下：

* Uboot
* Linux 内核
* Buildroot
* 文档资料
* 其他工具

对于全志的早期的芯片，开放社区提供了完善的支持，其中的一些资料仍然可用于最新的芯片，包括 LOFT-Q 所使用的A31 芯片。更多有关的信息，可以访问 Linux-sunxi 社区，网站地址：http://linux-sunxi.org 。

相关的硬件资料还在整理中，将会很快予以释放。

软件介绍
----------

LOFT-Q 基于全志设计的 A31 芯片，除了全志以及前述的 Linux-sunxi 社区支持之外，Mixtile 项目在 Github 也提供了相关的开放资源。

* loftq-docs

  LOFT-Q 项目软件相关文档，主要文档包括：LOFT-Q 相关说明（环境搭建，buildroot 构建说明， ubuntu touch, opensuse 等构建说明，android 构建打包说明等）。

  链接地址： https://github.com/mixtile/loftq-docs

* loftq-build

  主要用于完成 LOFT-Q 项目中 uboot, linux, android, buildroot, 以及 ubuntu touch 等的构建，打包工作，包含有原厂的一些构建工具，linaro-gcc 工具链， LOFT-Q 项目的 linux/android 的 sys_config, sys_partition, 开机画面等配置文件。

  链接地址： https://github.com/mixtile/loftq-build

* loftq-uboot

  用于 LOFT-Q 项目中引导 linux/android 以及其他项目的 uboot 代码，基于原厂提供的 A31 的 uboot 源代码。

  链接地址： https://github.com/mixtile/loftq-uboot

* loftq-linux

  用于 LOFT-Q 项目的 linux kernel, 基于原厂提供 A31 内核源代码，并添加了一些适用于 LOFT-Q 的硬件配置。

  链接地址： https://github.com/mixtile/loftq-linux


* mixtile-android-device

  LOFT-Q 定制的 Android 设备固件包。

  Repository: https://github.com/mixtile/mixtile-android-device


* Buildroot

  用于 LOFT-Q 项目的 buildroot，基于 buildroot 官网最新代码，并添加了 LOFT-Q 相关配置文件。将会定期对 buildroot 进行更新以使用最新的软件包。

  链接地址： https://github.com/mixtile/buildroot

* Android

  用于 LOFT-Q 项目的 android 源代码，基于原厂的 A31 的盒子版的 android 4.4.2 版本，并添加适用于 LOFT-Q 原型版的 wifi, 蓝牙等外设的驱动固件。

  链接地址： http://www.mixtile.com/downloads/loft-q/

硬件介绍
----------

原理图下载链接：http://www.mixtile.com/downloads/loft-q/mixtile-loftq-schematic.pdf

LOFT-Q 基本的硬件资料介绍如下：

处理器
'''''''''

* ARM Cortex™-A7 4 核
* 256KB L1 缓存
* 1MB L2 缓存

GPU
''''''

* PowerVR™ SGX544MP2
* 兼容 OpenGL ES2.0, OpenCL 1.x, DX 9_3

视频处理
''''''''''

* UHD H.264 4Kx2K 视频解码
* 多种高清视频解码，包括 Mpeg1/2, Mpeg4 SP/ASP GMC, H.263, H.264,等.
* H.264 高清晰度 ``1080P@60fps`` 编码
* ``3840x1080@30fps`` 3D 解码, 支持 BD/SBS/TAB/FP 
* ``3840x1080@30fps`` 3D 编码
* 支持 RTSP, HTTP, HLS, RTMP, MMS 流媒体协议

显示
''''''''

* Integrated Parallel & MIPI I/F sensor
* Integrated powerful ISP, supporting Raw Data CMOS sensor
* Supports 5M/8M/12M CMOS sensor
* Supports 8/10/12-bit YUV/Bayer sensor

Wifi 和蓝牙
''''''''''''''''

* 正基 AP6234 模组，支持 2.4G/5G Wifi & 蓝牙 4.0
* 802.11 a/g/n 支持

Zigbee
'''''''

* NXP JN5168 低功耗 Zigbee 芯片

其他
'''''

* 2GB 64-bit DDR3
* 板载支持 8GB emmc
* HDMI 支持 ``1080p@60Hz``
* Toslink 接口
* 千兆以太网支持
* 高保真麦克风
* 2.5-inch SATA III 硬盘接口
* 4路 USB2.0 支持
* 板载加速度传感器
* 180 针扩展板
* 红外遥控支持


