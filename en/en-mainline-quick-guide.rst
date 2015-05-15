.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=======================================
LOFT-Q Quick Guide for mainline kernel
=======================================
:Organization: Mixtile Team 
:Author: Phil.Han <phil.han@focalcrest.com>
:Version: 0.1
:Date: 2015.01.23

|
|
|

.. image:: ../_static/pictures/loftq-board.png
  :width: 50%
  :align: center

|
|

LOFT-Q is the second prototyping board of Mixtile project. Based on AllWinner A31 SOC, designed for embedded developers, engineers, makers and hackers, it can be used as home media center, personal could device, NAS, etc. This guide will help developers quickly building the environment, compiling usable os and customizing you own application.

Mainline u-boot building
--------------------------------------

Now sunxi u-boot is heavily developed with the effort of sunxi community, and the support for A31 series is almostly completed.

Get mainline u-boot
'''''''''''''''''''''''''''''''''''''''''

.. code-block:: sh  
   
   git clone https://github.com/mixtile/loftq-uboot-next.git

Build mainline u-boot
'''''''''''''''''''''''''''''''''''''''''

.. code-block:: sh  
   
   cd loftq-uboot-next

   make CROSS_COMPILE=arm-linux-gnueabi- mixtile_loftq_defconfig

   make CROSS_COMPILE=arm-linux-gnueabi-

when making finished, it will create **u-boot-sunxi-with-spl.bin**, and this is the file we need.

Mainline kernel building
------------------------------------------

Now, the mainline linux kernel has partly supported A31 soc with the help of sunxi community and volunteers all around the world. If we are using the GNU/Linux system, we can use the mainline kernel with latest features.


Get Mainline kernel
''''''''''''''''''''''''''

LOFT-Q now has forked the mainline upstream kernel and add some customizations upon it. you can fork it from the github of Mixtile project.

.. code-block:: sh

  git clone https://github.com/mixtile/linux.git -b loftq-dev

**Note：** the `master` branch is the referring upstream branch, and `loftq-dev` is the customized branch.


Build Mainline kernel
''''''''''''''''''''''''''

After getting the code, we should follow commands below to build it.

.. code-block:: sh

  cp arch/arm/configs/mixtile_loftq_defconfig .config

  make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-

  make INSTALL_MOD_PATH=output ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- modules_install

when building is finished, we can get the directories and files we needed:

* `arch/arm/boot/zImage`，mainline kernel image.
* `arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb`，the dts file for mainline kernel.
* `output/lib/modules`，the modules for mainline kernel.

As for the image, dts and modules, we should copy them to referring paths of GNU/Linux rootfs to make it bootable, also we can move them to other customized paths.

Debian building
-----------------------

Debian is the base system for Many GNU/Linux systems, Now it could also run on LOFT-Q with some simple configurations.

Requirements
'''''''''''''''''''''''''''

Before building Debian, we will have to prepare some files needed according to different kernel released.

kernel config binary for Allwinner customized kernel:

* script.bin: the configuration file for Allwinner soc, used for setting up kernel and uboot, generated from sys_config.fex.

dts config for Mainline kernel:

* sun6i-a31-mixtile-loftq.dtb: the dts needed by mainline kernel, when building kernel, it will be generated automatically.

other files for both kinds of kernel:

* boot.scr: could be loaded and executed by Uboot, contains configurations and commands for booting up Linux kernel, generated from boot.cmd.
* u-boot-sunxi-with-spl.bin: the Uboot binary file, built by mainline uboot provided by linux-sunxi community, This release is based on **next** branch of `u-boot-sunxi <http://git.denx.de/?p=u-boot/u-boot-sunxi.git;a=summary>`_.
* uImage and referring kernel modules: linux kernel and kernel modules of LOFT-Q.
* rootfs archive of debian: rootfs archive for ARM platform, which contains the base system and configs of GNU/Linux.

Get Debian rootfs
'''''''''''''''''''''''

We can follow below commands to get rootfs of Debian, more info, please refer to  https://wiki.debian.org/ArmHardFloatChroot .

* get the latest unstable armhf base system with **qemu-deootstrap** .
 
  .. code-block:: sh     
   
     qemu-debootstrap --no-check-gpg --arch=armhf sid /chroots/sid-armhf ftp://ftp.debian.org/debian/

  it will take a long time to getting the rootfs depending on your network environment.

* add source repository of Debian, please add following lines to **/chroots/sid-armhf/etc/apt/sources.list**:

  .. code-block:: sh   
      
     deb http://ftp.debian.org/debian sid main
     deb-src http://ftp.debian.org/debian sid main


