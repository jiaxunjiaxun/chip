# System Environment Variables

    cat /etc/environment
    vim /etc/environment
    source /etc/environment

# APT

[APT How to](https://help.ubuntu.com/community/AptGet/Howto)

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

# Chrome

    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
    sudo apt-get update
    sudo apt-get install google-chrome
    sudo apt-get install google-chrome-beta
    sudo apt-get install google-chrome-unstable
