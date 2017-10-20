# LNMP

> [Ref 1](http://linux.it.net.cn/Ubuntu/2016/0403/20991.html)\
> [Ref 2](https://www.zybuluo.com/happyfans/note/161734)\
> [Ref 3](http://blog.csdn.net/hguisu/article/details/8930668)\
> [Ref 4](http://nginx.org/en/linux_packages.html)

## MySQL

### APT
~~~ shell
sudo apt-get install mysql-server mysql-client
~~~

### YUM
~~~ shell
yum localinstall https://dev.mysql.com/get/mysql57-community-release-el6-9.noarch.rpm
yum update
yum install mysql-community-server mysql-community-client

service mysqld start

grep 'A temporary password' /var/log/mysqld.log | tail -1

mysql_secure_installation
~~~


## Nginx

~~~ shell
sudo service apache2 stop
sudo update-rc.d -f apache2 remove
sudo apt-get remove apache2
sudo apt-get install nginx

sudo service nginx start
sudo service nginx restart
sudo service nginx reload
~~~

### Test configure

~~~ shell
nginx -t -c /path/to/configure-file
~~~

### Install latest version

#### APT
~~~ shell
sudo apt-get install python-software-properties
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get install nginx
~~~

[ --- *OR* --- ]

~~~ shell
sudo apt-key add nginx_signing.key
sudo cd /etc/apt
sudo vim sources.list
~~~

> Debian\
> deb http://nginx.org/packages/debian/ [codename] nginx\
> deb-src http://nginx.org/packages/debian/ [codename] nginx
>
> deb http://nginx.org/packages/mainline/debian/ codename nginx\
> deb-src http://nginx.org/packages/mainline/debian/ codename nginx
>
> Ubuntu\
> deb http://nginx.org/packages/ubuntu/ [codename] nginx\
> deb-src http://nginx.org/packages/ubuntu/ [codename] nginx
>
> deb http://nginx.org/packages/mainline/ubuntu/ codename nginx\
> deb-src http://nginx.org/packages/mainline/ubuntu/ codename nginx

Debian
| Version | Codename | Supported Platforms
| ---     | ---      | ---
| 7.x     | wheezy   | x86_64, i386
| 8.x     | jessie   | x86_64, i386
| 9.x     | stretch  | x86_64, i386

Ubuntu
| Version | Codename | Supported Platforms
| ---     | ---      | ---
| 12.04   | precise  | x86_64, i386
| 14.04   | trusty   | x86_64, i386, aarch64/arm64
| 16.04   | xenial   | x86_64, i386, ppc64el, aarch64/arm64
| 16.10   | yakkety  | x86_64, i386

#### YUM
~~~ shell
cd /etc/yum.repos.d
vim nginx.repo
~~~

> [nginx]\
> name=nginx repo\
> baseurl=http://nginx.org/packages/[ OS ]/[ OSRELEASE ]/$basearch/\
> gpgcheck=0\
> enabled=1
---
> [nginx]\
> name=nginx repo\
> baseurl=http://nginx.org/packages/mainline/OS/OSRELEASE/$basearch/\
> gpgcheck=0\
> enabled=1

Replace "OS" with "rhel" or "centos", depending on the distribution used, and "OSRELEASE" with "6" or "7", for 6.x or 7.x versions, respectively.

~~~ shell
yum install nginx
~~~

## PHP Fast CGI

### APT

~~~ shell
sudo apt-get install php5-fpm
sudo apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
~~~

### YUM

~~~ shell
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

yum update
yum install php70w-fpm php70w-cli php70w-opcache php70w-gd php70w-mcrypt php70w-mysql php70w-xml php70w-mbstring php70w-pdo php70w-json
~~~

## Nginx conf

- /etc/nginx/nginx.conf
~~~ nginx
user www-data;
worker_processes 4;
pid /var/run/nginx.pid;

events {
    worker_connections 768;
    use epoll;
    # multi_accept on;
}

http {
    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    client_max_body_size 10M;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;

    ##
    # Gzip Settings
    ##

    gzip on;
    gzip_disable "msie6";

    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_types application/atom+xml application/javascript application/json application/rss+xml application/vnd.ms-fontobject application/x-font-ttf application/x-web-app-manifest+json application/xhtml+xml application/xml font/opentype image/svg+xml image/x-icon text/css text/plain text/xml text/javascript text/x-component;

    # optional
    gzip  on;
    gzip_min_length 1000;
    gzip_comp_level 4;
    gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

    open_file_cache max=655350 inactive=20s;
    open_file_cache_valid 30s;
    open_file_cache_min_uses 2;

    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}

#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
#
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}
#}
~~~

- /etc/nginx/sites-available/*

~~~ nginx
server {
    listen 80;
    listen [::]:80 ipv6only=on;

    root /var/www/html/discover;

    index index.php index.html index.htm;

    server_name your_domain;

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location = /robots.txt {
        allow all;
        log_not_found off;
        access_log off;
    }

    location ~* /(?:uploads|files)/.*\.php$ {
        deny all;
    }

    location / {
        # url rewrite
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ [^/]\.php(/|$) {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+?\.php)(/.*)$;

        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # index.php/xxx/xxx
    location ~ ^/index\.php(/|$) {
        try_files $uri =404;

        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;

        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param HTTPS off;
    }

    location ~ \.php$ {
        return 404;
    }
    # ---

    location ~ /\.ht {
        deny all;
    }
}
~~~
