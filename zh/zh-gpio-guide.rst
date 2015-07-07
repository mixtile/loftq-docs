.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

===========================================================
LOFT-Q GPIO 使用
===========================================================
:组织: Mixtile Team 
:版本: 0.1
:日期: 2015.07.07

.. contents:: 内容


LOFT-Q 提供了众多的 GPIO 口用于开发者根据自己的需要进行扩展。不过这些 GPIO 的使用需要配合具体的扩展板才能使用。

对于 GPIO 的使用需要在内核中启用相关的配置，然后就可以在应用程序中使用相关的 GPIO 进行操作。

.. raw:: pdf

   PageBreak

介绍
--------------------------------------------------------------

LOFT-Q 提供了 3 个 FPC 座子，分别为 J4, J5, J6，其中每个提供 60 针的 GPIO 支持，所有合计有 180 针的 GPIO 可供使用。具体的每个针脚的说明可以参考 `GPIO 引脚说明`_ 。如下图为 LOFT-Q 各个部分的简略图，其中有关 GPIO 的对照如下：

* 2 Expansion CSI/MIPI-DSI 为摄像头所用 IO 口，也可以作为通用 GPIO 使用。
* 3 Expansion LVDS 为显示屏，触摸屏所用 IO 口，也可以作为通用 GPIO 使用。
* 5 Expansion CSPIO 包含一些可用作 SPI，UART，I2C 等接口的 GPIO, 以及通用的 GPIO。

.. image:: ../_static/pictures/loftq-interface-map.png
  :width: 100%
  :align: center

.. raw:: pdf

   PageBreak

GPIO 引脚说明
--------------------------------------------------------------


EXPANSION CSI/MIPI‐DSI --- J4
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''


=========    =============    =============    =============
引脚号        功能             引脚号            功能
=========    =============    =============    =============
1             CSI-D0           2                CSI-D1
3             CSI-D2           4                CSI-D3	
5             CSI-D4           6                CSI-D5
7             CSI-D6           8                CSI-D7
9             CSI-D8           10               CSI-D9
11            CSI-D10          12               CSI-D11
13            GND              14               CSI-HSYNC
15            CSI-VSYNC        16               TWI0-SCK
17            TWI0-SDA         18               GND
19            CSI-MCLK         20               CSI-PCLK
21            GND              22               VDD1V8-CSI
23            VDD1V8-CSI       24               GND
25            VCC-5V           26               VCC-5V
27            GND              28               VCC-3V3
29            VCC-3V3          30               GND
31            MCSI-MCLK        32               NC
33            NC               34               GND
35            DSI-D0N          36               DSI-D0P
37            GND              38               DSI-D1N
39            DSI-D1P          40               GND
41            DSI-D2N          42               DSI-D2P
43            GND              44               DSI-D3N
45            DSI-D3P          46               GND
47            DSI-CKN          48               DSI-CKP
49            GND              50               SPDIF-IN
51            SPDIF-OUT        52               GND
53            I2S1-DIN         54               I2S1-BCLK
55            I2S1-LRCK        56               I2S1-MCLK
57            EARGND2          58               LRADC0
59            EARGND2          60               LRADC1
=========    =============    =============    =============


EXPANSION GPIO --- J5 
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''


=========    =============    =============    =============
引脚号        功能             引脚号            功能
=========    =============    =============    =============
1             VCC-5V           2                VCC-5V
3             GND              4                VCC-LCD	
5             VCC-LCD          6                GND
7             UART4-TX         8                UART4-RX
9             GND              10               VCC-JTAG
11            VCC-JTAG         12               AP-RESET#
13            TMS0             14               TCK0
15            TDO0             16               TDI0
17            JTAG-SEL0        18               GND
19            SPI0-MOSI        20               SPI0-MISO
21            SPI0-CLK         22               SPI0-CS0
23            GND              24               CSI2-D0N
25            CSI2-D0P         26               GND
27            CSI2-D1N         28               CSI2-D1P
29            GND              30               CSI2-D2N
31            CSI2-D2P         32               GND
33            CSI2-D3N         34               CSI2-D3P
35            GND              36               DSI-D0P
37            CSI2-CKP         38               GND
39            TWI3-SCK         40               TWI3-SDA
41            GND              42               MCS-MCLK1
43            GND              44               CAM-R-STBY-EN
45            CAM-R-RESET#     46               GND
47            PH0              48               PH1
49            PH2              50               PH3
51            PH4              52               PH5
53            PH6              54               PH7
55            PH8              56               PH29
57            PH30             58               GND
59            USB-DP0          60               USB-DM0
=========    =============    =============    =============


EXPANSION LVDS --- J6
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''


