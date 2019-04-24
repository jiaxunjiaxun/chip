# Shell

## Convert character set

Convert character set of file

~~~ shell
for i in `find ./ -name "*.php"`
do
    echo $i;
    iconv -c -f gb18030 -t utf8 $i -o /tmp/convert.tmp;
    mv /tmp/convert.tmp $i;
done
rm /tmp/convert.tmp
~~~

## Fetch Page

~~~ shell
curl --retry [retry times] --retry-delay [delay seconds] --retry-max-time [retry seconds] [url] -o [file]
wget -t [retry times] -w [delay seconds] -T [retry seconds] [url] -O [file]
~~~

## Kill process

~~~ shell
ps -ef | grep what_you_want | grep -v grep | cut -c 9-15 | xargs kill -9
~~~

## Check Linux version

~~~ shell
cat /etc/redhat-release
# Or
lsb_release -a
~~~

## Clean old kernel

**Note**: kernel version, do not remove all

~~~ shell
# [CentOS]
# check current kernel
uname -r
# list all kernel
rpm -q kernel
# remove unused kernel
rpm -e kernel-xxxxxx
~~~

## Yum update

~~~ shell
yum clean all
yum update glibc* yum* rpm* python*
yum update
~~~

## Apt update

~~~ shell
apt-get update
apt-get dist-upgrade
apt-get upgrade
apt-get autoremove
apt-get autoclean
# after 16.04
apt update
apt dist-upgrade
apt upgrade
apt autoremove
apt autoclean
~~~

## Tar, gzip and zip

### tar package

~~~ shell
tar -cvf /path/to/dest/pkg.tar /path/to/src
~~~

### tar gzip package

~~~ shell
tar -zcvf /path/to/dest/pkg.tgz /path/to/src
tar --exclude /path/to/src/exclude -zcvf /path/to/dest/pkg.tar.gz /path/to/src
~~~

### tar bz2 package

~~~ shell
tar -jcvf /path/to/dest/pkg.tar.bz2 /path/to/src
~~~

### list tgz package

~~~ shell
tar -ztvf /path/to/src/pkg.tar.gz
~~~

### tar unwrap to current path

~~~ shell
tar -zxvf /path/to/src/pkg.tar.gz
~~~

### tar unwrap part of package to current path

~~~ shell
tar -zxvf /path/to/src/pkg.tar.gz part/in/package
~~~

### gzip

~~~
-c target zip file
-d unzip file
-l list zip file
-r recursive
~~~

### zip and unzip

~~~
-r recursive
# zip -r zip_file directory
-v list zip file
-d target path
~~~

## netstat

~~~
-a (all)显示所有选项，默认不显示LISTEN相关
-t (tcp)仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化成数字。
-l 仅列出有在 Listen (监听) 的服務状态
-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命令。
~~~

### security check

~~~ shell
# check tcp port
netstat -alt

# check changelog [CentOS]
rpm -qi --changelog <package name> | grep CVE-
~~~

### mount nfs

[ref](http://www.tutorialspoint.com/unix_commands/mount.htm)

~~~ shell
mkdir -p /path/to/mount

showmount -e [ip address]

mount -t nfs -o rw -v [nfs ip address]:/path/to/dst /path/to/mount
mount -t cifs -o username=[user],password=[pwd] //[cifs ip address]/path/to/dst /path/to/mount
~~~

### generate password

[ref](https://www.cyberciti.biz/faq/linux-random-password-generator/)

1. /dev/urandom file – Linux kernel’s random number generator source/interface. A read from the /dev/urandom device will not block waiting for more entropy.
2. tr command – Translate or delete characters. Used to remove unwanted characters from /dev/urandom.
3. head command – Output the first part of files.
4. xargs command – build and execute command lines from standard input/pipe.

~~~ shell
genpasswd() {
    local l=$1
        [ "$l" == "" ] && l=16
        tr -dc A-Za-z0-9_ < /dev/urandom | head -c ${l} | xargs
}

genpasswd() {
    strings /dev/urandom | grep -o '[[:alnum:]]' | head -n 14 | tr -d '\n'; echo
}
~~~

~~~ shell
# Change password
passwd root
~~~