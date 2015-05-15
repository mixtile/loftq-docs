.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=========================
LOFT-Q 主流系统快速指南
=========================
:组织: Mixtile Team 
:编写: Phil.Han <phil.han@focalcrest.com>
:版本: 0.2
:日期: 2015.04.23

LOFT-Q 是 Mixtile 项目的第二代原型板，基于全志 A31 芯片，主要面向嵌入式开发人员，工程师，创客，以及极客，可以作为家庭影音中心，个人云存储设备，NAS，等。该指南主要用于向开发人员介绍如何快速的根据自己的需要搭建合适的开发环境，并基于 LOFT-Q 定制自己的应用。

目前主流的内核和Uboot已经对 A31 系列芯片很多功能和特性提供了支持，虽然在显示驱动，wifi和蓝牙，以及电源管理等方面还存在缺陷，但是基本的功能和以太网驱动已经能够很好的支持，为了和旧版的内核和系统进行区分，也为了更好的描述主流内核和Uboot的支持情况和构建方法，我们将原有的快速指南进行了拆分，将主流内核及系统的支持独立出来，方便开发者和爱好者参考。

主流 Uboot 构建
----------------------

目前 uboot 对于全志系列芯片的支持已经取得了很大的进展，对于 A31 系列芯片的支持已经非常完善。


主流 Uboot 代码获取
'''''''''''''''''''''''''

loftq-uboot-next 下载指令如下：

.. code-block:: sh  
   
   git clone https://github.com/mixtile/loftq-uboot-next.git

主流 Uboot 代码构建
'''''''''''''''''''''''''

loftq-uboot-next 的构建指令如下：

.. code-block:: sh  
   
   cd loftq-uboot-next

   make CROSS_COMPILE=arm-linux-gnueabi- mixtile_loftq_defconfig

   make CROSS_COMPILE=arm-linux-gnueabi-

在构建完成后，在 loftq-uboot-next 目录中生成 u-boot-sunxi-with-spl.bin 文件。


主流 Linux 构建
----------------------

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

在主流内核代码构建完成之后将会生成如下我们所需要的文件和目录：

* `arch/arm/boot/zImage`，主流内核文件 。
* `arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb`，主流内核所需的 dts 配置文件。
* `output/lib/modules`，主流内核对应的内核模块。

对于上述生成的内核，dts文件，以及内核模块，我们需要将它们分别添加到 GNU/Linux 系统 rootfs 指定路径，对于不同的系统可能会有所不同，当然也可以根据自己的需要进行配置。

Debian 构建
------------------

Debian 衍生出了很多 GNU/Linux 系统，目前 LOFT-Q 上也可以运行这一系统，只要做一些基本的配置就可以实现让 Debian 在 LOFT-Q 上运行。

文件准备
''''''''''''''''''''''''''

对于 Debian 的构建，根据我们所使用的内核不同，需要不同的准备文件。

全志内核依赖配置文件：

* script.bin: 全志平台专用的配置文件，用于内核和uboot的配置，由 sys_config.fex 生成。

主流内核依赖配置文件：

* sun6i-a31-mixtile-loftq.dtb: 主流内核启动所需要的 dts 配置文件，在内核构建时会自动生成该文件。

其他两者都需要的文件：

* boot.scr: UBoot 可加载执行文件，主要包含 UBoot 启动内核之前所需要执行的一系列指令和配置，由 boot.cmd 生成，对于全志内核和主流内核，两者会有很大不同。
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
   
* 添加 debian 源，在 **/chroots/sid-armhf/etc/apt/sources.list** 中添加如下内容：

  .. code-block:: sh   
      
     deb http://ftp.debian.org/debian sid main
     deb-src http://ftp.debian.org/debian sid main


script.bin 生成
''''''''''''''''''''''''

script.bin 是全志平台所用到的UBoot和内核配置文件，您可以在如下地址获取到最新的文件：

* script.bin 获取地址：https://github.com/mixtile/loftq-build/tree/master/bsp-legacy/binary/boot

或者可以使用下述指令来生成 script.bin 文件。

.. code-block:: sh   

   cd loftq-build/bsp/configs
  
   ./bin/fexc sys_config.fex script.bin

上述指令生成的 script.bin 即为可用的配置文件。

boot.scr 生成
''''''''''''''''''''''''

boot.scr 是 UBoot 可以加载和执行的指令文件，主要用于执行加载内核以及执行内核之前的一些配置。当然，该文件也可以直接使用已生成的文件。

* 预编译 boot.scr: https://github.com/mixtile/loftq-build/tree/master/bsp/binary/boot .
* 如果根据自己的需要进行定制，具体的定制方法可以参考 `boot.cmd 规则说明`_ 在定制完成之后可以使用如下命令：

  .. code-block:: sh   

     cd loftq-build/bsp/configs

     mkimage -C none -A arm -T script -d boot_single.cmd boot.scr

  .. note:: 
   
    * 对于全志旧版内核，boot.scr 配置位于： **loftq-build/bsp-legacy/configs/configs/boot_single.cmd**
    * 对于主流内核，boot.scr 配置位于： **loftq-build/bsp/configs/boot_single.cmd**


boot.cmd 规则说明
^^^^^^^^^^^^^^^^^^^^^^^^

