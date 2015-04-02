LOFT-Q Owncloud 说明
=============================

`ownCloud <https://owncloud.org>`_ 是一个自由且开源的个人云存储解决方案，最初由 KDE 开发者 `Frank Karlitschek <http://zh.wikipedia.org/wiki/Frank_Karlitschek>`_ 于2010年一月创建，目标是替代商业的云服务提供商。对于 owncloud, 用于可以自由免费的获取，但是自行架设该服务器，则需要一些基本的安装和配置。

LOFT-Q 提供了 Debian 和 OpenSUSE 两个 Linux 系统，对于它们，我们可以使用如下的方法来配置属于自己的 owncloud 专属云服务器，所有的安装和配置基于 `owncloud 8.0 <https://owncloud.org/eight>`_ 。

Debian 配置
-------------------

对于 Debian SID 版本安装方法如下：

.. code-block:: sh
	
   sudo apt-get install owncloud


openSUSE 配置
---------------------

1. 安装 Owncloud:

   .. code-block:: sh

      zypper addrepo http://download.opensuse.org/repositories/isv:ownCloud:community/openSUSE_Factory_ARM/isv:ownCloud:community.repo
      zypper refresh
      zypper install owncloud

2. 启动和配置 Maria DB

   .. code-block:: sh

      Message from package mariadb:

      You just installed MySQL server for the first time.

      You can start it using:
       rcmysql start

      During first start empty database will be created for your automatically.

      PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
      To do so, start the server, then issue the following commands:

      '/usr/bin/mysqladmin' -u root password 'new-password'
      '/usr/bin/mysqladmin' -u root -h misibook password 'new-password'

      Alternatively you can run:
      '/usr/bin/mysql_secure_installation'

      which will also give you the option of removing the test
      databases and anonymous user created by default.  This is
      strongly recommended for production servers.

   根据上述提示执行如下命令：

   * 启动 MySQL 服务器

     .. code-block:: sh
   
        rcmysql start

   * 设置数据库 root 密码

     .. code-block:: sh

        mysqladmin -u root password 'new-password'

        mysqladmin -u root -h misibook password 'new-password'

      **'new-password'** 是根据需要设置的密码.

3. 设置和启动 Apache2

   .. note:: 

      对于 openSUSE，目前存在一个问题，其默认安装的 apache2, 版本为2.4, 该版本的 **authz_default** 模块已经被移除，并且其中相应功能由 **authz_core** 所替换。因此我们需要在 **/etc/sysconfig/apache2** 中移除 **authz_default** 而将其替换为 **authz_core**。

      最终，在 **/etc/sysconfig/apache2** 中的默认模块加载配置为：

      .. code-block:: sh

         APACHE_MODULES="php5 actions alias auth_basic authn_file authz_host authz_groupffile 
         authz_user authz_core autoindex cgi dir env expires include log_config mime negotiation 
         setenvif ssl socache_shmcb userdir  reqtimeout"

   参照上述说明修改完配置之后，需要执行下述命令重新启动 apache2 服务：
 
   .. code-block:: sh

      rcapache2 start


4. 初始化设置 owncloud

   在完成上述的全部配置之后，在浏览器中输入 **http://192.168.1.102/owncloud** 开始探索和配置自己的私人专属云服务器。

   .. note:: 
      
      **http://192.168.1.102/owncloud** 是 owncloud 的网络地址，需要根据不同的 IP 地址进行修改。


更多参考
---------------

* `owncloud 文档 <https://doc.owncloud.org/>`_
