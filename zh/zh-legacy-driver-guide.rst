.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=======================
LOFT-Q 旧版内核驱动指南
=======================
:组织: Mixtile Team 
:编写: Phil.Han <phil.han@focalcrest.com>
:版本: 0.1
:日期: 2015.04.20

.. note:: 

   文档还在完善之中，有任何问题，请在我们的相关网站提出。 

LOFT-Q 的旧版内核基于全志提供的 linux-3.3 版本内核。本指南主要对旧版内核的驱动模块进行介绍，在开始本文的阅读和范例前，需要对 LOFT-Q 原型板和 Linux 内核有一个初步的了解，如果能够对内核有更深的了解，将会更加有助于对 LOFT-Q 相关的内核驱动的认识和理解。

驱动介绍
------------------------

在开始对 LOFT-Q 的具体驱动进行探讨前，我们需要对 LOFT-Q 上所有相关的器件以及主芯片 A31 提供的功能有一个初步的介绍。

A31 主芯片功能
^^^^^^^^^^^^^^^^^^^^^^^^

* powervr sgx554 gpu
* ar100 低功耗待机芯片
* cedar 音视频解码模块

LOFT-Q 外围器件
^^^^^^^^^^^^^^^^^^^^^^^^

* HDMI 输出端子
* 4 路 usb 外接口
* 4 路启动切换开关
* RGMII 以太网借口
* ap6234 Wifi 和蓝牙模块
* jn5168 zigbee 芯片
* usb2sata 
* gpio 扩展模块
* toslink 音频输出
* speakerout 扩展口
* line-in 扩展口
* 红外接收器
* 前面板 led 呼吸灯
* uart 调试串口
* pcf8563 rtc
* axp221 电源管理芯片
* 高保真麦克风

内核驱动结构
------------------------

在对上述的 A31 主芯片和外围器件有一个初步了解后，为了能够快速定位相关的代码，我们需要对全志内核中有关 A31 的不同驱动所处的位置有一个基本的了解。

* arch/arm/mach-sun6i/

音频相关驱动：

* sound/soc/sun6i/

字符设备相关驱动：

* drivers/char/ar100_test/
* drivers/char/dma_test
* drivers/char/gpio_test/
* drivers/char/sun6i_g2d/
* drivers/char/sunxi_mem/ 连续内存分配驱动代码
* drivers/char/sunxi_mem_test 连续内存分配测试代码
* drivers/char/timer_test/ timer 测试代码

gpio 相关驱动：

* drivers/gpio/axpio-sunxi.c 
* drivers/gpio/gpio-sunxi.c

gpu 相关驱动：

* drivers/gpu/ion/sunxi android ion 驱动

加速度传感器驱动：

* drivers/gsensor 全志所用的加速度传感器驱动

输入设备相关驱动：

* drivers/input/sw_device.c
* drivers/input/keyboard/sw-keyboard.c 
* drivers/input/keyboard/sun6i-ir.c
* drivers/input/touchscreen/sun6i-ts.c
* drivers/input/touchscreen/AW5306_ts.c
* drivers/input/touchscreen/ft5x_ts.c

多媒体驱动：

* drivers/media/video/sun6i/ 全志 cedar 相关驱动
* drivers/media/video/sunxi-vfe/ 全志 CSI1 v4l2 驱动
* drivers/media/video/sunxi_csi 全志摄像头驱动

其他驱动：

* drivers/misc/sun6i-vibrator.c 
* drivers/misc/sunxi-reg.c 用户寄存器访问驱动
* drivers/misc/sunxi_hw_test.c

emmc 驱动：

* drivers/mmc/host/sunxi-mci.c

rtc 驱动:

* drivers/rtc/rtc-sun6i.c
* drivers/rtc/rtc-pcf8563-sunxi.c

spi 驱动：

* drivers/spi/spi-sun6i.c

usb 驱动：

* drivers/usb/sun6i_usb/
* drivers/usb/host/sw_hci_sun6i.c 

视频卡驱动：

* drivers/video/sun6i/ 全志所有显示设备驱动，包括 disp, hdmi, lcd, 以及 tv 驱动。

安全会话相关：

* modules/aw_schw

powervr SGX DDK:

* modules/eurasia_km

闭源 nand flash 相关部分：

* modules/nand/