boot.cmd 是用于生 boot.scr，即 uboot 可执行文件的源文件，我们以上述主流内核所用的 boot_single.cmd 文件为例，其内容如下：

.. code-block:: sh  

   bootdelay=3
   console=ttyS0,115200
   mmc_root=/dev/mmcblk0p1
   init=/sbin/init
   loglevel=8

   setenv bootargs noinitrd console=${console} init=${init} loglevel=${loglevel} vmalloc=${vmalloc} \
         partitions=${partitions} root=${mmc_root} ${extra}

   ext4load mmc 0 0x46000000 boot/mainline/zImage
   ext4load mmc 0 0x48000000 boot/mainline/sun6i-a31-mixtile-loftq.dtb
   bootz 0x46000000 - 0x48000000


其中关键的内容如下：

* **bootdelay=3** ，指定 uboot 在启动后，在正式加载内核启动前，有3秒中的倒计时，用于用户可能选择的操作，比如进入 uboot，类似于 PC 启动时进入 BIOS 时的选择。
* **console=ttyS0,115200**，指定开机后控制台登入端口和参数，这里指定开机后将 **ttyS0** 指定为控制台入口，并且其符号率设置为 **115200** 。
* **mmc_root** ，指定系统引导的 rootfs 所在的分区。
* **init=/sbin/init** ，指定系统初始化文件。
* **loglevel=8** ，指定系统日志等级。
* **setenv bootargs ...** ，这部分用于正式的将我们前几项设为 uboot 的环境变量。
* **ext4load mmc 0 0x46000000 boot/mainline/zImage** ，加载内核到指定的内存地址，为了统一放置主流内核固件文件，我们将内核放置在根分区的 **/boot/mainline/** 目录下，用户可以自行修改，通常放置在 */boot*。
* **ext4load mmc 0 0x48000000 boot/mainline/sun6i-a31-mixtile-loftq.dtb** ，加载 dts 文件到指定的内存地址，dts 二进制文件的位置与 **zImage** 相同，尽量放在相同的位置。
* **bootz 0x46000000 - 0x48000000** ，系统引导指令，从指定位置运行内核，并传递 dts 配置文件地址到内核。

对于旧版的内核，我们的配置文件会有所不同，主要不同部分内容如下：

.. code-block:: sh  

   ext4load mmc 0 0x43000000 boot/script.bin
   ext4load mmc 0 0x48000000 boot/uImage
   bootm 0x48000000

不同部分说明：

* **ext4load mmc 0 0x43000000 boot/script.bin**，这里我们加载的是全志的内核配置文件，script.bin。
* **ext4load mmc 0 0x48000000 boot/uImage**，我们使用了 uImage 而非 zImage，两者的主要差异可以查看 http://stackoverflow.com/questions/22322304/image-vs-zimage-vs-uimage 。
* **bootm 0x48000000**，在此我们仅需要知名 kernel 的内存位置，旧版内核会默认加载相应位置的 script.bin 配置。

下载并构建 loftq-uboot-next
'''''''''''''''''''''''''''''

主流 Uboot 构建，可以参考 `主流 Uboot 构建`_ 。

linux 内核及模块构建
''''''''''''''''''''''

对于 Linux 内核及模块构建，可以参考 `主流 Linux 构建`_ 。
  
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


对于 SD/emmc 的分区，我们需要进行如下的设置。我们假定所使用的 sd 卡设备为 **/dev/mmcblk0** 。

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

.. note::

  **/chroot/sid-armhf** 是指我们在 `获取 Debian rootfs`_ 中所指定的 rootfs 路径。

拷贝 BSP 文件到 ext4 分区
'''''''''''''''''''''''''''''''

在完成 rootfs 的拷贝之后，我们需要将 BSP 相关文件拷贝到指定的位置。

主流内核 BSP 文件拷贝
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* **boot.scr** 文件拷贝到 **/mnt/boot/** 。
* **linux/arch/arm/boot/zImage** 文件拷贝到 **/mnt/boot/mainline/** 。
* **linux/arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb** 文件拷贝到 **/mnt/boot/mainline/** 。
* **linux/output/lib/modules** 目录拷贝到 **/mnt/lib** 。

总体的参考命令如下：

.. code-block:: sh

   sudo cp boot.scr /mnt/boot
   sudo cp linux/arch/arm/boot/zImage /mnt/boot/mainline
   sudo cp linux/arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb /mnt/boot/mainline
   sudo cp -r linux/output/lib/modules /mnt/lib
   sudo sync
   sudo umount /mnt

旧版内核 BSP 文件拷贝
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* **boot.scr** 文件拷贝到 **/mnt/boot/** 。
* **linux/arch/arm/boot/uImage** 文件拷贝到 **/mnt/boot/** 。
* **script.bin** 文件拷贝到 **/mnt/boot** 。
* **linux/output/lib/modules** 目录拷贝到 **/mnt/lib** 。

总体的参考命令如下：

.. code-block:: sh

   sudo cp boot.scr /mnt/boot
   sudo cp loftq-linux/arch/arm/boot/uImage /mnt/boot
   sudo cp script.bin /mnt/boot
   sudo cp -r loftq-linux/output/lib/modules /mnt/lib
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
