# Shell

## Convert character set

Convert character set of file

    #!/bin/sh
    
    for i in `find ./ -name "*.php"`
    do
        echo $i;
        iconv -c -f gb18030 -t utf8 $i -o /tmp/convert.tmp;
        mv /tmp/convert.tmp $i;
    done
    rm /tmp/convert.tmp

## Fetch Page

    curl --retry [retry times] --retry-delay [delay seconds] --retry-max-time [retry seconds] [url] -o [file]
    wget -t [retry times] -w [delay seconds] -T [retry seconds] [url] -O [file]

## Kill process

    ps -ef | grep what_you_want | grep -v grep | cut -c 9-15 | xargs kill -9

## Clear kernel

Note: kernel version, do not remove all

    rpm -q kernel
    rpm -e kernel-xxxxxx

## Apt update

    apt-get update
    apt-get upgrade
    apt-get autoremove
    apt-get autoclean

## PHP Pear upgrade

    pear upgrade-all

    cd path/to/tgz/
    unzip *.tgz
    pear upgrade *.tar

## Python update with pip

    pip install -U pip
    pip freeze --local | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U --allow-all-external

## Tar, gzip and zip

### tar package

    tar -cvf /path/to/dest/pkg.tar /path/to/src

### tar gzip package

    tar -zcvf /path/to/dest/pkg.tgz /path/to/src
    tar --exclude /path/to/src/exclude -zcvf /path/to/dest/pkg.tar.gz /path/to/src

### tar bz2 package

    tar -jcvf /path/to/dest/pkg.tar.bz2 /path/to/src

### list tgz package

    tar -ztvf /path/to/src/pkg.tar.gz

### tar unwrap to current path

    tar -zxvf /path/to/src/pkg.tar.gz

### tar unwrap part of package to current path

    tar -zxvf /path/to/src/pkg.tar.gz part/in/package

### gzip

    -c target zip file
    -d unzip file
    -l list zip file
    -r recursive

### zip and unzip

    -r recursive

    -v list zip file
    -d target path

