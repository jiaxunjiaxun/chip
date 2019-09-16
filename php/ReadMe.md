# Chip for code and design

## php

### install

~~~ shell
sudo apt-get install php5 libapache2-mod-php5
sudo apt-get install php5-fpm

sudo apt-get install php-fpm php-cli

[/etc/php5/fpm/php.ini]
cgi.fix_pathinfo=0
[/etc/php5/fpm/pool.d/www.conf]
listen=/var/run/php5-fpm.sock

sudo service php5-fpm restart
~~~

### PHP extension

    php_sockets php_xmlrpc php_xsl php_openssl php_curl php_soap
    php_mysql php_mysqli php_pdo_mysql php_pdo_sqlite php_sqlite3
    php_mbstring php_exif php_gd2 php_gettext php_bz2

### Ubuntu 14.04LTS PHP 5.6

[Ref](https://www.dev-metal.com/install-setup-php-5-6-ubuntu-14-04-lts/)

~~~ shell
sudo add-apt-repository ppa:ondrej/php5-5.6
sudo apt-get update
sudo apt-get install python-software-properties
sudo apt-get update
sudo apt-get install php5
~~~

### PHP Pear upgrade

~~~ shell
pear upgrade-all
~~~

---

~~~ shell
cd path/to/tgz/
unzip *.tgz
pear upgrade *.tar
~~~

### PHP CMS

[WordPress](https://wordpress.org/)\
[TYPO3](https://typo3.org/)\
[Joomla!](https://www.joomla.org/)\
[Drupal](https://www.drupal.org/)\
[Contao](https://contao.org/en/)

---

[Ref - 1](https://thecustomizewindows.com/2016/12/list-flat-file-cms-without-database/)

#### Static Generator
[List](https://www.staticgen.com/)\
[PHP Solution](https://sculpin.io/)\
[PHP Solution](https://www.statie.org/)
