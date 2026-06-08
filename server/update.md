# Update

https://ipcmen.com/

## YUM
```bash
# 列出所有可更新的软件清单命令
yum check-update

# 更新所有软件命令
yum update

# 仅安装指定的软件命令
yum install [package_name]

# 仅更新指定的软件命令
yum update [package_name]

# 列出所有可安裝的软件清单命令
yum list

# 删除软件包命令
yum remove [package_name]

# 查找软件包命令
yum search [keyword]

# 清除缓存命令
# 清除缓存目录下的软件包
yum clean packages
# 清除缓存目录下的 Headers
yum clean headers
# 清除缓存目录下旧的 headers
yum clean oldheaders
# 清除缓存目录下的软件包及旧的 headers
yum clean
# 等价于 yum clean packages + yum clean oldheaders
yum clean all
```

## DNF
``` bash
# 查看 DNF 包管理器版本
dnf –version

# 查看系统中可用的 DNF 软件库
dnf repolist

# 查看系统中可用和不可用的所有的 DNF 软件库
dnf repolist all

# 列出所有 RPM 包
dnf list

# 列出所有安装了的 RPM 包
dnf list installed

# 列出所有可供安装的 RPM 包
dnf list available

# 搜索软件库中的 RPM 包
dnf search [package_name]

# 查找某一文件的提供者
dnf provides /bin/bash

# 查看软件包详情
dnf info [package_name]

# 安装软件包
dnf install [package_name]

# 升级软件包
dnf update [package_name]

# 检查系统软件包的更新
dnf check-update

# 升级所有系统软件包
dnf update
dnf upgrade

# 删除软件包
dnf remove [package_name]
dnf erase [package_name]

# 删除无用孤立的软件包
dnf autoremove

# 删除缓存的无用软件包
dnf clean all

# 获取有关某条命令的使用帮助
dnf help clean

# 查看所有的 DNF 命令及其用途
dnf help

# 查看 DNF 命令的执行历史
dnf history

# 查看所有的软件包组
dnf grouplist

# 安装一个软件包组
dnf groupinstall [package_group_name]

# 升级一个软件包组中的软件包
dnf groupupdate [package_group_name]

# 删除一个软件包组
dnf groupremove [package_group_name]

# 从特定的软件包库安装特定的软件
dnf –enablerepo=epel install [package_name]

# 更新软件包到最新的稳定发行版
dnf distro-sync

# 重新安装特定软件包
dnf reinstall [package_name]

# 回滚某个特定软件的版本
dnf downgrade [package_name]
```

## APT
``` bash
# 列出所有可更新的软件清单命令
sudo apt update

# 升级软件包
sudo apt upgrade

# 列出可更新的软件包及版本信息
apt list --upgradeable

# 升级软件包，升级前先删除需要更新软件包
sudo apt full-upgrade

# 安装指定的软件命令
sudo apt install [package_name]
sudo apt install [package_1] [package_2] [package_3]

# 更新指定的软件命令
sudo apt update [package_name]

# 显示软件包具体信息，例如：版本号，安装大小，依赖关系等等
sudo apt show [package_name]

# 删除软件包命令
sudo apt remove [package_name]

# 清理不再使用的依赖和库文件
sudo apt autoremove

# 移除软件包及配置文件
sudo apt purge [package_name]

# 查找软件包命令
sudo apt search [keyword]

# 列出所有已安装的包
apt list --installed

# 列出所有已安装的包的版本信息
apt list --all-versions
```

## OpenSSH

``` bash
# 检查配置
./configure --prefix=/usr/local/openssh --sysconfdir=/etc/ssh --with-pam --with-ssl-dir=/usr/local/openssl --with-md5-passwords --mandir=/usr/share/man --with-zlib=/usr/local/zlib --without-hardening
# 编译安装
make && make install
# 调整配置
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
echo "PubkeyAuthentication yes" >> /etc/ssh/sshd_config
echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
# 备份配置，调整部署
mv /usr/sbin/sshd /usr/sbin/sshd_bak
mv /etc/sysconfig/sshd /opt
mv /usr/lib/systemd/system/sshd.service /opt
\cp -arf /usr/local/openssh/sbin/sshd /usr/sbin/sshd
for i in $(rpm -qa |grep openssh);do rpm -e $i --nodeps ;done
mv /etc/ssh/sshd_config.rpmsave /etc/ssh/sshd_config
mv /etc/ssh/ssh_config.rpmsave /etc/ssh/ssh_config
mv /etc/ssh/moduli.rpmsave /etc/ssh/moduli
\cp -arf /usr/local/openssh/bin/* /usr/bin/
\cp -arf /usr/local/openssh/sbin/sshd /usr/sbin/sshd
cp /opt/openssh
cp ~/openssh-8.9p1/contrib/redhat/sshd.init /etc/init.d/sshd
chmod +x /etc/init.d/sshd
cp -a contrib/redhat/sshd.pam /etc/pam.d/sshd.pam
systemctl daemon-reload
service sshd restart
systemctl restart sshd
chkconfig --add sshd
chkconfig --level 2345 sshd on
chkconfig --list
```