Generate script.bin
'''''''''''''''''''''''''''

script.bin is the configuration file for Uboot and Linux kernel of Allwinner soc, and we could get or make it:

* prebuilt script.bin: https://github.com/mixtile/loftq-build/tree/master/bsp/binary/boot .
* make script.bin:

  .. code-block:: sh   

     cd loftq-build/bsp/configs
  
     ./bin/fexc sys_config.fex script.bin


Generate boot.scr
'''''''''''''''''''''''

boot.scr is the binary file which could be loaded and executed by UBoot for preparing booting environment and loading Linux kernel. we also can get or make following steps as below:

* prebuilt mainline boot.scr: https://github.com/mixtile/loftq-build/tree/master/bsp/binary/boot .
* make boot.scr:

  .. code-block:: sh   

     cd loftq-build/bsp/configs

     mkimage -C none -A arm -T script -d boot_single.cmd boot.scr

  .. note:: 
   
    * boot.scr for legacy kernel: **loftq-build/bsp-legacy/configs/configs/boot_single.cmd**
    * boot.scr for mainline kernel: **loftq-build/bsp/configs/boot_single.cmd**

boot.cmd rules guide
^^^^^^^^^^^^^^^^^^^^^^^^

boot.cmd used for generating boot.scr, the loadable and executable uboot script. The contants of boot_single.cmd for the mainline uboot script as below:

.. code-block:: sh  

   bootdelay=3
   console=ttyS0,115200
   mmc_root=/dev/mmcblk0p1
   init=/sbin/init
   loglevel=8

   setenv bootargs noinitrd console=${console} console=tty0 init=${init} loglevel=${loglevel} vmalloc=${vmalloc} partitions=${partitions} root=${mmc_root} rootwait rw rootfstype=ext4 panic=10 consoleblank=0 ${extra}

   ext4load mmc 0 0x46000000 boot/mainline/zImage
   ext4load mmc 0 0x48000000 boot/mainline/sun6i-a31-mixtile-loftq.dtb
   bootz 0x46000000 - 0x48000000

Statements parsing：

* **bootdelay=3**, specify the delaying time before uboot loading kernel, user could choose different operations, just like the choices of BIOS for PC.
* **console=ttyS0,115200**, the params for console device and baudrate, here we set console device to **ttyS0**, and baudrate as **115200**.
* **mmc_root**，the volumes that root file system is located.
* **init=/sbin/init**, the initial execute file/script after booting up.
* **loglevel=8**, the level for system logging.
* **setenv bootargs ...**, setup the parameters we specified as uboot environment params.
* **ext4load mmc 0 0x46000000 boot/mainline/zImage**, load specified kernel to memory, here we put mainline referring files to **/boot/mainline/** directory, and user could give some other paths, which is usually under `/boot` directory.
* **ext4load mmc 0 0x48000000 boot/mainline/sun6i-a31-mixtile-loftq.dtb**, load dts to memory, dts is needed by mainline kernel.
* **bootz 0x46000000 - 0x48000000**, run kernel and dts from the memory offset we specified previous steps.

As for legacy kernel, we will have different commands:

.. code-block:: sh  

   ext4load mmc 0 0x43000000 boot/script.bin
   ext4load mmc 0 0x48000000 boot/uImage
   bootm 0x48000000

Statements：

* **ext4load mmc 0 0x43000000 boot/script.bin**，the Allwinner customized kernel config file.
* **ext4load mmc 0 0x48000000 boot/uImage**，this is the binary format for u-boot, not zImage used by mainline kernel, more info about the difference, please refer to http://stackoverflow.com/questions/22322304/image-vs-zimage-vs-uimage.
* **bootm 0x48000000**，load legacy kernel from memory offset we specified above.

Build u-boot-sunxi-with-spl.bin
''''''''''''''''''''''''''''''''''

Please refer to previous `Mainline u-boot building`_ for generating  **u-boot-sunxi-with-spl.bin**.

Build Linux kernel and its modules
''''''''''''''''''''''''''''''''''''''

we have introduced the kernel building process, please refer to `Mainline kernel building`_ . 

Partition SD card
'''''''''''''''''''''''''

The layout of bootable SD card for LOFT-Q as below:

==========  =================  ==================================
start        size		   usage
==========  =================  ==================================
0		8KB		unused, for partition table etc.
8		24KB		Initial SPL loader
32		512KB		U-Boot
544		128KB		environment
672		352KB		reserved
1024		-		free for partitions
==========  =================  ==================================

As for partitions of SD/Emmc, we have to make it in host os (Linux), we take '/dev/mmcblk0' as example.

1. clear partitions of SD/Emmc, please backup your data before clearing.

   .. code-block:: sh  
     
      sudo dd if=/dev/zero of=/dev/mmcblk0 bs=1M count=1

2. start partitioning.

   .. code-block:: sh

      sudo fdisk /dev/mmcblk0

3. process partitioning.


   .. code-block:: sh
 
      Command (m for help): n                                 # Type n
      Partition type:
         p   primary (0 primary, 0 extended, 4 free)
         e   extended
      Select (default p):                                     # Press Enter Key      
      Using default response p
      Partition number (1-4, default 1):                      # Press Enter Key 
      Using default value 1
      First sector (2048-15523839, default 2048):             # Press Enter Key 
      Using default value 2048
      Last sector, +sectors or +size{K,M,G} (2048-15523839, default 15523839):      # Press Enter Key 

      Command (m for help): w                                   # Enter w and press Enter

      The partition table has been altered!

4. format partition as Ext4.

   .. code-block:: sh
    
      sudo mkfs.ext4 /dev/mmcblk0p1


Burn Uboot to SD/Emmc
'''''''''''''''''''''''''

After partitions prepared, we can burn **u-boot-sunxi-with-spl.bin** to SD card.

.. code-block:: sh
 
   sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblk0 bs=1024 seek=8

Prepare rootfs for SD card
''''''''''''''''''''''''''''

.. code-block:: sh
	
   sudo mount -t ext4 /dev/mmcblk0p1 /mnt
   
   sudo cp -r /chroot/sid-armhf/* /mnt

.. note::

  **/chroot/sid-armhf** is the rootfs path we prepared in `Get Debian rootfs`_ .


