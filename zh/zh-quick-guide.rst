.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=================
LOFT-Q 快速指南
=================
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

LOFT-Q 是 Mixtile 项目的第二代原型板，基于全志 A31 芯片，主要面向嵌入式开发人员，工程师，创客，以及极客，可以作为家庭影音中心，个人云存储设备，NAS，等。该指南主要用于向开发人员介绍如何快速的根据自己的需要搭建合适的开发环境，并基于 LOFT-Q 定制自己的应用。


环境准备
-----------


在进行正式的 LOFT-Q 使用之前，如果在板子中已经烧录了最新稳定的系统镜像，则可以直接使用。如果需要根据自己的需要对硬件进行重新配置，或者添加一些硬件接口，需要重新构建系统镜像，需要执行如下的准备工作。

LOFT-Q 代码
'''''''''''''

LOFT-Q 项目目前的代码主要包含一下几个部分：

* build: https://github.com/mixtile/loftq-build
* uboot: https://github.com/mixtile/loftq-uboot
* linux: https://github.com/mixtile/loftq-linux
* buildroot: https://github.com/mixtile/buildroot
* android: https://bitbucket.org/Mixtile/loftq-android
* uboot-next: https://github.com/mixtile/loftq-uboot-next

后续，我们会提供更多的有关新系统移植的进展及下载地址。

代码下载
'''''''''''''

您可以通过下述命令来下载各个部分的代码，并且完成构建环境搭建。

* 创建构建目录
    
  通过下面的指令创建用于 LOFT-Q 项目的目录：

  .. code-block:: sh

    mkdir loftq
    cd loftq


* 下载代码

  您可以执行下述命令来下载各个部分代码, 对于 linux 内核和 buildroot 部分, 代码量比较大, 可能需要比较长的时间才能完成下载。

  .. code-block:: sh

    git clone https://github.com/mixtile/loftq-build.git
    git clone https://github.com/mixtile/loftq-uboot.git
    git clone https://github.com/mixtile/loftq-linux.git
    git clone https://github.com/mixtile/buildroot.git
    git clone https://github.com/mixtile/loftq-uboot-next


关于 loftq-build
''''''''''''''''''

loftq-build 包含用于完成 uboot， linux , android, 以及后续添加代码的构建指令, 以及 sunxi 的一部分打包相关工具。 

在执行具体的构建指令之前，我们需要执行如下指令，在当前的命令行环境下导入相关的构建指令：

.. code-block:: sh

  source loftq-build/sunxi_env.sh


之后，我们可以执行后续的构建指令，下述是构建 linux 版 uboot 的指令：

.. code-block:: sh

  linux_build_uboot


更多说明
''''''''''''''''''

sunxi_env.sh 类似于 android 的 lunch 配置文件。用于向当前终端中导入可用环境变量以及一些指令。

sunxi_env.sh 的开头部分定义了 uboot, linux, 以及 buildroot, android 等的路径，用户可以根据自己的需要进行修改，所有的可配置环境变量如下：

.. code-block:: sh

  export BUILD_TRUNK=$(pwd)
  export BUILD_TRUNK_OUT=$BUILD_TRUNK/out
  
  # envs for sunxi tools
  export SUNXI_TOOLS_PATH=$(pwd)/loftq-build
  export SUNXI_LINUX_PATH=$(pwd)/loftq-linux
  export SUNXI_UBOOT_PATH=$(pwd)/loftq-uboot
  export SUNXI_TOOLCHAIN_PATH=${SUNXI_TOOLS_PATH}/toolschain/gcc-linaro/bin/
  
  # envs for android
  export ANDROID_TRUNK=$(pwd)/android
  export ANDROID_DEVICE=loftq
  export ANDROID_DEVICE_TRUNK=${ANDROID_TRUNK}/device/mixtile/${ANDROID_DEVICE}
  
  # envs for ubuntu touch 
  # only used if we have android base sdk released by ubuntu touch team
  # Note: now we can build this image but can't burn it to disk with PhoenixTool
  export UBUNTU_OUTPUT=$BUILD_TRUNK_OUT/ubuntu
  export UBUNTU_TARBALL=$UBUNTU_OUTPUT/vivid-preinstalled-touch-armhf.tar.gz
  
  # commom env
  export ANDROID_OUT=${ANDROID_TRUNK}/out
  export ANDROID_DEVICE_OUT=${ANDROID_OUT}/target/product/${ANDROID_DEVICE}
  
  export LINARO_GCC_PATH=$SUNXI_TOOLCHAIN_PATH
  export PATH=$PATH:$LINARO_GCC_PATH


