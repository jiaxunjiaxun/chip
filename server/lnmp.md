# LNMP

[Ref](http://linux.it.net.cn/Ubuntu/2016/0403/20991.html)

## MySQL

    sudo apt-get install mysql-server mysql-client

## Nginx

    sudo service apache2 stop
    sudo update-rc.d -f apache2 remove
    sudo apt-get remove apache2
    sudo apt-get install nginx
    sudo service nginx start

## PHP

    sudo apt-get install php5-fpm
    sudo apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

## Nginx conf

    /etc/nginx/nginx.conf

