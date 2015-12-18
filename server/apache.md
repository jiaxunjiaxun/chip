# Apache Configure

## Virtual Host

Reference [2.2](http://httpd.apache.org/docs/2.2/vhosts/examples.html) [2.4](http://httpd.apache.org/docs/2.4/vhosts/examples.html)

    <VirtualHost *:80>
        ServerAdmin webmaster@example.com
        DocumentRoot /path/to/webapp
        ServerName demo.example.com
        
        # no limit
        ErrorLog logs/demo.neoease.com-error.log
        CustomLog logs/demo.neoease.com-access.log common
        # by size 1M
        ErrorLog "|/path/to/rotatelogs -l logs/site-error-%Y-%m-%d.log 1M"
        CustomLog "|/path/to/rotatelogs -l logs/site-access-%Y-%m-%d.log 1M" common
        CustomLog "|/path/to/rotatelogs -l logs/site-access-%Y-%m-%d.log 1M" combined
        # by date
        ErrorLog "|/path/to/rotatelogs -l logs/site-error-%Y-%m-%d.log 86400"
        CustomLog "|/path/to/rotatelogs -l logs/site-access-%Y-%m-%d.log 86400" common
        CustomLog "|/path/to/rotatelogs -l logs/site-access-%Y-%m-%d.log 86400" combined
    </VirtualHost>

## Directory

    <Directory /path/to/webapp>
        Options FollowSymLinks
        AllowOverride None
        Order deny,allow
        Deny from all
    </Directory>
    <Directory "/path/to/webapp">
        Options -Indexes FollowSymLinks
        AllowOverrride None
        Order allow,deny
        Allow from all
    </Directory>

## Cross Domain Header

### Load Library

    LoadModule headers_module modules/mod_headers.so

### Add Configure

    <!-- File Type -->
    <FilesMatch "\.(ttf|otf|eot|woff)$">
        <IfModule mod_headers.c>
            Header set Access-Control-Allow-Origin "*"
        </IfModule>
    </FilesMatch>

    Header add Access-Control-Allow-Origin "*"
    Header add Access-Control-Allow-Methods "GET, POST, OPTIONS"
    <!-- *** -->
    Header add Access-Control-Allow-Headers "Content-Type"

    <!-- *** -->
    <Directory "/path/to/resource">
        SetEnvIf Origin "http(s)?://(www\.)?(example1\.com|example2\.com)$" AccessControlAllowOrigin=$0$1
        Header add Access-Control-Allow-Origin %{AccessControlAllowOrigin}e env=AccessControlAllowOrigin
        Header set Access-Control-Allow-Methods "GET, POST, OPTIONS"
    </Directory>

    <!-- *** -->
    SetEnvIf Origin "http(s)?://(example\.com|example\.org)$" AccessControlAllowOrigin=$0
    Header add Access-Control-Allow-Origin %{AccessControlAllowOrigin}e env=AccessControlAllowOrigin

## GZip Header

### Load Library

    LoadModule deflate_module modules/mod_deflate.so
    LoadModule headers_module modules/mod_headers.so

### Add Configure

    <IfModule mod_deflate.c>
        SetOutputFilter DEFLATE
        # Don’t compress images and other
        SetEnvIfNoCase Request_URI .(?:gif|jpe?g|png)$ no-gzip dont-vary
        SetEnvIfNoCase Request_URI .(?:exe|t?gz|zip|bz2|sit|rar)$ no-gzip dont-vary
        SetEnvIfNoCase Request_URI .(?:pdf|doc)$ no-gzip dont-vary
        
        AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css
        AddOutputFilterByType DEFLATE application/x-javascript
    </IfModule>

## Tomcat

    # httpd.conf
    LoadModule jk_module modules/mod_jk.so
    
    JkWorkersFile conf/workers.properties
    JkMountFile conf/uriworkermap.properties
    JkLogFile logs/mod_jk.log
    JkLogLevel warn
    
    # workers.properties
    # list the workers by name
    worker.list=DLOG4J, status
    # localhost server 1
    worker.s1.port=8109
    worker.s1.host=localhost
    worker.s1.type=ajp13
    # localhost server 2
    worker.s2.port=8209
    worker.s2.host=localhost
    worker.s2.type=ajp13
    worker.s2.stopped=1
    
    worker.DLOG4J.type=lb
    worker.retries=3
    worker.DLOG4J.balanced_workers=s1,s2
    worker.DLOG4J.sticky_session=1
    
    worker.status.type=status