用户可以在其中添加自定义的指令，以 *linux_build_uboot* 为例：

.. code-block:: sh

  function linux_build_uboot()
  {
      CURDIR=$PWD
  
      cd $SUNXI_UBOOT_PATH
      make distclean
      make sun6i_config
      make -j4
      cd $CURDIR
  }


可以参照上述格式进行添加。


UBoot 构建
------------

基于 LOFT-Q 环境编译
''''''''''''''''''''''

目前可以使用 loftq-build 中的工具实现对 uboot 的快速编译。

对于构建适用于 Linux 和 Android 的构建指令分别如下：

* 构建适用于 Linux 的 Uboot.

  .. code-block:: sh

    linux_build_uboot


* 构建适用于 Android 的 Uboot.

  .. code-block:: sh

    android_build_uboot


手动定制编译
''''''''''''''

目前使用的 Uboot 除了可以使用构建工具进行编译之外, 还可以在 Uboot 目录下进行编译，相应的指令如下：

.. code-block:: sh

  make distclean
  make sun6i_config
  make -j4


Linux 旧版内核构建
--------------------

.. note:: 
  
  对于 Linux 旧版内核，建议仅用于 Android 系统。

对于全志的 linux 内核, 目前使用的是 linux 3.3 版本。目前这一版本的内核中统一了 linux 和 android 版本的内核。可以在同一份内核代码中完成对 Linux 和 android 内核的分开编译。

基于 LOFT-Q 环境的编译
'''''''''''''''''''''''

目前可以使用 loftq-build 中的工具实现对 linux 内核的快速编译。下述指令分别实现对标准 linux 内核和 android 定制内核的编译。

* 标准 linux 内核编译

  .. code-block:: sh

    linux_build_kernel

* 定制 android 内核编译

  .. code-block:: sh

    android_build_kernel


手动定制编译
'''''''''''''''''''''''''''

如果用户需要直接在 loftq-linux 中使用 linux 的 Makefile 进行内核的编译，需要完成如下工作。

* 导入 sunxi 的 linaro-gcc 编译工具链

  .. code-block:: sh

    export SUNXI_TOOLS_PATH=$(pwd)/loftq-build
    export SUNXI_TOOLCHAIN_PATH=$SUNXI_TOOLS_PATH/toolschain/gcc-linaro/bin/
    export PATH=$PATH:$SUNXI_TOOLCHAIN_PATH

  对于上述的环境变量，需要根据自己的路径进行正确的配置。

* 标准 linux 内核编译

  .. code-block:: sh

    make distclean
    ./build.sh -p sun6i

* 定制 android 内核编译

  .. code-block:: sh

    make distclean
    ./build.sh -p sun6i_fiber


Linux 主流内核构建
------------------

由于 sunxi 社区以及全球志愿者的努力和支持，目前主流内核已经提供了对全志 A31 的部分驱动支持，建议在使用 GNU/Linux 系统时，使用主流的 Linux 内核，可以使用最新的内核的一些特性。


主流内核代码获取
'''''''''''''''''''''''''

目前 LOFT-Q 项目实时跟进 Linux 最新代码的进度，提供了一些配置文件的定制和更新。最新的代码已经托管在 Mixtile 项目的 github 仓库。相关代码获取指令如下


.. code-block:: sh

  git clone https://github.com/mixtile/linux.git -b loftq-dev

**备注：** 我们的 master 分支为主流分支，而定制分支为 `loftq-dev`。


主流内核代码构建
''''''''''''''''''''''''''''

在获取了上述的代码之后，需要执行如下过程来构建主流内核代码：

.. code-block:: sh

  cp arch/arm/configs/mixtile_loftq_defconfig .config

  make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

  make INSTALL_MOD_PATH=output ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- modules_install

