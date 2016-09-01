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
    sudo service nginx restart
    sudo service nginx reload
    
### Test configure
    nginx -t -c /path/to/configure-file

### Install latest version
    sudo apt-get install python-software-properties
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:nginx/stable
    sudo apt-get update
    sudo apt-get install nginx

## PHP

    sudo apt-get install php5-fpm
    sudo apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

## Nginx conf

    [/etc/nginx/nginx.conf]
    
    user www-data;
    worker_processes 4;
    pid /run/nginx.pid;
    
    events {
        worker_connections 768;
        # multi_accept on;
    }
    
    http {
        ##
        # Basic Settings
        ##
        
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 10;
        types_hash_max_size 2048;
        client_max_body_size 8M;
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

---

    [/etc/nginx/sites-available/*]
    
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
        
        location ~ /\.ht {
            deny all;
        }
    }
