.. header::
  .. image:: ../_static/pictures/mixtile-logo.png
    :align: left

=====================
LOFT-Q 旧版系统快速指南
=====================
:组织: Mixtile Team 
:编写: Phil.Han <phil.han@focalcrest.com>
:版本: 0.2
:日期: 2015.04.23

LOFT-Q 是 Mixtile 项目的第二代原型板，基于全志 A31 芯片，主要面向嵌入式开发人员，工程师，创客，以及极客，可以作为家庭影音中心，个人云存储设备，NAS，等。该指南主要用于向开发人员介绍如何快速的根据自己的需要搭建合适的开发环境，并基于 LOFT-Q 定制自己的应用。

对于全志 A31 系列芯片，目前主流内核已经提供了很多的支持，但在无线网卡，显示系统，电源管理等方面仍存在功能缺失等问题，这也是旧版内核目前所优于主流内核的方面。为了和基于主流内核的系统构建方法进行区分，现在将旧版内核各个方面的资料，构建方法等从原本的快速指南中取出来单独组成新的资料。

旧版环境准备
----------------

在进行正式的 LOFT-Q 使用之前，如果在板子中已经烧录了最新稳定的系统镜像，则可以直接使用。如果需要根据自己的需要对硬件进行重新配置，或者添加一些硬件接口，需要重新构建系统镜像，需要执行如下的准备工作。

对于旧版系统，其主要可以用于支持和构建如下系统：

* buildroot
* Android

我们需要准备的代码包括如下几个部分：

* 构建工具： https://github.com/mixtile/loftq-build
* 旧版内核： https://github.com/mixtile/loftq-linux
* 旧版uboot: https://github.com/mixtile/loftq-uboot
* android: https://bitbucket.org/Mixtile/loftq-android
* buildroot: https://github.com/mixtile/buildroot

对于上述的代码和工具，我们需要按如下的目录结构进行组织

.. code-block:: sh
  android
  loftq-build
  loftq-linux
  loftq-uboot

那么我们可以参照如下命令来搭建基本的构建环境：

  .. code-block:: sh

    mkdir loftq
    cd loftq

    git clone https://github.com/mixtile/loftq-build.git
    git clone https://github.com/mixtile/loftq-uboot.git
    git clone https://github.com/mixtile/loftq-linux.git
    git clone https://github.com/mixtile/buildroot.git

关于 loftq-build
''''''''''''''''''

loftq-build 主要用于旧版系统的构建和打包使用，包含用于完成 uboot， linux , android, 以及后续添加代码的构建指令, 以及 sunxi 的一部分打包相关工具。

在执行具体的构建指令之前，我们需要执行如下指令，在当前的命令行环境下导入相关的构建指令：

.. code-block:: sh

  source loftq-build/sunxi_env.sh


之后，我们可以执行后续的构建指令，下述是构建 linux 用的 uboot 的指令：

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

旧版 UBoot 构建
--------------------

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

旧版 Linux 内核构建
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
------------------------

LOFT-Q 原型板的 Android 源码基于全志原厂提供的 Android 4.4.2 版本源码提供，增添了 LOFT-Q 相关的蓝牙, Wifi 部分驱动, 固件, 外接硬盘, USB, 红外遥控等的配置文件。由于 Android 源码过于庞大，我们提供了可供下载的压缩包，并未提供源码库，并且在源码中去除了提交日志等内容。

Android 的构建，同样可以使用类似于上述 Linux 和 buildroot 的基于 LOFT-Q 环境的编译和手动定制编译两种方式。

基于 LOFT-Q 环境构建 Android
'''''''''''''''''''''''''''''''''''''

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

与 buildroot 最后的打包过程类似，在执行完命令后将会打印出目标镜像的路径。则该文件就是 sunxi PhoenixTool 可以进行烧录的文件，可以烧录为卡启动，或者卡量产模式。


