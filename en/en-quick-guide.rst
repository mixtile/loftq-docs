.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=====================
LOFT-Q Quick Guide
=====================
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

Build Environment
----------------------

The board has burned with the latest stable os when you get it. And if you want to customize the board add your own external devices, you will have to rebuild your own system image, then you will have to make some preparation as below.

Source Architecture
''''''''''''''''''''''''''''

The main source of LOFT-Q as below:

* documents: https://github.com/mixtile/loftq-docs
* build: https://github.com/mixtile/loftq-build
* uboot: https://github.com/mixtile/loftq-uboot
* linux: https://github.com/mixtile/loftq-linux
* buildroot: https://github.com/mixtile/buildroot
* android: https://bitbucket.org/Mixtile/loftq-android
* uboot-next: https://github.com/mixtile/loftq-uboot-next

Building Environment 
''''''''''''''''''''''''''''

Now we can follow steps as below,  download different parts of the code, and build the developing environment.

* Create building folder

  Follow the following instructions to make root folder.

  .. code-block:: sh

    mkdir loftq
    cd loftq

* Download code repositories

  As previous description, we will have to download the kernel, buildroot, uboot, etc. it will need some time to download all the code repositories.

  .. code-block:: sh

     git clone https://github.com/mixtile/loftq-build.git
     git clone https://github.com/mixtile/loftq-uboot.git
     git clone https://github.com/mixtile/loftq-linux.git
     git clone https://github.com/mixtile/buildroot.git
     git clone https://github.com/mixtile/loftq-uboot-next

About loftq-build
''''''''''''''''''''''''''''

loftq-build contains the srcipts and tools for building uboot, linux, android, and packing system image.

Before accurately building, we will have to import the building environments to current working command line.

.. code-block:: sh

  source loftq-build/sunxi_env.sh

After environment importing, we can start compiling instructions, such as building uboot for linux:

.. code-block:: sh

  linux_build_uboot


About sunxi_env
''''''''''''''''''''''''''''

sunxi_env.sh is the env importing script for LOFT-Q, working as the lunch script for Android. it will import environment variables and compiling instructions to current working shell.

At the front, it's the definations of the repository paths for uboot, linux, buildroot and android, developers could make some modifitations according to their own configurations. The following the the original configs:

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

And developers can also add their own compiling instructions, sample as *linux_build_uboot* :

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


Uboot Building
-----------------------

As for building of uboot, we have two methods. we can build it with predefined building instruction in sunxi_env and follow the manual commands step by step.

Predefined building
''''''''''''''''''''''''''''''''''''''''

we have predefined building instructions uboot for android and linux.

* Uboot for Linux

  .. code-block:: sh

    linux_build_uboot


* Uboot for Android

  .. code-block:: sh

    android_build_uboot

Manually building
'''''''''''''''''''''''''''''''''''''''''

Manually building is just the seperate instructions of predefined instruction. Commands as below:

.. code-block:: sh

  make distclean
  make sun6i_config
  make -j4

Legacy Linux Kernel Building
--------------------------------

.. note:: 
  
  As for the Legacy linux kernel, we advice it to be used only for Android system.

Now the kernel we use is based on the cuszomized version by Allwinner for A31 soc, whihc is linux 3.3 . this version contains the drivers and configurations for both android and common linux releases, and we can compile kernel for both GNU/Linux and Android.

 
Predefined building
''''''''''''''''''''''''''''''''''''''''

loftq-build also provides predefined instructions for kernel building.

* kernel for GNU/Linux

  .. code-block:: sh

    linux_build_kernel

* kernel for Android

  .. code-block:: sh

    android_build_kernel

Manually building
''''''''''''''''''''''''''''''''''''''

Manually building for kernel will be a little complex, which needs external toolchain support.

* toolchain importing

  .. code-block:: sh

    export SUNXI_TOOLS_PATH=$(pwd)/loftq-build
    export SUNXI_TOOLCHAIN_PATH=$SUNXI_TOOLS_PATH/toolschain/gcc-linaro/bin/
    export PATH=$PATH:$SUNXI_TOOLCHAIN_PATH

  Developers can add these variables according to own path definations.

* kernel building for GNU/Linux

  .. code-block:: sh

    cd loftq-linux
    make distclean
    ./build.sh -p sun6i

* kernel building for Android

  .. code-block:: sh
    
    cd loftq-linux
    make distclean
    ./build.sh -p sun6i_fiber

Mainline Linux kernel building
----------------------------------

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

When building completed, we can add the kernel images, dts and referring modules to referring paths of GNU/Linux rootfs to make it bootable.

Buildroot building
---------------------------

Buildroot is a simple, efficient and easy-to-use tool to generate embedded Linux systems through cross-compilation. 

Getting source
'''''''''''''''''''''''''''''

Now we have the latest buildroot support, developers can download the code from our github repository, which will bee updated with the official repository.


.. code-block:: sh

  git clone https://github.com/mixtile/buildroot.git

Compiling buildroot
'''''''''''''''''''''''''''''''''''''''''''

After downloading source, we can begin compiling buildroot.

.. code-block:: sh

  make mixtile_loftq_defconfig
  make

.. note:: 

  * When building with make, it will try to downlaod all the code of package configured, also some temp files will be generated, so before building, it's necessary to prepare enough disk space according to number of packages, or it may cause building failed.
  * after building finished, there will be **output/images** at the root building directory.
  
Packing image
''''''''''''''''''''''''''''''

This step is packing image, we will have to put `rootfs.ext4` generated in previous step into the specified directory **$BUILD_TRUNK/out/linux** and then run cmds:
  
.. code-block:: sh

  linux_pack
  
As for linux image packing, it will show one prompt session as below:

