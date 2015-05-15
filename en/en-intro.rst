.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

LOFT-Q Introduction
===================
:Organization:    Mixtile Team
:Authors:         Phil.Han <phil.han@focalcrest.com>
:Version:         0.1
:Date:            2015.01.23


|
|
|

.. image:: ../_static/pictures/loftq-board.png
  :width: 50%
  :align: center

|
|



LOFT-Q is the second series prototyping board of Mixtle Project. Based on Allwinner A31 soc, This board is well designed for embedded developers and engineers, makers and geekers, which could be used as Home Media Center, personal cloud platform, NAS, etc.



Open Hardware
-------------

LOFT-Q is released as an open hardware project, all the resource will be open to all people focused on it, including source code, hardware designing, documents, tools. For now, we have opened some software resources as below:

* Uboot
* Linux kernel
* buildroot
* documentation
* misc tools

For the Allwinner socs, there is one open community that provides support for its earlier socs. Some of the resources will still work for the later socs, including A31 soc. More info, please refer to the site of Linux-sunxi community: http://linux-sunxi.org.

The Hardware resource is still under working, and will be released soon.


Software
----------

LOFT-Q is based on A31 soc which is designed by Allwinner, and it is supported by linux-sunxi community as desribed above. Mixtile project provides github repository for all open resource.

* loftq-docs

  The documents repository of LOFT-Q project. which includes: guides and manuals related to LOFT-Q project (FAQ, Howtos, and tutorials and detailed guide about how to building buildroot, kernel, android and different linux releases, etc)

  Repository: https://github.com/mixtile/loftq-docs

* loftq-build

  This is the tools set for building, packing uboot, linux kernel, android, buildroot, which contains some referring configurations, like sys_config, sys_partition, boot logo specified for Allwinner socs, gcc tool chain, etc.

  Repository: https://github.com/mixtile/loftq-build

* loftq-uboot

  The Uboot for LOFT-Q project, which is based on the origin vendor with customized configuration for LOFT-Q prototyping board, and can be used for booting linux kernel for linux/Android.

  Repository: https://github.com/mixtile/loftq-uboot

* loftq-linux

  Linux kernel for LOFT-Q project, based on source code provided by the original vendor for A31 soc with specified configuration for LOFT-Q.

  Repository: https://github.com/mixtile/loftq-linux

* mixtile-android-device

  The bsp for Android platform.

  Repository: https://github.com/mixtile/mixtile-android-device

* buildroot

  Buildroot for LOFT-Q project, based on the latest code of official buildroot repository, with LOFT-Q related configuration files and will be updated regularly with the latest official buildroot.

  Repository: https://github.com/mixtile/buildroot

* Android

  Android for LOFT-Q, based on android 4.4.2 Kitkat provided by the original vendor for A31 soc with customized drivers, firmwares of Wifi, Bluetooth, RTC etc.

  Archive: http://www.mixtile.com/downloads/loft-q/

Hardware 
-----------

Schematic link: http://www.mixtile.com/downloads/loft-q/mixtile-loftq-schematic.pdf


CPU
'''''''''

* ARM Cortex™-A7 Quad-Core
* 256KB L1 Cache
* 1MB L2 Cache

GPU
''''

* PowerVR™ SGX544MP2
* Complies with OpenGL ES2.0, OpenCL 1.x, DX 9_3

Video
''''''

* UHD H.264 4Kx2K video decoding
* Multi-format FHD video decoding, including Mpeg1/2, Mpeg4 SP/ASP GMC, H.263, H.264,etc.
* H.264 High Profile ``1080P@60fps`` encoding
* ``3840x1080@30fps`` 3D decoding, BD/SBS/TAB/FP supported
* ``3840x1080@30fps`` 3D encoding
* Complies with RTSP, HTTP, HLS, RTMP, MMS streaming media protocol

Display
''''''''

* Integrated Parallel & MIPI I/F sensor
* Integrated powerful ISP, supporting Raw Data CMOS sensor
* Supports 5M/8M/12M CMOS sensor
* Supports 8/10/12-bit YUV/Bayer sensor

Wifi & Bluetooth
''''''''''''''''

* AP6234 2.4G/5G Wifi & BT4.0
* 802.11 a/g/n support

Zigbee
'''''''

* NXP JN5168 pow power zigbee chip

Misc
'''''

* 2GB64-bit DDR3
* Onboard 8GB emmc
* HDMI supports ``1080p@60Hz``
* toslink plug support
* Gigabit ethernet port
* High defination Microphone support
* 2.5-inch SATA III hard disk interface
* 4xUSB2.0 HUB support
* Motion detection (onboard acceleration sensor)
* Available 180-PIN expansion connector
* remote support