在编译完成主流代码之后，我们可以将生成的内核，dts文件，以及内核模块添加到 GNU/Linux 系统 rootfs 的制定路径，需要根据自己的需要配置相应的 GNU/Linux 系统，如 Debian, OpenSUSE 等。

Buildroot 构建
---------------

Buildroot 是非常易于使用，定制，和构建的一套编译工具系统，便于开发人员快速的构建基于 Linux 内核的嵌入式操作系统。

获取源码
''''''''''''

我们参照全志提供的较老的 buildroot 配置方法, 目前已经适配了最新的 buildroot 。目前开发人员可以从我们的 Github 项目代码库中下载最新的 buildroot 代码：

.. code-block:: sh

  git clone https://github.com/mixtile/buildroot.git


我们会定期对 buildroot 进行更新以方便于使用最新的软件。

编译构建
''''''''''''

在下载完 buildroot 源代码之后，可以使用下述命令完成对项目的编译和构建。

.. code-block:: sh

  make mixtile_loftq_defconfig
  make


.. note:: 

  * 在执行 make 指令时, 将会从相关站点下载最新的代码, 同时在构建时将会编译生成很多临时文件, 因此应根据自己编译是选择的包的数量, 准备一定空余的磁盘空间, 否则可能会因为没有足够的空间而造成构建失败。
  * 在构建完成之后，将会在 output/images 目录中生成 rootfs.ext4 镜像，如果生成该文件，则表示构建成功。

镜像打包
''''''''''''