.. code-block:: sh

  nano@vps:~/mixtile/loftq$ linux_pack
  Generating linux out directory!
  Copiing uboot!
  Copying linux kernel and modules!
  Packing final image!
  Start packing for Lichee system

  All valid chips:
  0. sun6i
  Please select a chip:

then enter **0** for choosing **sun6i**, the next session for os :

.. code-block:: sh

  All valid platforms:
  0. android
  1. dragonboard
  2. linux
  Please select a platform:

Now, we use linux, so enter **2** for **linux**, and then:

.. code-block:: sh

  All valid boards:
  0. aw_w01
  1. aw_w02
  2. evb
  3. loftq
  4. loftq_suse
  5. qc
  Please select a board:
  
we use **loftq**, so enter **3**, maybe the options list will vary with time. so choose the specified option according to your request. and then it will continue packing according to your selections, screen info as below:

.. code-block:: sh
 
  ....

  c:\bat
  c:\magic.bin
  find magic !! 
  RealLen=0x5A5C00
  CPlugin Free lib 
  CPlugin Free lib 
  get rootfs from ../../../out/linux
  compute signature for datafile /home/nano/mixtile/loftq/loftq-build/pack/out/boot.fex
  /home/nano/mixtile/loftq/loftq-build/pack/pctools/linux/eDragonEx/
  /home/nano/mixtile/loftq/loftq-build/pack/out
  Begin Parse sys_partion.fex
  Add partion bootloader.fex BOOTLOADER_FEX00
  Add partion very bootloader.fex BOOTLOADER_FEX00
  FilePath: bootloader.fex
  FileLength=5a5c00 FileSizeHigh=0
  Add partion env.fex ENV_FEX000000000
  Add partion very env.fex ENV_FEX000000000
  FilePath: env.fex
  FileLength=20000 FileSizeHigh=0
  Add partion boot.fex BOOT_FEX00000000
  Add partion very boot.fex BOOT_FEX00000000
  FilePath: boot.fex
  FileLength=dac800 FileSizeHigh=0
  Add partion rootfs.fex ROOTFS_FEX000000
  Add partion very rootfs.fex ROOTFS_FEX000000
  FilePath: rootfs.fex
  FileLength=8327800 FileSizeHigh=0
  BuildImg 0
  Dragon execute image.cfg SUCCESS !
  ---------image is at-------------

   /home/nano/mixtile/loftq/loftq-build/pack/sun6i_linux_loftq.img

  pack finish


**/home/nano/mixtile/loftq/loftq-build/pack/sun6i_linux_loftq.img** is the target image that we need. And next, we can burn this image to sdcard for installing or booting with **PhoenixCard**.

.. note:: 

  **PhoenixCard** is the tool provided by Allwinner for burning sdcard for booting or factory installing, which is only for windows platform. so we can't fully be free, and with mainline kernel or uboot, we can fly without this tool ;) , but for now, we still need this.
  
Customizing
''''''''''''''''''''''''''''''

We have preadded packages  in configuration file, also developers can add or delete them according to requirement. Commands as below:

.. code-block:: sh

  make menuconfig

**menuconfig** needs some extra libs or commands support, it you have been warned something missed, you can install them according to the prompting infomation.

More Information
'''''''''''''''''''''''''

Sites of Buildroot:

* Official site: http://buildroot.uclibc.org
* Documents: http://buildroot.uclibc.org/docs.html

Debian building
-----------------------

Debian is the base system for Many GNU/Linux systems, Now it could also run on LOFT-Q with some simple configurations.

Requirements
'''''''''''''''''''''''''''

Before building Debian, we will have some files needed:

* script.bin: the configuration file for Allwinner soc, used for setting up kernel and uboot, generated from sys_config.fex.
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

* prebuilt boot.scr: https://github.com/mixtile/loftq-build/tree/master/bsp/binary/boot .
* make boot.scr:

  .. code-block:: sh   

     cd loftq-build/bsp/configs

     mkimage -C none -A arm -T script -d boot_single.cmd boot.scr


Build u-boot-sunxi-with-spl.bin
''''''''''''''''''''''''''''''''''

we can download uboot code as below:

.. code-block:: sh  
   
   git clone https://github.com/mixtile/loftq-uboot-next.git

we can build it as below:

.. code-block:: sh  
   
   cd loftq-uboot-next

   make CROSS_COMPILE=arm-linux-gnueabi- mixtile_loftq_defconfig

   make CROSS_COMPILE=arm-linux-gnueabi-

when making finished, it will create **u-boot-sunxi-with-spl.bin**, and this is the file we need.


Build Linux kernel and its modules
''''''''''''''''''''''''''''''''''''''

we have introduced the kernel building process, please refer to `Mainline Linux kernel building`_ . 

Partion SD card
'''''''''''''''''''

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

2. start partioning.

   .. code-block:: sh

      sudo fdisk /dev/mmcblk0

3. process partioning.


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


Android building
-------------------------

The android source is based on **Android 4.4.2** provided by Allwinner with drivers for bluetooth, wifi, spdif, inner disk and more. The source tarball needs really much space that we can't afford it with one single code repository, so later, we will release latest code archive in our site. And for now we have one repository for Android code, because of its big body, only readable for downloading.

Building steps as below:

* enter android directory.

  .. code-block:: sh
   
    cd android
 
* build uboot for android with predefined instruction.

  .. code-block:: sh
   
    android_build_uboot

* build kernel for android with predefined instruction.

  .. code-block:: sh

    android_build_kernel
     
* build android project.
  
  .. code-block:: sh

    source build/envsetup.sh
    lunch mars_loftq-eng
    android_extract_bsp
    make -j16
    android_pack 


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
 
All you have to do is `Partion SD card`_ and `Burn Uboot to SD/Emmc`_ as described above, and decompress the tarball to the Ext4 partion, then the Linux world is open.

