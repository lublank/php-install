# PHP安装与配置

下载地址：http://www.php.net/downloads.php

    # wget http://cn.php.net/distributions/php-7.1.1.tar.gz

**安装依赖libxml2**

    # apt-get install libxml2-dev

**配置、编译、安装**

    # ./configure --prefix=/usr/local/php --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-mysqlnd --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-opcache --enable-mbstring --enable-soap --enable-zip --enable-bcmath --with-openssl --with-zlib --with-curl --with-gd
    
    # make && make install 

**安装成功后创建配置文件**

    # cp php.ini-production /usr/local/php/etc/php.ini  #配置文件
    # cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf    #php-fpm配置文件
    # cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
    # cp sapi/fpm/php-fpm /usr/local/php/bin    #启动php-fpm的执行文件

**软连接**

    # ln -s /usr/local/php/bin/php /usr/bin/php #配置全局环境软连接
    # ln -s /usr/local/php/etc/php.ini /etc/php.ini #添加软链接到 /etc目录
    # ln -s /usr/local/php/etc/php-fpm.conf /etc/php-fpm.conf   #添加软连接到 /etc目录

**配置文件**
1、php.ini修改以下：
    
    cgi.fix_pathinfo=0
    
2、www.conf中修改用户和用户组（默认为nobody）
    
    user = www
    group = www

指定ini文件的目录

    # /usr/local/php/bin/php-fpm -c /usr/local/php/etc/php.ini


**启动 php-fpm 服务**

    # /usr/local/php/bin/php-fpm

**查看是否启动成功：**

    # netstat -lnt | grep 9000
    或
    # netstat -tunpl | grep 9000

**重启 php-fpm 服务**
    kill -USR2 `cat /usr/local/php/var/run/php-fpm.pid`

**关闭 php-fpm 服务**

    # pkill php-fpm
    或
    # kill -INT `cat /usr/local/php/var/run/php-fpm.pid`

**查看PHP版本**
    
    # php -v
    
**卸载PHP**
1.	apt-get安装的用 `apt-get purge 'php7*'`
2.	源文件编译安装的直接删掉安装目录


---

## 不重新编译安装PHP增加扩展的方法

linux上已安装php，但是发现安装后不支持某扩展，以openssl为例：

PHP安装目录为：`/usr/local/php/`

切换到php源码目录的 /usr/local/src/php-7.1.4/ext/openssl目录

将config0.m4改名为config.m4（有些扩展直接是config.m4则不需改名）

    # cp config0.m4 config.m4

准备PHP 扩展库的编译环境

    # /usr/local/php/bin/phpize

配置、编译、安装

    # ./configure –with-openssl -with-php-config=/usr/local/php/bin/php-config
    
    # make
    
    # make install

编译成功后会返回以下：

> Installing shared extensions: /usr/local/php/lib/php/extensions/no-debug-non-zts-20160303

进入文件夹将生成的文件移到ext下

    # cd /usr/local/php/lib/php/extensions/no-debug-non-zts-20160303
    # mv openssl.so /usr/local/php/etc/

编辑配置文件:

    # vi /usr/local/php/php.ini

在php.ini文件中添加扩展:

    extension=openssl.so

通过运行 `php --ini` 查找 php.ini 文件位置

重新加载配置:

    # /usr/local/php/sbin/php-fpm reload
    或
    # pkill php-fpm && /usr/local/php/bin/php-fpm

这样扩展就编译安装完成了~

查看安装了哪些扩展
    
    # php -m

常用的扩展：

    php7.0-mysql php7.0-curl php7.0-gd php7.0-intl php-pear php-imagick php7.0-imap php7.0-mcrypt php-memcache  php7.0-pspell php7.0-recode php7.0-sqlite3 php7.0-tidy php7.0-xmlrpc php7.0-xsl php7.0-mbstring php-gettext

redis扩展：
    
    php-redis

apt-get 安装：

    apt-get install php7.0 php7.0-fpm
    apt-get -y install php7.0-mysql php7.0-curl php7.0-gd php7.0-intl php-pear php-imagick php7.0-imap php7.0-mcrypt php-memcache  php7.0-pspell php7.0-recode php7.0-sqlite3 php7.0-tidy php7.0-xmlrpc php7.0-xsl php7.0-mbstring php-gettext php7.0-bcmath