=========    =============    =============    =============    =============   =============
引脚号        第一功能         第二功能          引脚号           第一功能         第二功能
=========    =============    =============    =============    =============   =============
1             LVDS0-D0P        LCD0-D0          2                LVDS0-D0N       LCD0-D1
3             GND                               4                LVDS0-D1P       LCD0-D2
5             LVDS0-D1N        LCD0-D3          6                GND
7             LVDS0-D2P        LCD0-D4          8                LVDS0-D2N       LCD0-D5
9             GND                               10               LVDS0-CLKP      LCD0-D6
11            LVDS0-CLKN       LCD0-D7          12               GND
13            LVDS0-D3P        LCD0-D8          14               LVDS0-D3N       LCD0-D9
15            GND                               16               LVDS1-D0P       LCD0-D10
17            LVDS1-D0N        LCD0-D11         18               GND
19            LVDS1-D1P        LCD0-D12         20               LVDS1-D1N       LCD0-D13
21            GND                               22               LVDS1-D2P       LCD0-D14
23            LVDS1-D2N        LCD0-D15         24               GND
25            LVDS1-CLKP       LCD0-D16         26               LVDS1-CLKN      LCD0-D17
27            GND                               28               LVDS1-D3P       LCD0-D18
29            LVDS1-D3N        LCD0-D19         30               GND
31            LCD0-D20                          32               LCD0-D21
33            LCD0-D22                          34               LCD0-D23
35            LCD0-CLK                          36               LCD0-HSYNC
37            LCD0-DE                           38               LCD0-VSYNC
39            LCD-PWM                           40               LCD-BL-EN
41            GND                               42               CTP-WAKE
43            CTP-INT                           44               TWI1-SCK
45            TWI1-SDA                          46               GND
47            RTP-X1                            48               RTP-X2
49            RTP-Y1                            50               RTP-Y2
51            GND                               52               VCC2V8-LCD
53            VCC2V8-LCD                        54               VCC2V8-LCD
55            GND                               56               VCC1V8-LCD
57            VCC1V8-LCD                        58               GND
59            VCC-5V                            60               VCC-5V
=========    =============    =============    =============    =============   =============


GPIO 内核配置
--------------------------------------------------------------

.. note::

   本节中所说的内核配置针对全志提供的旧版内核，而非主流内核。不过方法相同。

在用户应用使用 GPIO 之前，需要在内核中配置 GPIO 的 sysfs 支持。具体的配置方式如下：

.. code-block:: sh

    Device Drivers  ---> 
                    GPIO Support  ---> 
                                /sys/class/gpio/... (sysfs interface)

在内核中，启用上述配置即可，默认在 **arch/arm/configs/loftq_linux_defconfig** 中已经启用了该配置。

GPIO 使用示例
--------------------------------------------------------------

使用上述配置生成的内核，然后可以在应用中通过下面的示例进行测试。下面分别是通过命令行和C语言进行访问和测试的方法，基本的过程如下：

* 根据 GPIO 名称计算实际对应的 gpio 编号
  
  全志方案的 gpio 编号计算方式： **(第二个字母在字母序列中的位置 - 1) x 32 + 针口编号** 。例如 **PC13** 或者 **pc13** ，那么它的计算方式分别如下：
  
  **pc13** 的编号计算： ('c'-'a')*32 + 13 ，结果为 77
  
  **PC13** 的编号计算： ('C'-'A')*32 + 13 ，结果为 77
  
* 导出 GPIO 访问路径和接口
* 设置 GPIO 使用方式为输入或者输出
* GPIO 读取或者写入访问
* 取消 GPIO 导出

命令行访问和测试
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

我们以 "PC13" 为例作为测试。首先是确定其实际的 gpio 编号。根据前面介绍中的计算方式，可以知道 "PC13" 的 GPIO 编号为 **77** 。

* 导出 GPIO 访问路径命令：

  .. code-block:: sh
  
     echo 77 > /sys/class/gpio/export
     
* 设置 GPIO 访问方式，输入或者输出，前一个为设置为输出模式，后一个设置为输入模式：

  .. code-block:: sh
  
     echo "out" > /sys/class/gpio/gpio77/direction
     
  .. code-block:: sh
  
     echo "in" > /sys/class/gpio/gpio77/direction
     
* 如果设置为输出模式，下述分别为将 GPIO 设置为高和低的输出：

  .. code-block:: sh
  
     echo 1 > /sys/class/gpio/gpio77/value 
     
  .. code-block:: sh
  
     echo 0 > /sys/class/gpio/gpio77/value 
     
* 如果设置为输入模式，可以通过下属命令读取当前 GPIO 的状态：

  .. code-block:: sh
  
     cat /sys/class/gpio/gpio77/value 
  
* 取消 GPIO 导出：

  .. code-block:: sh
  
     echo 77 > /sys/class/gpio/unexport

C语言访问和测试
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

对于 C 语言的访问方式与上述方式相同，只是所有的文件打开和访问，写入操作均使用 C 语言的方法来完成。

具体的实现可以参考我们的 jennic-util 中所使用的 gpio 相关代码。

* gpio 参考： https://github.com/pengphei/jennic-utils/blob/master/util_gpio.c
* 使用示例： https://github.com/pengphei/jennic-utils/blob/master/util_loftq.c

更多参考
--------------------------------------------------------------

linux-sunxi 社区 GPIO 使用： http://linux-sunxi.org/GPIO

