# LAMP

## Install

    sudo apt-get install apache2 mysql-server mysql-client

    sudo apt-get install php5 php5-dev libapache2-mod-php5
    
    sudo apt-get install php5-fpm
    
    sudo apt-get install php5-curl php5-gd php5-mcrypt php5-memcache php5-memcached php5-mysql php5-sqlite
    
    sudo apt-get install php5-mongo
    
    sudo apt-get install php5-xdebug

## Service

&gt;= 15.04

    sudo systemctl restart apache2

&lt;= 14.10

    sudo service apache2 restart


## Compile

### build
    apt-get install build-essential

### apache
    wget http://mirrors.hust.edu.cn/apache/httpd/httpd-2.2.31.tar.gz
    wget http://mirrors.hust.edu.cn/apache/apr/apr-1.5.2.tar.gz
    wget http://mirrors.hust.edu.cn/apache/apr/apr-util-1.5.4.tar.gz

    tar -zxvf apr-1.5.2.tar.gz
    tar -zxvf apr-util-1.5.4.tar.gz
    tar -zxvf httpd-2.2.31.tar.gz

    apr: ./configure
    apr: make
    apr: make install

    apr-util: ./configure --with-apr=/usr/local/apr
    apr-util: make
    apr-util: make install

    ./configure
    --prefix=/usr/local/apache
    --sysconfdir=/etc/httpd
    --enable-so
    --enable-ssl
    --enable-cgi
    --enable-rewrite
    --with-zlib
    --with-apr=/usr/local/apr
    --with-apr-util=/usr/local/apr
    --enable-modules=most
    
    httpd: ./configure --prefix=/usr/local/apache2 --sysconfdir=/etc/httpd --enable-so --enable-ssl --enable-cgi --enable-rewrite --with-zlib --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr --enable-modules=most
    httpd: make
    httpd: make install

    http-bin: /usr/local/apache2/bin/apachectl [start|stop|restart]
    http-www: /usr/local/apache2/htdocs/
    http-conf: /etc/httpd/

### php
    wget http://cn2.php.net/get/php-5.4.45.tar.gz/from/this/mirror

    ./configure
    --prefix=/usr/local/php
    --with-apxs2=/path/to/apache/bin/apxs
    --with-config-file-path=/path/to/php/etc
    --with-mysql=mysqlnd
    --with-mysqli=mysqlnd
    --with-pdo-mysql=mysqlnd
    --with-gettext
    --enable-mbstring
    --with-iconv
    --with-mcrypt
    --with-mhash
    --with-openssl
    --enable-bcmath
    --enable-soap
    --with-libxml-dir
    --enable-pcntl
    --enable-shmop
    --enable-sysvmsg
    --enable-sysvsem
    --enable-sysvshm
    --enable-sockets
    --with-curl
    --with-gd
    --with-zlib
    --enable-zip
    --with-bz2
    --without-sqlite3
    --without-pdo-sqlite
    --with-pear
    --with-freetype-dir
    --with-jpeg-dir
    --with-png-dir
    --enable-safe-mode
    --enable-inline-optimization
    --enable-gd-native-ttf
    --enable-pdo

    ./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache2/bin/apxs --with-config-file-path=/usr/local/php/etc --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-gettext --enable-mbstring --with-iconv --with-mcrypt --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-gd --with-zlib --enable-zip
    ./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache2/bin/apxs --with-config-file-path=/usr/local/php/etc --with-mysql=mysqlnd --with-mysqli=mysqlnd --enable-pdo --with-pdo-mysql=mysqlnd --with-gettext --enable-mbstring --with-iconv --with-mcrypt --with-mhash --with-openssl --enable-bcmath --enable-soap --with-libxml-dir --enable-pcntl --enable-shmop --enable-sysvmsg --enable-sysvsem --enable-sysvshm --enable-sockets --with-curl --with-gd --enable-gd-native-ttf --with-jpeg-dir --with-png-dir --with-zlib --enable-zip --without-sqlite3 --without-pdo-sqlite --with-pear --enable-inline-optimization
