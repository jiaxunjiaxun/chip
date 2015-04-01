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