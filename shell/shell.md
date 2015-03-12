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
