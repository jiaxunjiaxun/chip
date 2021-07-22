# Firewall Service

[Firewall](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)
[Firewall](http://www.tuicool.com/articles/vMr6Vj)
[Firewall](https://note.xdq.me/centos-7-firewalldchang-yong-ming-ling/)
[Firewall](https://oracle-base.com/articles/linux/linux-firewall-firewalld)

[SSH](https://wiki.centos.org/HowTos/Network/SecuringSSH)
[SSH](http://linux.it.net.cn/CentOS/course/2016/0529/22705.html)
[SSH](http://www.cnblogs.com/nwf5d/archive/2011/03/28/1998002.html)

firewall-cmd
--permanent
--zone=public
--add-rich-rule="rule family="ipv4" source address="192.168.0.4/24" accept"

firewall-cmd --permanent --zone=public --add-rich-rule 'rule family="ipv4" source address="219.224.0.0/20" service name=ssh accept'
firewall-cmd --permanent --zone=public --remove-rich-rule 'rule family="ipv4" source address="219.224.0.0/20" service name=ssh accept'
firewall-cmd --reload

---

```shell
# List all
firewall-cmd --list-all

# About zone
firewall-cmd --get-zones
firewall-cmd --get-default-zone
firewall-cmd --set-default-zone=[zone name]
firewall-cmd --get-active-zones

# About interface
firewall-cmd --get-zone-of-interface=[net device name]
firewall-cmd --zone=work --add-interface=[net device name]
firewall-cmd --zone=block --change-interface=[net device name]

# About service
firewall-cmd --get-services
firewall-cmd --list-services

firewall-cmd --permanent --zone=public --add-service=ssh
firewall-cmd --permanent --zone=public --add-service=mysql
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --permanent --zone=public --add-service=ftp

firewall-cmd --permanent --zone=public --remove-service=ftp

# Reload~~
firewall-cmd --reload
```