在完成对 buildroot 的构建之后，将生成的 rootfs.ext4 文件拷贝到指定的目录，即 *$BUILD_TRUNK/out/linux/*。之后执行下述指令：

.. code-block:: sh

  linux_pack


在执行完命令后将会打印出目标镜像的路径。则该文件就是 sunxi PhoenixTool 可以进行烧录的文件，可以烧录为卡启动，或者卡量产模式。

自我定制
'''''''''''''''''''

如果您需要根据自己的需要，添加或者删除指定的软件库，可以参照如下说明进行定制。

.. code-block:: sh

  make menuconfig


执行上述指令时，可能会提示缺少某些库，以至于无法完成操作，可以根据错误提示信息，安装制定的系统软件。

更多信息
''''''''''''

* buildroot 官网: http://buildroot.uclibc.org
* buildroot 文档: http://buildroot.uclibc.org/docs.html


Android 构建使用
---------------------------

LOFT-Q 原型板的 Android 源码基于全志原厂提供的 Android 4.4.2 版本源码提供，增添了 LOFT-Q 相关的蓝牙, Wifi 部分驱动, 固件, 外接硬盘, USB, 红外遥控等的配置文件。由于 Android 源码过于庞大，我们提供了可供下载的压缩包，并未提供源码库，并且在源码中去除了提交日志等内容。

Android 的构建，同样可以使用类似于上述 Linux 和 buildroot 的基于 LOFT-Q 环境的编译和手动定制编译两种方式。

基于 LOFT-Q 环境构建 Android
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

基于 LOFT-Q 环境构建 Android 的指令如下：

1. 进入 android 源码目录

   .. code-block:: sh

     cd android

2. 构建 android 用 Uboot

   .. code-block:: sh

     android_build_uboot

3. 构建 android 用 Linux 内核
 
   .. code-block:: sh

     android_build_kernel

4. 构建 Android 项目

   .. code-block:: sh

     source build/envsetup.sh
     lunch mars_loftq-eng
     android_extract_bsp
     make -j16
     android_pack

Debian 构建
------------------

Debian 衍生出了很多 GNU/Linux 系统，目前 LOFT-Q 上也可以运行这一系统，只要做一些基本的配置就可以实现让 Debian 在 LOFT-Q 上运行。

文件准备
''''''''''''''''''''''''''

对于 Debian 的构建，我们需要准备如下文件：

* script.bin: 全志平台专用的配置文件，用于内核和uboot的配置，由 sys_config.fex 生成。
* boot.scr: UBoot 可加载执行文件，主要包含 UBoot 启动内核之前所需要执行的一系列指令和配置，由 boot.cmd 生成。
* u-boot-sunxi-with-spl.bin: 基于 linux-sunxi 社区提供的 mainline uboot 定制的用于 LOFT-Q 的 UBoot 代码，该版本基于 `u-boot-sunxi <http://git.denx.de/?p=u-boot/u-boot-sunxi.git;a=summary>`_ 的 **next** 分支。
* uImage 及内核模块: LOFT-Q 的 linux 内核及模块文件。
* rootfs 压缩包: ARM 平台的系统 rootfs 所需要的根文件系统压缩包，包含一系列 GNU/Linux 系统用的基础系统和配置。


获取 Debian rootfs 
'''''''''''''''''''''''''''''''''''''''

Debian 的根文件系统可以使用如下方式获取，更多信息可以参考 https://wiki.debian.org/ArmHardFloatChroot 。

* 使用 qemu-debootstrap 获取最新的 armhf 基础系统。
   
  .. code-block:: sh     
   
     qemu-debootstrap --no-check-gpg --arch=armhf sid /chroots/sid-armhf ftp://ftp.debian.org/debian/
   
  指令的执行可能需要比较长的时间。
   
* 添加 debian 源，在 /chroots/sid-armhf/etc/apt/sources.list 中添加如下内容：

  .. code-block:: sh   
      
     deb http://ftp.debian.org/debian sid main
     deb-src http://ftp.debian.org/debian sid main


script.bin 生成
''''''''''''''''''''''''

script.bin 是全志平台所用到的UBoot和内核配置文件，您可以在如下地址获取到最新的文件：

* script.bin 获取地址：https://github.com/mixtile/loftq-build/tree/master/bsp/binary/boot

或者可以使用下述指令来生成 script.bin 文件。

.. code-block:: sh   

   cd loftq-build/bsp/configs
  
   ./bin/fexc sys_config.fex script.bin

上述指令生成的 script.bin 即为可用的配置文件。

boot.scr 生成
''''''''''''''''''''''''

boot.scr 是 UBoot 可以加载和执行的指令文件，主要用于执行加载内核以及执行内核之前的一些配置。当然，该文件也可以直接使用已生成的文件，如果根据自己的需要进行定制，在定制完成之后可以使用如下命令，生成该文件。

.. code-block:: sh   

   cd loftq-build/bsp/configs

   mkimage -C none -A arm -T script -d boot_single.cmd boot.scr

下载并构建 loftq-uboot-next
'''''''''''''''''''''''''''''

loftq-uboot-next 下载指令如下：

.. code-block:: sh  
   
   git clone https://github.com/mixtile/loftq-uboot-next.git

loftq-uboot-next 的构建指令如下：

.. code-block:: sh  
   
   cd loftq-uboot-next

   make CROSS_COMPILE=arm-linux-gnueabi- mixtile_loftq_defconfig

   make CROSS_COMPILE=arm-linux-gnueabi-

在构建完成后，在 loftq-uboot-next 目录中生成 u-boot-sunxi-with-spl.bin 文件。

linux 内核及模块构建
''''''''''''''''''''''

对于 Linux 内核及模块构建，可以参考 `Linux 主流内核构建`_ 。
  
SD 卡分区
'''''''''''''''''''''''''

对于可启动的 SD/EMMC，其分区如下：


==========  =================  =====================
起始地址        大小		   使用
==========  =================  =====================
0		8KB		未使用，用于分区表
8		24KB		用于 SPL
32		512KB		用于 U-Boot
544		128KB		环境变量
672		352KB		保留
1024		-		可用分区
==========  =================  =====================


对于 SD/emmc 的分区，我们需要进行如下的设置。我们假定所使用的 sd 卡设备为 `/dev/mmcblk0` 。

* 清除原有分区信息，请在执行该动作之前，保存SD卡中原有重要数据。

  .. code-block:: sh  
     
     sudo dd if=/dev/zero of=/dev/mmcblk0 bs=1M count=1

* 新建 ext4 分区

  .. code-block:: sh

     sudo fdisk /dev/mmcblk0

* 执行分区

  .. code-block:: sh
 
     Command (m for help): n                                 # 输入 n
     Partition type:
        p   primary (0 primary, 0 extended, 4 free)
        e   extended
     Select (default p):                                     # 按下 Enter       
     Using default response p
     Partition number (1-4, default 1):                      # 按下 Enter
     Using default value 1
     First sector (2048-15523839, default 2048):             # 按下 Enter
     Using default value 2048
     Last sector, +sectors or +size{K,M,G} (2048-15523839, default 15523839):      # 按下 Enter

     Command (m for help): w                                   # 输入 w 并 按下 Enter

     The partition table has been altered!

* 格式化分区为 ext4 格式

  .. code-block:: sh
    
     sudo mkfs.ext4 /dev/mmcblk0p1

SD 卡写入 U-Boot
''''''''''''''''''''''''

在完成分区的划分之后，执行下述指令向 SD/EMMC 中写入 U-Boot 文件。

.. code-block:: sh
 
   sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblk0 bs=1024 seek=8

拷贝 rootfs 到 SD 卡的 ext4 分区
'''''''''''''''''''''''''''''''''''''

.. code-block:: sh
	
   sudo mount -t ext4 /dev/mmcblk0p1 /mnt
   
   sudo cp -r /chroot/sid-armhf/* /mnt

拷贝 BSP 文件到 ext4 分区
'''''''''''''''''''''''''''''''

.. code-block:: sh

   sudo cp boot.scr /mnt/boot

   sudo cp linux/arch/arm/boot/zImage /mnt/boot/mainline

   sudo cp linux/arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb /mnt/boot/mainline

   sudo cp -r linux/output/lib/modules /mnt/lib

   sudo sync

   sudo umount /mnt

在完成上述步骤之后，将 SD 卡接入 LOFT-Q SD卡座，连接电源，然后就可以进入 Debian 的世界。

Ubuntu 构建
------------------

Ubuntu 系统支持还在进行中...

OpenSUSE 使用
---------------

openSUSE 是来自德国的 Linux 发行版, 目前我们可以使用定制的 Uboot 和 Linux 内核来实现对 openSUSE ARM 版 JeOS 最简系统的引导和使用。

JeOS rootfs 下载
''''''''''''''''''

openSUSE 社区提供了多个版本的 JeOS rootfs 可供下载，如下：

* Factory 版本: http://download.opensuse.org/ports/armv7hl/factory/images/
* 13.1 版本: http://download.opensuse.org/ports/armv7hl/distribution/13.1/appliances/
* 12.3 版本: http://download.opensuse.org/ports/armv7hl/distribution/12.3/images/


我们可以从中选择一个版本的进行测试使用, 更多有关各个版本之间的异同信息, 可以参阅 https://en.opensuse.org/HCL:Chroot 。

以 13.1 版本的 openSUSE Jeos 根文件系统为例，我们需要下载名称如 **openSUSE-*-ARM-JeOS.armv7-rootfs-*.tbz** 的镜像文件，对于可启动 SD 卡的创建，其步骤和需要的文件与 `Debian 构建`_ 相同。只是在 rootfs 的生成和拷贝过程存在不同，其他过程按照 Debian 构建过程进行即可。

那么对于 rootfs 到 ext4 分区的拷贝，我们以 **openSUSE-Factory-ARM-JeOS.armv7-rootfs.armv7l-Current.tbz** 为例，所需执行指令如下：


.. code-block:: sh

   sudo tar -C /mnt -xjf openSUSE-Factory-ARM-JeOS.armv7-rootfs.armv7l-Current.tbz

在执行完该步骤之后，参照 `Debian 构建`_ 的步骤，在相应位置添加 BSP 及内核模块文件。

更多说明
----------------

Linux 系统
'''''''''''''''

为了方便快速的在 LOFT-Q 上使用 GNU/Linux 系统，我们为 LOFT-Q 提供了预编译了内核，启动配置文件的 Linux 系统压缩包，列表如下：

* Debian sid 版本
* openSUSE 13.1 版本
* openSUSE Factory 版本

链接地址: http://www.mixtile.com/downloads/loft-q/

您只需要参照 `SD 卡分区`_ 和 `SD 卡写入 U-Boot`_ 完成分区和 U-Boot 烧录后，将上述的压缩文件解压到 Ext4 分区即可。