Copy BSP files to ext4 partition
'''''''''''''''''''''''''''''''''''''

Copy Mainline BSP files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* **boot.scr** to **/mnt/boot/** 。
* **linux/arch/arm/boot/zImage** to **/mnt/boot/mainline/** 。
* **linux/arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb** to **/mnt/boot/mainline/** 。
* **linux/output/lib/modules** to **/mnt/lib** 。

Sample commands:

.. code-block:: sh
   
   sudo cp boot.scr /mnt/boot
   sudo cp linux/arch/arm/boot/zImage /mnt/boot/mainline
   sudo cp linux/arch/arm/boot/dts/sun6i-a31-mixtile-loftq.dtb /mnt/boot/mainline
   sudo cp -r linux/output/lib/modules /mnt/lib
   sudo sync
   sudo umount /mnt

After we finished all the steps above, now we can enter the Debian world, just pluging sd card to LOFT-Q and power it on.

Ubuntu building
-----------------------

ubuntu support is still under working, please wait ...

OpenSUSE 
---------------------

openSUSE is one of the best GNU/Linux distribution in the world, which is from German. Now we can boot up and try openSUSE JeOS for ARM with customized Uboot and Linux kernel.

JeOS rootfs
'''''''''''''''''''''''''''''''''

There are several versions of JeOS that can work on LOFT-Q.

* Factory: http://download.opensuse.org/ports/armv7hl/factory/images/
* 13.1: http://download.opensuse.org/ports/armv7hl/distribution/13.1/appliances/
* 12.3: http://download.opensuse.org/ports/armv7hl/distribution/12.3/images/

We can choose one version for testing. More info about different versions, please refer to https://en.opensuse.org/HCL:Chroot .

Here we take 13.1 version as testing example. then we have to download the **openSUSE-*-ARM-JeOS.armv7-rootfs-*.tbz** tarball. As for the bootable SD card makeing process, it's almose the same with `Debian building`_, and the only difference is **rootfs** creating and copying. 

Now we creating rootfs with **openSUSE-Factory-ARM-JeOS.armv7-rootfs.armv7l-Current.tbz**, orders as below:

.. code-block:: sh

   sudo tar -C /mnt -xjf openSUSE-Factory-ARM-JeOS.armv7-rootfs.armv7l-Current.tbz

Now, we follow `Copy BSP files to ext4 partition`_ to add BSP and kernel modules, then we just plug in SD card to LOFT-Q and power it on, Here is the openSUSE world.


Miscellaneous
------------------

GNU/Linux Archives
'''''''''''''''''''''

We have precompiled tarballs of openSUSE and Debian linux system for booting up linux quickly. 

* Debian sid release
* openSUSE 13.1 release
* openSUSE Factory release

Link: http://www.mixtile.com/downloads/loft-q/
 
All you have to do is `Partition SD card`_ and `Burn Uboot to SD/Emmc`_ as described above, and decompress the tarball to the Ext4 partion, then the Linux world is open.

