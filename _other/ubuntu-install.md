# System Environment Variables

    cat /etc/environment
    vim /etc/environment
    source /etc/environment

# Apache

## install

    sudo apt-get install apache2

## restart

    sudo service apache2 restart

## uninstall

    sudo service apache2 stop
    sudo update-rc.d -f apache2 remove
    sudo apt-get remove apache2

# Nginx

## install

    sudo apt-get install nginx
    sudo service nginx start

## reload

    sudo service nginx reload

## uninstall

    sudo service nginx stop
    sudo update-rc.d -f nginx remove
    sudo apt-get remove nginx

# Mysql

## install

    sudo apt-get install mysql-server mysql-client

# PHP

## install

    sudo apt-get install php5 libapache2-mod-php5
    sudo apt-get install php5-fpm

## PHP extension

- php_bz2
- php_curl
- php_mbstring
- php_exif
- php_gd2
- php_gettext
- php_mysql
- php_mysqli
- php_openssl
- php_pdo_mysql
- php_pdo_sqlite
- php_soap
- php_sockets
- php_sqlite3
- php_xmlrpc
- php_xsl

# Nodejs and NPM

## install

    sudo apt-get install nodejs
    sudo apt-get install npm

# Redis and MongoDB

## install

    sudo apt-get install mongodb mongodb-server mongodb-client
    sudo apt-get install redis-server redis-tools

# Open JDK

    sudo apt-get install openjdk-8-jdk openjdk-8-doc openjdk-8-source

# Oracle Java

- download java from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- unzip package
- export JAVA_HOME=/home/ubuntu/java/jdk1.7
- export PATH=$JAVA_HOME/bin:$PATH

# Hadoop

- jdk, maven, ant, cmake, gcc, g++, protobuf, findbugs
- download hadoop
- mvn clean package -Pdist,native -DskipTests -Dtar
