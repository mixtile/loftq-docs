LOFT-Q Owncloud Guide
=============================

`ownCloud <https://owncloud.org>`_ is a free and open source choice for personal cloud storage, created by KDE developer `Frank Karlitschek <http://zh.wikipedia.org/wiki/Frank_Karlitschek>`_ in 2010, which is developed for replacing commercial cloud service. Owncloud is free for usage, but if you want to build your own cloud, some basic tools, service and basic configurations is needed.

LOFT-Q provides two Linux releases, Debian and OpenSUSE, and we can build our own cloud server based on them, more info please referr to `owncloud 8.0 <https://owncloud.org/eight>`_ 。

Owncloud for Debian 
----------------------

The Debian for LOFT-Q is based on SID reversion. please install and configure owncloud as below operation:

1. Install Owncloud

   .. code-block:: sh
	
      sudo apt-get install owncloud

2. More ...


3. More ...


Owncloud for openSUSE
-----------------------

1. Owncloud Installation

   .. code-block:: sh

      zypper addrepo http://download.opensuse.org/repositories/isv:ownCloud:community/openSUSE_Factory_ARM/isv:ownCloud:community.repo
      zypper refresh
      zypper install owncloud

2. Mariadb startup and configuration

   After installation, please set root password for Maria database as tips below:

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


   * Mariadb startup

     .. code-block:: sh
   
        rcmysql start

   * root password setup

     .. code-block:: sh

        mysqladmin -u root password 'new-password'

        mysqladmin -u root -h misibook password 'new-password'

      **'new-password'** is the root password we should set.

3. Apache2 startup and configuration

   .. note:: 

      As for openSUSE，it will install the 2.4 version of Apache2.

      As for this version, **authz_default** has been removed，and its freature is partly replaced and updated by **authz_core**. so we will have to replace **authz_default** with **authz_core** in  **/etc/sysconfig/apache2**.

      So, Finally the default modules loaded in **/etc/sysconfig/apache2** is:

      .. code-block:: sh

         APACHE_MODULES="php5 actions alias auth_basic authn_file authz_host authz_groupffile 
         authz_user authz_core autoindex cgi dir env expires include log_config mime negotiation 
         setenvif ssl socache_shmcb userdir  reqtimeout"

   After configuration modified, we need to startup Apache2：
 
   .. code-block:: sh

      rcapache2 start


4. owncloud first start

   After all the configuration finished, we just enter **http://192.168.1.102/owncloud** in browser, and feel free to enter our own world of cloud.

   .. note:: 

      	 is the url for owncloud, please enter this according to your own ip configuration.


Reference
---------------

* `owncloud documents <https://doc.owncloud.org/>`_
